---
title: Git Submodules, and When to Use Them
description: A description
layout: layouts/base.njk
date: 2023-07-11
tags:
  - posts
---

# Git Submodules, and When to Use Them

Git submodules are a fairly obscure feature of git. Imagine you have a main repo in which you want to include another, separate git repo — maybe a dependency or a vendor or something like that. Submodules let you include a separate git repo within a main git repo, while still being able to pull, push, and do all the git things to that separate git repo.

## Uses of Git Submodules

The historic requirement from submodules is, so far as I understand, a desire to make it easy to vendor dependencies. These days, it's much easier to run a private npm/pip/etc. registry for anything internal you want to vendor. Heck, if you're doing a component-based application, you pretty much need your own registry, and the deploy processes to automatically push your components there. 

But sometimes you don't want to go to that amount of trouble. Maybe it's a small internal tool, or something only one person works on — the overhead of a real deploy process may be excessive. Maybe you've got a blog powered by one of those static site generators that keeps everything in `/content` and you don't want to worry about your content changing every time you `git checkout` a new branch.[^1]

## Code Examples

You probably don't want to use submodules. But, if your project is small enough, here's how you do it.

Start by cloning your submodule into your main git repo (assumes Github, but easy enough to change this for Gitlab etc.):

```bash
git submodule add git@github.com:username/repo.git destination   
```

So you're adding the repo as a submodule, in the folder named `destination`. (You can skip that value if you don't want to control your folder name.)

Now you can pull updates by:

```bash
cd destination
git pull
git submodule update --init --recursive
```

Note that you're doing two things here:

1. Pulling the change as normal
2. Updating the git index of the main project to point to whatever commit it is you just pulled

That last part is the important one here; the submodule points to a specific commit (or tag, etc.). If you want  

[^1]: That's how I use submodules. [This site](https://github.com/juniorbird/wadearmstrong.com) is made with [eleventy](https://www.11ty.dev) and I manage [my content in a separate repo](https://github.com/juniorbird/wadearmstrong.com-content). I started doing this after I realized that I'd written a blog post I wanted to publish now in a feature branch I didn't want to merge now.

