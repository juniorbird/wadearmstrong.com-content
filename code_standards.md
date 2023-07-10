---
title: Code Standards
description: In my experience, these are standards that make code better, easier-to-maintain, more pleasant to deal with.
date: 2019-07-04
layout: layouts/base.njk
tags:
  - posts
---
# Recommended Code Standards

In my experience, these are standards that make code

* Better
* Easier-to-maintain
* More pleasant to deal with

Try to use these as you write code. While the world is never nice and clean, and you'll break all of these sometime, try to at least do so consciously (and explain why, in your PR).


## Core Concepts

* Maintain a good [code line of sight](https://medium.com/@matryer/line-of-sight-in-code-186dd7cdea88)
* Keep the happy path obvious
* Return early
* Don't reinvent the wheel
* Override the least possible, and contribute the most possible back to the core
* When creating something new, make it possible to override
* Code should be easy for the next developer to understand
* Prefer clarity and readability to brilliance or cleverness

## Applying These Concepts To Details

### Rules of Thumb

* Rather than

```javascript
if (happy) {
  do 1st happy thing;
  ...
  do nth happy thing;
} else {
  throw tantrum();
}
```

try

```javascript
if (!happy) {
 throw tantrum();
}
do 1st happy thing;
...
do nth happy thing;
```

* If you do the above, you probably don't need an `else`
* The above is a really good way to `return` early, before you do any real work, when you're outside the happy path
* If there are several `if` s in a row, consider a `switch` statement
* Name things well. Be clear about what data they contain/tasks they do. Related things have related names
* Always return from functions. Always return the same datatype from a given function
* Break out reusable behavior into its own function, but
* Don't put an excessive number of functions on the call stack
* Your class should have an interface like similar classes; your function should have a signature like similar functions
* Make things configurable and those configurations overridable

### Usefulness

* Did you really need to override the _entire_ template?
* Are any magic numbers set in a config file? Can that config file be overridden?

### Readability

* Are all of your functions explained in docblocks? Do the explanations actually match the current code, and are all parameters actually documented?
* Do not nest ternaries. Save your logical comparisons in well-named variables and compare those
* Don't rely on the next developer to remember the order of arithmetic or logical operations; group parts of statements with parentheses to make your meaning obvious
* Comments explain _why_ you did it, not _what_ it does
* Functions do one thing
* Four parameters for a function is probably too many
* You should be able to read the entire method/function without scrolling
* Don't Repeat Yourself
* Constants are UPPERCASE even if you `const` them
* Write comments just like you'd explain something to another developer; don't feel like you need to use fancy, formal language

### Performance

* Do you save the value of expensive calculations in a variable, and then reference that variable? Are even cheap calculations not done once per loop iteration when they could be done just once?
* Do you use temporary variables inside loops, to make it easy to understand what the calculation is?
* For Javascript, do you declare variables outside loops, and update them inside?
* Are you sure that loop will end?

### Security

* If you're accepting input from the user, an API, or another component, do you validate that input is safe and of the right type, length, etc.?
* If you get bad input, do you handle it well?
* What happens if this gets passed a string containing Unicode characters? Does it not just handle it, but handle it correctly?
* What happens if someone passes a `<script>` tag into your code?
* If you're using PHP, *all* DB access should be via PDO. [Don't concatenate your own SQL statement](https://xkcd.com/327/).

### Testing

* Do your tests cover both the happy path and **all** error paths?
* Do your tests detect breakage?
* Are your tests brittle? Will they fail if someone changes an html attribute or the order in which attributes are assigned to an object? Do they have to fail under those circumstances?
* If you're testing with strings, have you tested with unicode strings?
* Do your unit tests actually test units, and not modules in integration?

### Cleanliness

* Don't leave in TODOs, ticket them up
* Delete commented-out code in a PR
* Each PR should only include commits that it's safe to roll back to; squash your commits before making a PR

### Formatting

* If writing PHP, generally follow [PSR2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md), except for a few things:
  * Tabs are clearly superior to spaces, so we use them
  * `TRUE`, `FALSE`, and `NULL` are generally capitalized, although this is not required
* If writing Javascript, you *must* be running [ESlint](https://eslint.org/) and using an agreed-upon and shared config. If you do not lint your code then you will be mocked ceaselessly for any style violations
* `variablesAreCamelCase`
* `indexes['are_snake_case']`
* `_` doesn't actually make your method or variable private so don't use it
