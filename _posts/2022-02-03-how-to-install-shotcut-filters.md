---
layout: post
title: How To Install Custom Filters In Shotcut
author: Joshua Britain
tags: Guide Windows
---

This is a pretty small post here, but I recently was attempting to install a user made filter in Shotcut, and couldn't find any instructions online on how to do so, so here's what you do.

Now the filter I've downloaded (titled 'affine') extracts to a folder with two files in it: `meta.qml` and `ui.qml`. What you want to do is copy that folder to `C:\Program Files\Shotcut\share\shotcut\qml\filters`, so that means your files will have a location like `C:\Program Files\Shotcut\share\shotcut\qml\filters\affine\meta.qml`. Obviously your file path will be different if you are using a different filter or have Shotcut installed to a different location.

Hopefully someone out there finds this useful!