---
title: "Node.js - Threading basics"
date: 2020-06-05 00:18:00 +0900
categories: updated
comments: true
---

I have been using Node.js for years, and always thought 'node.js' runs single thread and while asynchronous happens, magic makes it faster. Of course I was aware of event loops but, not any deeper than existence. I took this opportunity to study a bit further and try to understand what internet gurus teach me.

## Its single but multi

If someone asks you how many threads does node uses, the best answer is to ask 'when'. When I read the post from [preezma][link1], I thought this is great answer to step forward and understand node.js' structure.

![node.js structure](/assets/img/Node.png)

We're not going deeper than this diagram. So When we call Node.js a single-treaded application, we determine that Node.js has only one event loop. And that is totally true! But if we look further how event loop deal with every queued events, occationally we found POSIX Async _Threads_(which seems to be a plural), and you would notice that node.js is consist of V8 Runtime and, a C++ library: [_libuv_][link2].

Briefly, this amazing C++ library can run a event pool over various different OSs(That's why it is called POSIX). And it has a thread pool (which has 4 threads as default) that can handle different tasks asynchronously.

Then, why is usually said: single-thread application? Simply said, Multi-threads programming can be fishy with memory allocation, and Node.js handled this dilemma with existing answer from _libuv_. Node.js will throw any asynchronous tasks to event loop, and event loop will run a thread pool which shares memory addresses. Problem solved, and We're using multi-threads in ease.

## Non-blocking I/O

We found that one _single_ thread for event loop makes node.js possible to use asynchronous _multi_ threads. Next step is Non-blocking I/O accessing data. What is Non-blocking I/O and why is this essential?

These are examples from [Node.js document][link3]:

Synchronous, Blocking I/O:

```javascript
const fs = require('fs');
const data = fs.readFileSync('/file.md'); // blocks here until file is read
moreWork(); // This runs after constant data is loaded
```

Asynchronous Non-Blocking I/O:

```javascript
const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
});
moreWork(); // This runs right after fs.readfile() is thrown to event loop.
```

Unlike legacy JS, understanding asynchronous functions is very essential in Node.js because it allows you to make efficient program. As you see the code above, ```moreWork()``` was loaded as soon as fs.readFile (async function) was loaded to event loop. This allows your application to work efficient with both time and space.

With Non-blocking I/O, you can understand every Node.js' design patterns and newer standards such as _Promise_ or _async/await_. And this is what makes Node.js unique. (And difficult...)

## The answer is YES: both A and B

When I learned what I've written above, I just thought how modern programming is getting similar to mother nature. Some scientists and programmers even say that asynchronous function seems like a quantum, and I really liked that description.

When you print Promise object right after calling async function, it prints a simple Promise object without any returns observed. Asynchronous function will return data when threads pool finishes the work. Even with _EventEmmiter_ one function can have more than one return, and various errors can be detected in one call. (like Observer effect, light can have particle properties when it is observed)

![Light can have particle properties when it is observed](/assets/img/The-Observer-Effect.png)

So while we program with Promise object, we assume to have both error and result and code every cases. And that is quite quantum-ish experience to me.

[link1]: https://medium.com/preezma/node-js-event-loop-architecture-go-deeper-node-core-c96b4cec7aa4
[link2]: http://docs.libuv.org/en/v1.x/
[link3]: https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/
