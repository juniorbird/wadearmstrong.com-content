---
title: Building an Eleventy Plugin
description: Some Content
layout: layouts/base.njk
date: 2023-05-12
draft: true 
tags:
  - posts
---
The first thing I tried was to make a filter
Filters work by taking a bunch of input and giving you access to mutate it

But I wanted a more "plugin-like" experience that could set additional values on the page
So I tried to make a plugin
Here's the thing: there's not official documentation (that I could find) on how to do that
Here's what I learned

First of all, all plugins are either:
* filters
* transforms

Transforms get access to the whole dataset
Filters only get what you pipe them

I decided to do a filter because I need to know what the dataset is for the navigation, and that's provided by the user's choice in their template.

In order to do the whole job myself, I call the navigation filter on the output at the right time, and then append to that.