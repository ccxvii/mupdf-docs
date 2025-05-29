# Installing

## Download

Either [download a release](https://mupdf.com/releases) or clone the latest
source from the Git repository:

```none
git clone --recursive git://git.ghostscript.com/mupdf.git
```

## Build on Windows

For Windows use the Visual Studio solution file in `platform/win32/mupdf.sln`.

## Build on MacOS

MacOS should be buildable in the same way as for Linux, but the exact set of
required libraries may vary. No viewer option is available as standard.

## Build on Linux (and BSD and macOS)

Use the GNU makefile to build on Linux or the BSDs.

```none
make
```

The viewers require the X11 (and OpenGL for mupdf-gl) headers and libraries to build.
Install these packages (the exact package names may vary depending on your distro of choice):

```none
sudo apt install xorg-dev libxcursor-dev libxrandr-dev libxinerama-dev
sudo apt install mesa-common-dev libgl1-mesa-dev packages libglu1-mesa-dev
```

Alternatively, if you only want to build the MuPDF library command line tools:

```none
make tools
```

## Testing the build

The steps above will put the libraries and binaries in the `build/release/` directory.

The tools and viewers are portable executables that don’t need any auxiliary
files, so can be run from anywhere. To make sure that everything works, you can
run them directly from the build directory:

```none
./build/release/mutool -v
```

## Install on Linux

You can safely copy the binaries to any directory in your path.

```none
cp ./build/release/mutool ~/.local/bin/mutool
```

Or, if you prefer you can install the binaries, libraries, and headers system wide:

```none
make prefix=/usr/local install
```

Good luck!
