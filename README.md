smartcrop.go
============

smartcrop implementation in Go

smartcrop finds good crops for arbitrary images and crop sizes, based on Jonas Wagner's [smartcrop.js](https://github.com/jwagner/smartcrop.js)

![Example](./gopher_example.jpg)
Image: [https://www.flickr.com/photos/usfwspacific/8182486789](https://www.flickr.com/photos/usfwspacific/8182486789) CC BY U.S. Fish & Wildlife

## Installation

Make sure you have a working Go environment. See the [install instructions](http://golang.org/doc/install.html).

Additionally you need to have opencv installed. 

You can install it on Mac OS X using:
```
brew tap homebrew/science
brew install opencv
```

On Linux you need to have the following packages installed:
```
libcv-dev libopencv-dev libopencv-contrib-dev libhighgui-dev libopencv-photo-dev libopencv-imgproc-dev libopencv-stitching-dev libopencv-superres-dev libopencv-ts-dev libopencv-videostab-dev 
```

Now you can install smartcrop, simply run:

    go get github.com/muesli/smartcrop

To compile it from source:

    git clone git://github.com/muesli/smartcrop.git
    cd smartcrop && go build && go test -v

## Example
```go
package main

import (
	"github.com/muesli/smartcrop"
	"fmt"
	"image"
	_ "image/png"
	"os"
)

func main() {
  fi,err := os.Open("test.png")
  if err != nil {
    log.Fatalf(err.Error())
  }

  defer fi.Close()

  img, _, err := image.Decode(fi)
  if err != nil {
    log.Fatalf(err.Error())
  }

  analyzer := smartcrop.NewAnalyzer()
	topCrop, _ := analyzer.FindBestCrop(img, 250, 250)
	fmt.Printf("Top crop: %+v\n", topCrop)
}
```

With face detection:
```go
func main() {
  fi,err := os.Open("test.png")
  if err != nil {
    log.Fatalf(err.Error())
  }

  defer fi.Close()

  img, _, err := image.Decode(fi)
  if err != nil {
    log.Fatalf(err.Error())
  }

  settings := smartcrop.CropSettings{
    FaceDetection:                    true,
    FaceDetectionHaarCascadeFilepath: "./files/aarcascade_frontalface_alt.xml",
  }
  analyzer := smartcrop.NewAnalyzerWithCropSettings(settings)
  topCrop, _ := analyzer.FindBestCrop(img, 250, 250)
  fmt.Printf("Top crop: %+v\n", topCrop)
}
```
Also see the test-cases in crop_test.go for further working examples.

## Development
API docs can be found [here](http://godoc.org/github.com/muesli/smartcrop).

Join us on IRC: irc.freenode.net/#smartcrop

Continuous integration: [![Build Status](https://secure.travis-ci.org/muesli/smartcrop.png)](http://travis-ci.org/muesli/smartcrop)
