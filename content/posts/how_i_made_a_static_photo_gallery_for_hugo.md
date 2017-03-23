+++
date = "2016-01-20T07:25:15-02:00"
title = "How I made a static photo gallery for Hugo"
categories = [ "development"]
tags=["hugo", "python", "gallery", "photos"]
+++

> **updated:** I've named this project [Hugallery](https://github.com/vhugo/hugallery) and I share and maintain its code at Github, if you're interested go check it out. Also the `README.md` file looks way better and up-to-date than this articles.

## Goal

Create a command-line program that generate a static web photo gallery based on a given directory with raw photos files.


## Planning

Things I want to accomplish in this project:
- It must be easy to install and use;
- It must be easy to configure and define or modify the gallery structure;
- It must be reusable for existing galleries;
- It should create file compatible with [HUGO](https://gohugo.io/) standards;

## Workflow

Each album should have 2 configuration file `album.json` and `photos.csv` formatted as JSON. If configuration files were not created, program will create them as template and give the instruction to update the files before running the program again.

### Album Configuration file: album.cfg

`gallery_location` (string) : Where do I put this?
`album_name` (string) : What do I call it?
`parent_path` (string) : Does it have a parent?  *remember to check if parent in the cfg file really exists*
`thumbnail_max_height` (integer): How tall should the thumbnail be?
`thumbnail_max_width` (integer): How wide should the thumbnail be?

### Album Configuration file: photos.cfg

In this file, the `order` of the elements/photos in the array will represent its order in the album page.
`photos` (array) : List of images
`photos.title` (string) : Name of a given image
`photos.description` (string) : Description  of a given image

## Figure stuff out

This is a compilation of research I did for this project before work began.

### How to create a CLI Python tool

Decide to use [argparse](https://docs.python.org/2/library/argparse.html#usage).

### How to resize images

This will show how to resize images using the command-line. I've used ImageMagick long time ago, going to use it again.

#### Installing ImageMagick on Mac OS X

Using [Homebrew](http://brew.sh/) on Mac OS X Yosemite 10.10.5 (14F1509):
```
# brew install ImageMagick
```
Output:
```
==> Installing dependencies for imagemagick: libtool, libpng, libtiff, freetype
==> Installing imagemagick dependency: libtool
==> Downloading https://homebrew.bintray.com/bottles/libtool-2.4.6.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libtool-2.4.6.yosemite.bottle.tar.gz
==> Caveats
In order to prevent conflicts with Apple's own libtool we have prepended a "g"
so, you have instead: glibtool and glibtoolize.
==> Summary
🍺  /usr/local/Cellar/libtool/2.4.6: 127 files, 4.0M
==> Installing imagemagick dependency: libpng
==> Downloading https://homebrew.bintray.com/bottles/libpng-1.6.18.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libpng-1.6.18.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/libpng/1.6.18: 17 files, 1.2M
==> Installing imagemagick dependency: libtiff
==> Downloading https://homebrew.bintray.com/bottles/libtiff-4.0.6.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libtiff-4.0.6.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/libtiff/4.0.6: 259 files, 3.9M
==> Installing imagemagick dependency: freetype
==> Downloading https://homebrew.bintray.com/bottles/freetype-2.6_1.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring freetype-2.6_1.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/freetype/2.6_1: 60 files, 2.6M
==> Installing imagemagick
==> Downloading https://homebrew.bintray.com/bottles/imagemagick-6.9.1-10.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring imagemagick-6.9.1-10.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/imagemagick/6.9.1-10: 1447 files, 22M
```

To test I ran the following commands
```
# convert logo: logo.gif
```
and  then
```
# identify logo.gif
logo.gif GIF 640x480 640x480+0+0 8-bit sRGB 256c 28.6KB 0.000u 0:00.000
```
the command `display` didn't work for me, but I don't think I'm going to need it for now, but to see the image you can type `open logo.gif` it should open mac's preview.

#### Converting and resize images

Since this project is a **photo** gallery I'll assume all file should be converted to *JPEG*. Here is the first test for convert and resize an image. Downloaded this cool image from [Pexels](https://www.pexels.com/photo/forest-hiking-trees-15286/)
```
# wget -nd "https://static.pexels.com/photos/15286/pexels-photo.jpg"
```

Found this cool [article](http://www.cyberciti.biz/tips/howto-linux-creating-a-image-thumbnails-from-shell-prompt.html) which shows how to use a nice feature ImageMagick has that create smaller images from original, the following test will create a image
```
# convert -thumbnail 200 pexels-photo.jpg pexels-photo-thumb.jpg
```

It worked!

Found a cool pypi package called [Wand](http://docs.wand-py.org/en/0.4.2/)  did the following test it so easy:

{{< highlight python "linenos=inline" >}}
#!/usr/local/bin/python
from wand.image import Image

min_size = 600 # mostly for an even crop measure
with Image(filename="pexels-photo.jpg") as img:

	# look for the small measure to set as min_size
	if (img.height < img.width):
		height = min_size
		width = min_size * img.width / img.height
	else:
		width = min_size
		height = min_size * img.height / img.width

	# Clone the original image to create cropped file
	with img.clone() as i:
		i.resize( width, height )
		i.crop(width=min_size, height=min_size) # Perfect crop ;-)
		i.save(filename="pexels-photo-thumbtest.jpg")
{{</highlight>}}

## Installing

requirements
- [Wand](http://docs.wand-py.org/en/0.4.2/)
- [ImageMagick](www.imagemagick.org)

## Usage

This is how end user should use it:
```
# genstaphotogal
```

## Ideas for improvement

- Make it more interactive
- Optimize: Check if files exists before trying to create crops and resize (maybe check sizes)
- Make better way to select cover
- Make better tests

## Write repeatable instructions to use it

I wrote the [README](https://github.com/vhugo/hugallery/blob/master/README.md) file with more insights and instructions on how to install and use.
