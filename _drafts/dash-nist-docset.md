---
layout: post
title: Creating a Dash docset for the NIST algorithm database
comments: true
category: articles
---

I came across the [NIST Dictionary of Algorithms and Data Structures](http://xlinux.nist.gov/dads/) today, and decided to create a [Dash](http://kapeli.com/dash) docset for it.

The steps for [creating a docset](http://kapeli.com/docsets) are very simple, and I hope this blog post makes it easier for people to bundle standard webpages.

The first step in creating a docset is to create the directory layout, adding a propery list information file, and creating a sqlite3 database. 

~~~ bash
$ mkdir -p nist.docset/Contents/Resources/Documents/

$ cat > nist.docset/Contents/Info.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>CFBundleIdentifier</key>
	<string>nist</string>
	<key>CFBundleName</key>
	<string>NIST</string>
	<key>DocSetPlatformFamily</key>
	<string>nist</string>
	<key>isDashDocset</key>
	<true/>
</dict>

$ sqlite3 docSet.dsidx 'CREATE TABLE searchIndex(id INTEGER PRIMARY KEY, name TEXT, type TEXT, path TEXT);'
$ sqlite3 docSet.dsidx 'CREATE UNIQUE INDEX anchor ON searchIndex (name, type, path);'

</plist>
~~~

The next step is to crawl the NIST webpage (which is fast, your mileage may vary depending on the sites you want to scrape). I couldn't find an HTML zip file.

Note how I use the `-k` flag for wget to rewrite the HTML links.

~~~ bash
$ wget -r -k -c -np http://xlinux.nist.gov/dads/
$ # more the HTML files into the docset
$ mv xlinux.nist.gov/dads/* nist.docset/Contents/Resources/Documents/
~~~

Finally, we need to extract the titles of the downloaded HTML pages to extract the name of the entry, and add it to the sqlite3 index. I use [pup](https://github.com/EricChiang/pup) to parse the html. It is a very nice library that gives you a command line jquery style interface.

This is the index.sh script I use, which needs to be run from inside `nist.docset/Contents/Resources/Documents`.

~~~ bash
#!/bin/sh

index () {
    FILE="$1"
    NAME=$(pup 'title text{}' < "$FILE" | sed -e "s/\'/\'\'/g")

    sqlite3 ../docSet.dsidx "INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('$NAME', 'Entry', '$FILE');" ||
        echo sqlite3 docSet.dsidx "INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('$NAME', 'Entry', '$FILE');"
}

index "$1"
~~~

Here is the loop I use to construct all the entries:

~~~ bash
$ for i in HTML/*; do ../index.sh "$i"; done
~~~

Once the database has been created, we need to add a nice icon to differentiate the docset from other docsets. I use a small mathematica script to create a multiresolution TIFF icon.

~~~ mathematica
#!/usr/local/bin/MathematicaScript -script

filename = First@Rest[$ScriptCommandLine];

blackRect =
 Rasterize[Rectangle[], Background -> Black, ImageSize -> {64, 64}];

nist = Import[
  "https://www.nightlionsecurity.com/wp-content/uploads/2013/11/\
NISTlogo.jpg"];

smallNist = ImageResize[nist, 64];
smallN = ImageResize[ImageTake[nist, {70, 200}, {15, 190}], 64];

logo = ImageCompose[blackRect, smallN];

Export[filename, {ImageResize[logo, 16, Padding -> Black],
  ImageResize[logo, 32, Padding -> Black]}];
~~~

Just run `make-icon.m icon.tiff` from inside `nist.docset`. 

There you go, a nice docset for the NIST dictionary of data structures, ready to be imported into dash.