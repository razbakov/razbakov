---
title: ECMAScript const objects are references
description: A note about const in JavaScript, which is not a constant, but a way to solve some JavaScript problems related to block scopes.
date: 2018-06-29
category: WebDev
permalink: /blog/js-const-objects-are-references
---

A note about const in JavaScript, which is not a constant, but a way to solve some JavaScript problems related to block scopes.

Read more:

[MDN. JavaScript. const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

> The `const` declaration creates a read-only reference to a value. It does not mean the value it holds is immutable, just that the variable identifier cannot be reassigned. For instance, in the case where the content is an object, this means the object's contents (e.g., its parameters) can be altered. (edited)

[ES2015 const is not about immutability](https://mathiasbynens.be/notes/es6-const)

> `const` is not about immutability

> `const` makes the contract that no rebinding will happen

[Constant Variables in JavaScript, or: When "const" Isn't Constant](https://blog.mariusschulz.com/2015/12/31/constant-variables-in-javascript-or-when-const-isnt-constant)

> Unfortunately, the name of the `const` keyword might be misleading. In JavaScript, const does not mean constant, but one-time assignment. It's a subtle yet important distinction. Let's see what one-time assignment means..
