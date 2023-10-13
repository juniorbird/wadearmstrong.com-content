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

## Git Submodules: Why?

The historic requirement from submodules is, so far as I understand, a desire to make it easy to vendor dependencies. These days, it's much easier to run a private npm/pip/etc. registry for anything internal you want to vendor.[^1] Heck, if you're doing a component-based application, you pretty much need your own registry, and the deploy processes to automatically push your components there. 

But sometimes you don't want to go to that amount of trouble. Maybe it's a small internal tool, or something only one person works on — the overhead of a real deploy process may be excessive. Maybe you've got a blog powered by one of those static site generators that keeps everything in `/content` and you don't want to worry about your content changing every time you `git checkout` a new branch.[^2]

## OK, So What Do I Type Into the Terminal

As we discussed above, you probably don't want to use submodules. But, if your project is small enough, here's how you do it.

Start by cloning your submodule into your main git repo (assumes Github, but easy enough to change this for Gitlab etc.):

```bash
git submodule add git@github.com:username/repo.git <destination> 
```

So you're adding the repo as a submodule, in the folder named `destination`. (You can skip that value if you don't want to control your folder name.)

To make life more convenient, you should add a few little bits of sugar to your .gitconfig:

```bash
git config --local submodule.recurse true   
```

This pulls submodules of submodules. You probably don't need it, but it won't hurt.

```bash 
git config --local diff.submodule log
```

This adds info about your submodule when you run `git diff`.

```bash
git config --local status.submodulesummary 1 
```

This adds the actual git update message for your submodule's latest update when you run `git status`. 

Now you can pull updates by:

```bash
git submodule update --init --remote --recursive 
git add .
git commit -m 'Update to latest from submodule'      
``` 

That is, you:

1. Pull the latest from the submodule. This lets the repo know there's an update, but doesn't do anything with it.
2. Add the changes
3. Commit the changes, so now they're saved in your main repo.

Congrats, now you've got a mildly-inconvenient process that is possibly simpler than a real deploy for your small project. Have fun!  

[^1]: This is not to say that I don't like Go's approach, because, when I used Go a few years back, I really did. But it seems like everybody else is going the full package management, and, for those, you'd be better off using the package management.

[^2]: Yep, that's me. [This site](https://github.com/juniorbird/wadearmstrong.com) is made with [eleventy](https://www.11ty.dev) and I manage [my content in a separate repo](https://github.com/juniorbird/wadearmstrong.com-content). I started doing this after I realized that I'd written a blog post I wanted to publish now in a feature branch I didn't want to merge now.

