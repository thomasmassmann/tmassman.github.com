---
layout: post
title: "collective.prettyphoto for Plone"
description: "Over the past few days I had some time to finish what was planned
              for so long: integrate prettyPhoto (written by Stéphane Caron)
              into Plone. Here it is..."
category:
tags: [Plone]
---
{% include JB/setup %}

**Over the past few days I had some time to finish what was planned for so
long: integrate prettyPhoto (written by Stéphane Caron) into Plone. Here it
is...**


## Introduction

prettyPhoto is a jQuery based lightbox clone. Not only does it support images,
it also add support for videos, flash, YouTube, iFrames. It's a full blown
media lightbox. The setup is easy and quick, plus the script is compatible in
every major browser.

The original implementation by Stéphane Caron can be found [here][prettyPhoto]
.

This plugin has been tested and is known to work in the following browsers

- Firefox 2.0+
- Safari 3.1.1+
- Opera 9+
- Internet Explorer 6.0+

collective.prettyphoto is an implementation of prettyPhoto for Plone.

## Installing

[collective.prettyphoto] is availabe at PyPI. It
requires Plone 3.x or later (tested on 3.3.3).

### Installing without buildout

Install this package in either your system path packages or in the lib/python
directory of your Zope instance. You can do this using either easy_install or
via the setup.py script.

### Installing with buildout

If you are using [buildout] to manage your instance installing
collective.prettyphoto is even simpler. You can install collective.prettyphoto
by adding it to the eggs line for your instance:

    [instance]
    eggs = collective.prettyphoto

After updating the configuration you need to run the ''bin/buildout'', which will take care of updating your system.

## Usage

collective.prettyphoto adds a new view for Topics, Folders and Large Plone
Folders: **Thumbnail view (prettyPhoto)**.

To use prettyPhoto for inline elements just add **prettyPhoto** from the styles
menu (Kupu and TinyMCE) to the link.

## Configuration

collective.prettyphoto can be customized via property sheet (go to ZMI,
portal_properties, prettyphoto_properties).

- `theme`:

  - `dark_rounded`
  - `dark_square`
  - `facebook`
  - `light_rounded` (default)
  - `light_square`

- `speed`:

  - `fast`
  - `normal` (default)
  - `slow`

- `opacity: value from 0.0 to 1.0 (default: 0.80)
- `show_title`: show the title for images? (default: True)
- `counter_sep`: the separator for the gallery counter 1 "of" 2 (default: "/")
- `autoplay`: automatically start videos? (default: True)
- `iframe_width`: the width of the iframe (must be percantage, default: 75%)
- `iframe_height`: the height of the iframe (must be percantage, default: 75%)


[buildout]: http://pypi.python.org/pypi/zc.buildout
[collective.prettyphoto]: http://pypi.python.org/pypi/collective.prettyphoto
[prettyPhoto]: http://www.no-margin-for-errors.com/projects/prettyphoto-jquery-lightbox-clone/
