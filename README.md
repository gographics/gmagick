# Go bindings for [GraphicsMagick](http://www.graphicsmagick.org)

## Install `GraphicsMagick` libraries and header files

### Windows
+ Install [msys2-x86_64](http://www.msys2.org/)
+ In msys2 shell: 
```
pacman -S mingw-w64-x86_64-gcc
pacman -S mingw-w64-x86_64-pkg-config
pacman -S mingw-w64-x86_64-graphicsmagick
```
+ Add follwing environment variable:
```
set PATH=<msys64>\mingw64\bin;%PATH%
set PKG_CONFIG_PATH=<msys64>\mingw64\lib\pkgconfig
set MAGICK_CODER_MODULE_PATH=<msys64>\mingw64\lib\GraphicsMagick-1.3.25\modules-Q8\coders
```
(BTW: you should change `<msys64>` to your installation path of `msys2`)

### MacOS
Install `GraphicsMagick` using [Homebrew](https://brew.sh/) or [MacPorts](https://www.macports.org)    
An example of `MacPorts`:

+ `sudo port install graphicsmagick`
+ `export PKG_CONFIG_PATH=/opt/local/lib/pkgconfig`


### CentOS

+ ensure `epel-release` was installed. ([help](https://pkgs.org/download/epel-release))
+ `yum install GraphicsMagick-devel`

### Ubuntu

+ `sudo apt-get install libgraphicsmagick1-dev`

## Install golang bindings

+ `go get github.com/gographics/gmagick`


## Example

```
package main

import (
    "flag"
    "github.com/gographics/gmagick"
)

func resize(orig string, dest string) {
    mw := gmagick.NewMagickWand()
    defer mw.Destroy()
    mw.ReadImage(orig)
    filter := gmagick.FILTER_LANCZOS
    w := mw.GetImageWidth()
    h := mw.GetImageHeight()
    mw.ResizeImage(w/2, h/2, filter, 1)
    mw.WriteImage(dest)
}

func main() {
    f := flag.String("from", "", "original image file ...")
    t := flag.String("to", "", "target file ...")
    flag.Parse()

    gmagick.Initialize()
    defer gmagick.Terminate()

    resize(*f, *t)
}
```
