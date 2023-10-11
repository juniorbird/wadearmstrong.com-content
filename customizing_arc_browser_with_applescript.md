---
title: Customizing the Arc Web Browser Using Workspaces and AppleScript
description: So you want to have a customizable site that takes the least effort possible and is free to host... here's how to publish your 11ty site on GitHub Pages!
date: 2023-10-11
layout: layouts/base.njk
tags:
  - posts
---
# Customizing the Arc Web Browser Using Workspaces and AppleScript

I've recently gotten really into the [Arc Web browser](https://arc.net/), and in particular how its [Spaces](https://resources.arc.net/en/articles/6318861-spaces-distinct-browsing-areas) help me organize my work[^1] But, I also like the amazing battery life Safari gives me,[^2] plus some of its other clever features.[^3]

I also have come to rely on [Workspaces](https://www.apptorium.com/workspaces), a nifty launcher that opens the apps and Web sites I need for different kinds of works — and closes the things that will distract me (or just take up memory, _cough_, Docker). I got Workspaces with my [Setapp](https://www.apptorium.com/workspaces) subscription, so you may prefer another tool, but I recommend it if you're looking for something.

Anyway, I wanted to use Workspaces to automatically switch my default browser _and_, if I was using Arc, switch to the Space I wanted.

## Switching Default Browsers

My first idea was to try to [`defaults set`](https://macos-defaults.com/) something, but it turns out that's not how MacOS stores your Web browser. There _is_ a API to set it, but I'm no ace with Swift[^4] so I didn't want to deal with that.

Turns out you can just [`brew install`](https://brew.sh/)[`defaultbrowser`](https://github.com/kerma/defaultbrowser) and get a nifty command-line utility that will tell you the browsers you have; and help you switch. 

However, MacOS — for good and desirable security reasons! — will pop a dialog asking you if you really want to switch your browser. That's unnecessary, since this is a user-initiated change. So, I reached for AppleScript to auto-press ok in the dialog for me. But I don't write AppleScript all that often,[^5] so of course I had to google button pushing.

That's when I discovered [a blog post on how to do exactly this thing](https://www.felixparadis.com/posts/how-to-set-the-default-browser-from-the-command-line-on-a-mac/). He even ended up with the same toolchain!

The main things I needed that Felix Paradis didn't already give me were:

1. Be able to pass arguments to set both the browser and Space
2. Have Arc set the Space

So I added all that to his code and ended up with [this](https://github.com/juniorbird/small-tools/blob/main/set-browser.applescript). Let's break it down:

```applescript
on run argv
	local browser, profile, theBrowser
	set theArgs to argv

	try
		set theBrowser to item 1 of argv
	on error
		display dialog "You must pass a browser"
	end try

	try
		set profile to item 2 of argv
	on error
		set profile to ""
	end try
```

`on run` is basically `main()` for AppleScript. `argv` passes in arguments.

Oh, but AppleScript throws a fit if you test for an argument that's not there. So, here I wrap the argument-handling in a `try...catch` to not blow up.

```applescript
if (theBrowser = "Arc")
	set browser to "browser"
else if (theBrowser = "Safari")
	set browser to "safari"
end if
```

A lot of Chrome-based browsers report themselves as `browser`. I haven't checked to see what would happen if I also had Brave around, since that is one of the other `browser` offenders. Probably something bad, but that's a problem for future me.

AppleScript doesn't really have a Dictionary[^6] or Object or Struct or something like that where you can pass in a string and get back some other value, so we have a bunch of `if/else` statements. There will be more later, and I'm not sorry.

The bit after this is directly copied from [Felix Paradis](https://www.felixparadis.com/posts/how-to-set-the-default-browser-from-the-command-line-on-a-mac/), and he explains it pretty well, so let's skip on to...

```applescript
delay 1
try
	if (theBrowser is "Arc")
		if (profile is not "")
			tell application "Arc"
				tell last window
					tell space profile to focus
				end tell
			end tell
		end if
	end if
end try
```

`delay 1` just means "wait for the browser to be launched, please." I tell the `last window` because, if *no* window is focused, there's no `first window`, and Arc will open a new window; I assume here that the user's last window is likely to be the one they care about.

I do have to say I really appreciate that Arc's API supports named Spaces without me having to look up some other identifier for the Space. Makes it easy to switch to the space "Writing Code"! Technically this also works if the user passes in the number of the Space, but I'd personally rather use a name than a number.

That's all this is. It's just AppleScript, and, if you're on a Mac, learning a little AppleScript will get you just as far as learning a little bash does if you're a developer in general. It's good for glue.

[^1]: And also how it centers the idea of keeping tabs organized... I don't seem to end up with 60 open tabs in 3 windows with Arc.

[^2]: I know Chrome has supposedly closed the gap, but I still get noticeably more battery life watching non-YouTube videos on my M1 MacBook Air using Safari — like, > 20% better. YMMV.

[^3]: Handoff to send Web pages back and forth from my iPhone and Apple Pay integration are tops on my list.

[^4]: And definitely not Objective-C.

[^5]: Hey, I _like_ bash scripting. It can be clunky, but it's really portable and you can do a ton with it.

[^6]: But AppleScript does have a thing called a "Dictionary," which is a way that any app that has an AppleScript API can provide that API to the AppleScript editor. It's a great feature, but it makes it entirely impossible to google "AppleScript Dictionary" when you mean a data type.