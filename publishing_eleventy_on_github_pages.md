---
title: Publishing an Eleventy Site on GitHub Pages
description: So you want to have a customizable site that takes the least effort possible and is free to host... here's how to publish your 11ty site on GitHub Pages!
date: 2023-08-24
layout: layouts/base.njk
tags:
  - posts
---
# Publishing an Eleventy Site on GitHub Pages

I wanted a simple-to-maintain, totally customizable web site that made content updating easy, so I chose [Eleventy](https://www.11ty.dev). I've enjoyed using the framework, but I also really wanted to host it on GitHub Pages, since I already pay GitHub.[^1] But, while GitHub Pages has out-of-the-box support for other frameworks, not Eleventy. Here's how I got my Eleventy site to automatically publish to GitHub Pages every time I make a push to `main`. 

## What's an Eleventy?

Eleventy is one of many frameworks called "static site generators." SSGs often fit in the same market segment as WordPress, at least on the simple end, but, instead of building pages dynamically for each request[^2], they generate the entire page as HTML with all required CSS and Javascript one time, so then you can upload the site to any basic host. If you make a change to the site, you need to regenerate it. This is an expensive way to do a large, dynamic site; but a cheap way to do something small, like, say, a personal or project site.

## How to Publish Your 11ty Site on GitHub Pages

There are two ways to do this:

1. Directly publish your output to Pages, leveraging work already done to support GitHub's default Jekyll pages
2. Use your own action

I tried 1 first, and then ended up with 2.

### Option One: Publish Directly With GitHub Pages

If you read a lot of blogs on how to do exactly this, they'll advise you to:

1. Select the "Classic Pages Experience"
2. Use [PeaceIris's GitHub Pages Action](https://github.com/peaceiris/actions-gh-pages) to do the packaging and publishing

This is really appealing, and it's where I started too. In particular, it's appealing because that Action has a ton of tests, and who wants to write their own Action with that many tests for a simple publish?

This worked great for me, until it didn't, when I discovered that the robust Action code, designed to handle tons of edge cases, couldn't handle mine. Some troubleshooting showed me how I could fix the issue, but I realized that relying on someone else's code so much had left me with a site I'd have a hard time supporting over the long term... which is kind of the opposite of the point of using an SSG and GitHub Pages. Like many a nerd before me, I just wanted to understand exactly what was going on.

### Option Two: Roll Your Own Action

I didn't know a lot about GitHub Actions going into this, but it turns out the page publishing workflow is pretty simple, and [pretty well-documented by GitHub](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow). 

So I [made my own](https://github.com/juniorbird/wadearmstrong.com/blob/main/.github/workflows/main.yml), and it was pretty simple.

The top does two things:

```yaml
on:
  push:
    branches:
      - main

  # Must have `on: workflow_dispatch` to enable manual run from Actions tab
  workflow_dispatch:
```

Initially, I set the Action to run when there's a push to `main`. I also have `workflow_dispatch`, because, without that, there's no button to manually run the action.

```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

This sets up permissions for the Action and the token it will get.

```yaml
# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true
```

It's not clear to me that this is relevant to my needs, but, certainly if one were running a complex Action, this would be needed. I used it because I did go wild with `git push`es once and overran my hourly rate limit for GitHub Actions.[^3]

Now I run the first step of the process, the step that builds the pages.

```yaml
build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [14.x]

    steps:
      - name: Setup pages
        id: pages
        uses: actions/configure-pages@v3

      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Node ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
```

This starts the set-up for building the site. I'm running 3 Actions that are pre-built by GitHub:

* [Configure Pages](https://github.com/actions/configure-pages) — This can do a lot more than I'm using it for, but the basic config is all that's needed here
* Checkout
* Setup Node

Having completed this setup, I can actually run Node commands:

```yaml
	- name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build
```

`build` is an action that I set up in my `package.json`, use whatever command you use to create your site.[^4]

Here, the site is built; but I need to ensure it's ready for GitHub's Pages Servers.

```yaml
	- name: Fix permissions
        run: |
          chmod -c -R +rX "_site" | while read line; do
            echo "::warning title=Invalid file permissions automatically fixed::$line"
          done

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
            path: ${{ github.workspace }}/_site

```

The permissions fix is something I can probably get rid of, but some documentation called it out as potentially needed.

The [`Upload Artifact`](https://github.com/actions/upload-artifact) step `tar`s up an archive of the created site in the format Pages servers require.

Now we flow into the second step, deploying the site. This is much simpler than the build.

```yaml
 deploy:
    runs-on: ubuntu-latest

    needs: build

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - name: Deploy to GitHub pages
        id: deployment
        uses: actions/deploy-pages@v2
```

Having built the site, I tell the [`deploy`](https://github.com/actions/deploy-pages) action to actually put it on there, and provide the output as a configuration argument for the Pages environment. 

And that's it! A site of my own! Now to make it good...


[^1]: And I know if I hosted the site on S3 somehow I'd make it cost me $150/month.
[^2]: Or, you know, occasionally building them and then caching them very aggressively
[^3]: It's just [10 builds per hour](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#usage-limits)
[^4]: My [package.json](https://github.com/juniorbird/wadearmstrong.com/blob/main/package.json) basically has the build do some cleanup and setup actions, as well as build only non-`draft` posts.