# Advanced

## Create PDF

```javascript
import * as mupdf from "mupdf"

// Create a PDF from scratch using helper functions.

// This example creates a new PDF file from scratch, using helper
// functions to create resources and page objects.
// This assumes a basic working knowledge of the PDF file format.

import mupdf from "mupdf"

// Create a new empty document with no pages.
var pdf = new mupdf.PDFDocument()

// Load built-in font and create WinAnsi encoded simple font resource.
var font = pdf.addSimpleFont(new mupdf.Font("Times-Roman"))

// Load PNG file and create image resource.
var image = pdf.addImage(new mupdf.Image("example.jpg"))

// Create resource dictionary.
var resources = pdf.addObject({
	Font: { Tm: font },
	XObject: { Im0: image },
})

// Create content stream data.
var contents =
	"10 10 280 330 re s\n" +
	"q 200 0 0 200 50 100 cm /Im0 Do Q\n" +
	"BT /Tm 16 Tf 50 50 TD (Hello, world!) Tj ET\n"

// Create a new page object.
var page = pdf.addPage([0,0,300,350], 0, resources, contents)

// Insert page object at the end of the document.
pdf.insertPage(-1, page)

// Save the document to file.
pdf.save("out.pdf", "pretty,ascii,compress-images,compress-fonts")
```

## Create PDF (low level)

```javascript
import * as mupdf from "mupdf"

// Create a PDF from scratch.

// This example creates a new PDF file from scratch, using only the low level APIs.
// This assumes a basic working knowledge of the PDF file format.

// Create a new empty document with no pages.
var pdf = new mupdf.PDFDocument()

// Create and add a font resource.
var font = pdf.addObject({
	Type: "Font",
	Subtype: "Type1",
	Encoding: "WinAnsiEncoding",
	BaseFont: "Times-Roman",
})

// Create and add an image resource:
var image = pdf.addRawStream(
	// The raw stream contents, hex encoded to match the Filter entry:
	"004488CCEEBB7733>",
	// The image object dictionary:
	{
		Type: "XObject",
		Subtype: "Image",
		Width: 4,
		Height: 2,
		BitsPerComponent: 8,
		ColorSpace: "DeviceGray",
		Filter: "ASCIIHexDecode",
	}
);

// Create resource dictionary.
var resources = pdf.addObject({
	Font: { Tm: font },
	XObject: { Im0: image },
})

// Create content stream.
var buffer = new Buffer()
buffer.writeLine("10 10 280 330 re s")
buffer.writeLine("q 200 0 0 200 50 100 cm /Im0 Do Q")
buffer.writeLine("BT /Tm 16 Tf 50 50 TD (Hello, world!) Tj ET")
var contents = pdf.addStream(buffer)

// Create page object.
var page = pdf.addObject({
	Type: "Page",
	MediaBox: [0,0,300,350],
	Contents: contents,
	Resources: resources,
})

// Insert page object into page tree.
var pagetree = pdf.getTrailer().Root.Pages
pagetree.Count = 1
pagetree.Kids = [ page ]
page.Parent = pagetree

// Save the document.
pdf.save("out.pdf")
```

## Merge PDF files

```javascript
// A re-implementation of "mutool merge" in JavaScript.

import * as mupdf from "mupdf"

function copyPage(dstDoc, srcDoc, pageNumber, dstFromSrc) {
	var srcPage, dstPage
	srcPage = srcDoc.findPage(pageNumber)
	dstPage = dstDoc.newDictionary()
	dstPage.Type = dstDoc.newName("Page")
	if (srcPage.MediaBox) dstPage.MediaBox = dstFromSrc.graftObject(srcPage.MediaBox)
	if (srcPage.Rotate) dstPage.Rotate = dstFromSrc.graftObject(srcPage.Rotate)
	if (srcPage.Resources) dstPage.Resources = dstFromSrc.graftObject(srcPage.Resources)
	if (srcPage.Contents) dstPage.Contents = dstFromSrc.graftObject(srcPage.Contents)
	dstDoc.insertPage(-1, dstDoc.addObject(dstPage))
}

function copyAllPages(dstDoc, srcDoc) {
	var dstFromSrc = dstDoc.newGraftMap()
	var k, n = srcDoc.countPages()
	for (k = 0; k < n; ++k)
		copyPage(dstDoc, srcDoc, k, dstFromSrc)
}

function pdfmerge() {
	var srcDoc, dstDoc, i

	dstDoc = new mupdf.PDFDocument()
	for (i = 3; i < process.argv.length; ++i) {
		srcDoc = mupdf.Document.openDocument(process.argv[i])
		copyAllPages(dstDoc, srcDoc)
	}
	dstDoc.save(process.argv[2], "compress")
}

if (process.argv.length < 4)
	print("usage: mutool run pdf-merge.js output.pdf input1.pdf input2.pdf ...")
else
	pdfmerge()
```

## Create PDF with thumbnails

```javascript
// Create a PDF containing thumbnails of pages rendered from another PDF.

import * as mupdf from "mupdf"

var pdf = new mupdf.PDFDocument()

var subdoc = mupdf.Document.openDocument("pdfref17.pdf")

var resources = { XObject: {} }

var contents = new mupdf.Buffer()
for (var i=0; i < 5; ++i) {
	var pixmap = subdoc.loadPage(1140+i).toPixmap([0.2,0,0,0.2,0,0], mupdf.ColorSpace.DeviceRGB, true)
	resources.XObject["Im" + i] = pdf.addImage(new Image(pixmap))
	contents.writeLine("q 100 0 0 150 " + (50+100*i) + " 50 cm /Im" + i + " Do Q")
}

var page = pdf.addPage([0,0,100+i*100,250], 0, resources, contents)
pdf.insertPage(-1, page)

pdf.save("out.pdf")
```

## Use Device to draw graphics

```javascript
// Use device interface to draw some graphics and save as a PNG.

import * as mupdf from "mupdf"

var font = new mupdf.Font("Times-Roman")
var image = new mupdf.Image("huntingofthesnark.png")
var path, text

var pixmap = new mupdf.Pixmap(mupdf.ColorSpace.DeviceRGB, [ 0, 0, 500, 600 ], false)
pixmap.clear(255)
var device = new mupdf.DrawDevice(mupdf.Matrix.identity, pixmap)
var transform = [ 2, 0, 0, 2, 0, 0 ]
{
	text = new mupdf.Text()
	{
		text.showString(font, [ 16, 0, 0, -16, 100, 30 ], "Hello, world!")
		text.showString(font, [ 0, 16, 16, 0, 15, 100 ], "Hello, world!")
	}
	device.fillText(text, transform, mupdf.ColorSpace.DeviceGray, [ 0 ], 1)

	path = new mupdf.Path()
	{
		path.moveTo(10, 10)
		path.lineTo(90, 10)
		path.lineTo(90, 90)
		path.lineTo(10, 90)
		path.closePath()
	}
	device.fillPath(path, false, transform, mupdf.ColorSpace.DeviceRGB, [ 1, 0, 0 ], 1)
	device.strokePath(
		path,
		{ dashes: [ 5, 10 ], lineWidth: 3, lineCap: "Round" },
		transform,
		mupdf.ColorSpace.DeviceRGB,
		[ 0, 0, 0 ],
		1
	)

	path = new mupdf.Path()
	{
		path.moveTo(100, 100)
		path.curveTo(150, 100, 200, 150, 200, 200)
		path.curveTo(200, 300, 0, 300, 100, 100)
		path.closePath()
	}
	device.clipPath(path, true, transform)
	{
		device.fillImage(image, Matrix.concat(transform, [ 300, 0, 0, 300, 0, 0 ]), 1)
	}
	device.popClip()
}
device.close()

pixmap.saveAsPNG("out.png")
```

## Trace Device

```javascript
import * as mupdf from "mupdf"

var Q = JSON.stringify

var pathPrinter = {
	moveTo: function (x,y) { print("moveTo", x, y) },
	lineTo: function (x,y) { print("lineTo", x, y) },
	curveTo: function (x1,y1,x2,y2,x3,y3) { print("curveTo", x1, y1, x2, y2, x3, y3) },
	closePath: function () { print("closePath") },
}

var textPrinter = {
	beginSpan: function (f,m,wmode, bidi, dir, lang) {
		print("beginSpan",f,m,wmode,bidi,dir,repr(lang));
	},
	showGlyph: function (f,m,g,u,v,b) { print("glyph",f,m,g,u,v,b) },
	endSpan: function () { print("endSpan"); }
}

var traceDevice = {
	fillPath: function (path, evenOdd, ctm, colorSpace, color, alpha) {
		print("fillPath", evenOdd, ctm, colorSpace, color, alpha)
		path.walk(pathPrinter)
	},
	clipPath: function (path, evenOdd, ctm) {
		print("clipPath", evenOdd, ctm)
		path.walk(pathPrinter)
	},
	strokePath: function (path, stroke, ctm, colorSpace, color, alpha) {
		print("strokePath", Q(stroke), ctm, colorSpace, color, alpha)
		path.walk(pathPrinter)
	},
	clipStrokePath: function (path, stroke, ctm) {
		print("clipStrokePath", Q(stroke), ctm)
		path.walk(pathPrinter)
	},

	fillText: function (text, ctm, colorSpace, color, alpha) {
		print("fillText", ctm, colorSpace, color, alpha)
		text.walk(textPrinter)
	},
	clipText: function (text, ctm) {
		print("clipText", ctm)
		text.walk(textPrinter)
	},
	strokeText: function (text, stroke, ctm, colorSpace, color, alpha) {
		print("strokeText", Q(stroke), ctm, colorSpace, color, alpha)
		text.walk(textPrinter)
	},
	clipStrokeText: function (text, stroke, ctm) {
		print("clipStrokeText", Q(stroke), ctm)
		text.walk(textPrinter)
	},
	ignoreText: function (text, ctm) {
		print("ignoreText", ctm)
		text.walk(textPrinter)
	},

	fillShade: function (shade, ctm, alpha) {
		print("fillShade", shade, ctm, alpha)
	},
	fillImage: function (image, ctm, alpha) {
		print("fillImage", image, ctm, alpha)
	},
	fillImageMask: function (image, ctm, colorSpace, color, alpha) {
		print("fillImageMask", image, ctm, colorSpace, color, alpha)
	},
	clipImageMask: function (image, ctm) {
		print("clipImageMask", image, ctm)
	},

	beginMask: function (area, luminosity, colorspace, color) {
		print("beginMask", area, luminosity, colorspace, color)
	},
	endMask: function () {
		print("endMask")
	},

	popClip: function () {
		print("popClip")
	},

	beginGroup: function (area, isolated, knockout, blendmode, alpha) {
		print("beginGroup", area, isolated, knockout, blendmode, alpha)
	},
	endGroup: function () {
		print("endGroup")
	},
	beginTile: function (area, view, xstep, ystep, ctm, id) {
		print("beginTile", area, view, xstep, ystep, ctm, id)
		return 0
	},
	endTile: function () {
		print("endTile")
	},
	beginLayer: function (name) {
		print("beginLayer", name)
	},
	endLayer: function () {
		print("endLayer")
	},
	beginStructure: function (structure, raw, uid) {
		print("beginStructure", structure, raw, uiw)
	},
	endStructure: function () {
		print("endStructure")
	},
	beginMetatext: function (meta, metatext) {
		print("beginMetatext", meta, metatext)
	},
	endMetatext: function () {
		print("endMetatext")
	},

	renderFlags: function (set, clear) {
		print("renderFlags", set, clear)
	},
	setDefaultColorSpaces: function (colorSpaces) {
		print("setDefaultColorSpaces", colorSpaces.getDefaultGray(),
		colorSpaces.getDefaultRGB(), colorSpaces.getDefaultCMYK(),
		colorSpaces.getOutputIntent())
	},

	close: function () {
		print("close")
	},
}

if (scriptArgs.length != 2)
	print("usage: mutool run trace-device.js document.pdf pageNumber")
else {
	var doc = mupdf.Document.openDocument(scriptArgs[0]);
	var page = doc.loadPage(parseInt(scriptArgs[1])-1);
	page.run(traceDevice, mupdf.Matrix.identity);
}
```
