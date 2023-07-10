---
title: CommonJS, ES Modules, and Why to Test Natively in One or the Other
description: What is the difference between CommonJS and ES Modules, and how does this impact what unit test frameworks you can use?
layout: layouts/base.njk
date: 2023-05-12
keyboardShortcut: 'foo'
tags:
  - posts
---
# CommonJS, ES Modules, and Why to Test Natively in One or The Other

In Javascript, there are two different ways that a given file can export functions, objects, values, and other components, for reuse in a second file or files — CommonJS ('CJS') and ES Modules ('ESM'). These two methods of export are actually surprisingly different, because they were designed for use in completely separate circumstances. 

## Where You'll See The Difference

If you've worked in Javascript for a while, you've probably become used to the [`require()`](https://nodejs.org/docs/latest-v20.x/api/modules.html#requireid) syntax for including the contents of other files, and the [`module.exports`](https://nodejs.org/docs/latest-v20.x/api/modules.html#moduleexports) syntax for exporting features from a file:

```javascript
const { importedModule } = require('/path/to/module.js')

let value = importedModule(input);

module.exports = {
	value
} 
```

That's a tip-off that you're using CommonJS; ESM uses [`import`](https://tc39.es/ecma262/#sec-imports) and [`export`](https://tc39.es/ecma262/#sec-exports) for the same jobs:

```javascript
import { importedModule } from '/path/to/module.js'

let value = importedModule(input);

export default value;
```

For most developers, this will be the only time they see a difference between the two approaches. Which syntax you use is probably going to be defined by the frameworks you're using:

* Node natively supports CJS, but has supported ESM since v.12
* React supports both, but [create-react-app](https://create-react-app.dev) uses ESM (which one you choose will primarily be defined by your bundler)
* Angular supports both, but strongly prefers ESM
* Vue switched to ESM in v.13

While technically you can mix CJS and ESM in a single project,[^1] that's a good way to cause build chain problems. You can't practically mix the two syntaxes in a single file[^2].

## What Is The Difference Under The Hood

But it's not just build chains that have trouble with the two different export formats — there are actually significant technical differences in the way that ESM and CJS are implemented.

### Why CommonJS is Here 

First, a little history. When Node first came around, there was no standardized way to include components from different files. They chose CommonJS, one of the major proposals at the time, not least because it was straightforward to use on servers[^3].

One thing that made it so straightforward on servers was that it loads all included modules _synchronously_ — that is, it kind of assumes that the modules will just 'be there' when it hits the `require()` line in the code. This is true on servers, so that's a great choice for Node.

### Why ES Modules are Here

Of course, on the Web, files are not at all 'just there' — they take a while to load from wherever in the world they are hosted. CJS's synchronous approach to loading would result in the browser appearing to stall as each dependency was loaded, in order; an asynchronous approach, matching the way assets like CSS and images are loaded in the browser, was clearly required.

That's where ES Modules come in. They implement something called 'top level await', which is basically just a way of saying "You know how [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) works? Every ES Module is by default wrapped in that."

### But it's Not Just Synchronous vs. Asynchronous

Understanding synchronous vs. asynchronous is a good first step, but the real difference under the hood — and the reason why you want to test natively in whatever exports approach your build chain uses — comes down to the consequences of being synchronous or asynchronous.

Imagine your Javascript interpreter parsing a single file. It hits a line like:

```javascript
let foo = 47;
```

The interpreter can then:

1. Allocate memory for the value of `foo` to inhabit
2. Populate that memory with `47`
3. Move on and be done with it

Nice and simple, right? But what if the value `47` isn't there yet?

```javascript
let foo = await fetchFortySeven()
```

If you've used [`Promises`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) before, you'll know that the interpreter works more like:

1. Allocate memory for the value of `foo` to inhabit
2. Populate that memory with a Promise
3. Move on to the next thing
3. Check periodically in the Event Loop for the Promise to be fulfilled
4. ...
5. ...
6. Now finally populate the memory with `47`

This pretty much describes the under-the-hood difference between CJS and ESM.

* CJS immediately and synchronously processes all exports and sets their value in memory at that time
* ESM asynchronously processes all exports and sets their value in memory when all required processes (loading, calculating, etc.) have been completed

As a consequence of this:

* CJS exports _can never_ be recalculated during a given execution; they are what they are at the initial export time. So, if you have a module that exports a default; waits for a Promise to resolve; then exports the new value, the CJS import will only ever get the default value. 
* ESM exports _can_ be recalculated during a given execution. In the previous example, the exported value would change during the execution, and you would be able to access the new value.

## An ESM Testing Toolchain

Obviously this means that you need to be careful in testing — you want to write tests that check the _actual_ value the code will produce in production, and it's a problem if the tests assume the value will never change and it does (or vice versa). So stick to a testing toolchain that natively uses whatever module format your build chain exports.

Facebook's excellent Jest testing framework [does not yet support ES Modules well](https://jestjs.io/docs/ecmascript-modules), so, while it's a great choice for CJS build chains, you should avoid it for ESM.

For ES Modules, I like the Mocha framework, which has [supported ESM since mid-2020](https://mochajs.org/#nodejs-native-esm-support). It doesn't offer all of the great React-specific features you get for free in Jest, but it's a solid, flexible, and fast framework.

Now you know the difference between ES Modules and CommonJS. Go out, and reuse your code!

[^1]: And you may have, depending on the npm modules you use.
[^2]: You're basically using CJS and `require`ing external files to give you the ESM interfaces, and your build chain will need to reflect that you're using both, so you'd really better be determined to mix it up!
[^3]: The syntax for CJS is also, to my eyes at least, [cleaner than that of an early competitor, AMD](https://requirejs.org/docs/whyamd.html#amd)