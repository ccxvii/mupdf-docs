# Multi-threaded

This example renders a PDF to PNG images using multiple threads.

```c
/*
Multi-threaded rendering of all pages in a document to PNG images.

First look at docs/example.c and make sure you understand it.
Then, before coming back here to see an example of multi-threading,
please read the multi-threading section in:
https://mupdf.readthedocs.io/en/latest/using-mupdf.html#multi-threading

This example will create one main thread for reading pages from the
document, and one thread per page for rendering. After rendering
the main thread will wait for each rendering thread to complete before
writing that thread's rendered image to a PNG image. There is
nothing in MuPDF requiring a rendering thread to only render a
single page, this is just a design decision taken for this example.

To build this example in a source tree and render every page as a
separate PNG, run:
make examples
./build/debug/multi-threaded document.pdf

To build from installed sources, and render the same document, run:
gcc -I/usr/local/include -o multi-threaded \
	/usr/local/share/doc/mupdf/examples/multi-threaded.c \
	/usr/local/lib/libmupdf.a \
	/usr/local/lib/libmupdfthird.a \
	-lpthread -lm
./multi-threaded document.pdf

Caution! As all pages are rendered simultaneously, please choose a
file with just a few pages to avoid stressing your machine too
much. Also you may run in to a limitation on the number of threads
depending on your environment.
*/

//Include the MuPDF header file, and pthread's header file.
#include <mupdf/fitz.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

// A convenience function for dying abruptly on pthread errors.

void
fail(const char *msg)
{
	fprintf(stderr, "%s\n", msg);
	abort();
}

// The data structure passed between the requesting main thread and
// each rendering thread.

struct thread_data {
	// A pointer to the original context in the main thread sent
	// from main to rendering thread. It will be used to create
	// each rendering thread's context clone.
	fz_context *ctx;

	// Page number sent from main to rendering thread for printing
	int pagenumber;

	// The display list as obtained by the main thread and sent
	// from main to rendering thread. This contains the drawing
	// commands (text, images, etc.) for the page that should be
	// rendered.
	fz_display_list *list;

	// The area of the page to render as obtained by the main
	// thread and sent from main to rendering thread.
	fz_rect bbox;

	// This is the result, a pixmap containing the rendered page.
	// It is passed first from main thread to the rendering
	// thread, then its samples are changed by the rendering
	// thread, and then back from the rendering thread to the main
	// thread.
	fz_pixmap *pix;

	// This is a note of whether a given thread failed or not.
	int failed;
};

// This is the function run by each rendering function. It takes
// pointer to an instance of the data structure described above and
// renders the display list into the pixmap before exiting.

void *
renderer(void *data_)
{
	struct thread_data *data = (struct thread_data *)data_;
	int pagenumber = data->pagenumber;
	fz_context *ctx = data->ctx;
	fz_display_list *list = data->list;
	fz_rect bbox = data->bbox;
	fz_device *dev = NULL;

	fprintf(stderr, "thread at page %d loading!\n", pagenumber);

	// The context pointer is pointing to the main thread's
	// context, so here we create a new context based on it for
	// use in this thread.
	ctx = fz_clone_context(ctx);

	// Next we run the display list through the draw device which
	// will render the request area of the page to the pixmap.

	fz_var(dev);

	fprintf(stderr, "thread at page %d rendering!\n", pagenumber);
	fz_try(ctx)
	{
		// Create a white pixmap using the correct dimensions.
		data->pix = fz_new_pixmap_with_bbox(ctx, fz_device_rgb(ctx), fz_round_rect(bbox), NULL, 0);
		fz_clear_pixmap_with_value(ctx, data->pix, 0xff);

		// Do the actual rendering.
		dev = fz_new_draw_device(ctx, fz_identity, data->pix);
		fz_run_display_list(ctx, list, dev, fz_identity, bbox, NULL);
		fz_close_device(ctx, dev);
	}
	fz_always(ctx)
		fz_drop_device(ctx, dev);
	fz_catch(ctx)
		data->failed = 1;

	// Free this thread's context.
	fz_drop_context(ctx);

	fprintf(stderr, "thread at page %d done!\n", pagenumber);

	return data;
}

// These are the two locking functions required by MuPDF when
// operating in a multi-threaded environment. They each take a user
// argument that can be used to transfer some state, in this case a
// pointer to the array of mutexes.

void lock_mutex(void *user, int lock)
{
	pthread_mutex_t *mutex = (pthread_mutex_t *) user;

	if (pthread_mutex_lock(&mutex[lock]) != 0)
		fail("pthread_mutex_lock()");
}

void unlock_mutex(void *user, int lock)
{
	pthread_mutex_t *mutex = (pthread_mutex_t *) user;

	if (pthread_mutex_unlock(&mutex[lock]) != 0)
		fail("pthread_mutex_unlock()");
}

int main(int argc, char **argv)
{
	char *filename = argc >= 2 ? argv[1] : "";
	pthread_t *thread = NULL;
	fz_locks_context locks;
	pthread_mutex_t mutex[FZ_LOCK_MAX];
	fz_context *ctx;
	fz_document *doc = NULL;
	int threads;
	int i;

	// Initialize FZ_LOCK_MAX number of non-recursive mutexes.
	for (i = 0; i < FZ_LOCK_MAX; i++)
	{
		if (pthread_mutex_init(&mutex[i], NULL) != 0)
			fail("pthread_mutex_init()");
	}

	// Initialize the locking structure with function pointers to
	// the locking functions and to the user data. In this case
	// the user data is a pointer to the array of mutexes so the
	// locking functions can find the relevant lock to change when
	// they are called. This way we avoid global variables.
	locks.user = mutex;
	locks.lock = lock_mutex;
	locks.unlock = unlock_mutex;

	// This is the main thread's context function, so supply the
	// locking structure. This context will be used to parse all
	// the pages from the document.
	ctx = fz_new_context(NULL, &locks, FZ_STORE_UNLIMITED);

	fz_var(thread);
	fz_var(doc);

	fz_try(ctx)
	{
		// Register default file types.
		fz_register_document_handlers(ctx);

		// Open the PDF, XPS or CBZ document.
		doc = fz_open_document(ctx, filename);

		// Retrieve the number of pages, which translates to the
		// number of threads used for rendering pages.
		threads = fz_count_pages(ctx, doc);
		fprintf(stderr, "spawning %d threads, one per page...\n", threads);

		thread = malloc(threads * sizeof (*thread));

		for (i = 0; i < threads; i++)
		{
			fz_page *page;
			fz_rect bbox;
			fz_display_list *list;
			fz_device *dev = NULL;
			fz_pixmap *pix;
			struct thread_data *data;

			fz_var(dev);

			fz_try(ctx)
			{
				// Load the relevant page for each thread. Note, that this
				// cannot be done on the worker threads, as only one thread
				// at a time can ever be accessing the document.
				page = fz_load_page(ctx, doc, i);

				// Compute the bounding box for each page.
				bbox = fz_bound_page(ctx, page);

				// Create a display list that will hold the drawing
				// commands for the page. Once we have the display list
				// this can safely be used on any other thread.
				list = fz_new_display_list(ctx, bbox);

				// Create a display list device to populate the page's display list.
				dev = fz_new_list_device(ctx, list);

				// Run the page to that device.
				fz_run_page(ctx, page, dev, fz_identity, NULL);

				// Close the device neatly, so everything is flushed to the list.
				fz_close_device(ctx, dev);
			}
			fz_always(ctx)
			{
				// Throw away the device.
				fz_drop_device(ctx, dev);

				// The page is no longer needed, all drawing commands
				// are now in the display list.
				fz_drop_page(ctx, page);
			}
			fz_catch(ctx)
				fz_rethrow(ctx);

			// Populate the data structure to be sent to the
			// rendering thread for this page.
			data = malloc(sizeof (*data));

			data->pagenumber = i + 1;
			data->ctx = ctx;
			data->list = list;
			data->bbox = bbox;
			data->pix = NULL;
			data->failed = 0;

			// Create the thread and pass it the data structure.
			if (pthread_create(&thread[i], NULL, renderer, data) != 0)
				fail("pthread_create()");
		}

		// Now each thread is rendering pages, so wait for each thread
		// to complete its rendering.
		fprintf(stderr, "joining %d threads...\n", threads);
		for (i = 0; i < threads; i++)
		{
			char filename[42];
			struct thread_data *data;

			if (pthread_join(thread[i], (void **) &data) != 0)
				fail("pthread_join");

			if (data->failed)
			{
				fprintf(stderr, "\tRendering for page %d failed\n", i + 1);
			}
			else
			{
				sprintf(filename, "out%04d.png", i);
				fprintf(stderr, "\tSaving %s...\n", filename);

				// Write the rendered image to a PNG file
				fz_save_pixmap_as_png(ctx, data->pix, filename);
			}

			// Free the thread's pixmap and display list.
			fz_drop_pixmap(ctx, data->pix);
			fz_drop_display_list(ctx, data->list);

			// Free the data structure passed back and forth
			// between the main thread and rendering thread.
			free(data);
		}
	}
	fz_always(ctx)
	{
		// Free the thread structure
		free(thread);

		// Drop the document
		fz_drop_document(ctx, doc);
	}
	fz_catch(ctx)
	{
		fz_report_error(ctx);
		fail("error");
	}

	// Finally the main thread's context is freed.
	fz_drop_context(ctx);

	fprintf(stderr, "finally!\n");
	fflush(NULL);

	return 0;
}
```
