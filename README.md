# queue.js

This is a folk of mbostock's queue.js that vastly overcomplicates
the original module by requiring you to explicitly state where the
callback argument is supposed to go

 **Queue.js** is yet another asynchronous helper library for JavaScript.
 Think of it as a minimalist version of [Async.js](https://github.com/caolan/async)
 that allows fine-tuning over parallelism. Or, think of it as a version of
 [TameJs](http://tamejs.org/) that does not use code generation.

For example, if you wanted to stat two files in parallel:

```js
queue()
    .defer(fs.stat, __dirname + "/../Makefile", queue.D)
    .defer(fs.stat, __dirname + "/../package.json", queue.D)
    .await(function(error, results) { console.log(results); });
```

Or, if you wanted to run a bazillion asynchronous tasks (here represented
as an array of closures) serially:

```js
var q = queue(1);
tasks.forEach(function(t) { q.defer(t, queue.D); });
q.await(function(error, results) { console.log("all done!"); });
```

Queue.js can be run inside Node.js or in a browser.

## API Reference

### queue([parallelism])

Constructs a new queue with the specified *parallelism*. If *parallelism*
is not specified or is *0*, the queue has infinite parallelism. Otherwise,
*parallelism* is a positive integer. For example, if *parallelism* is 1,
then all tasks will be run in series. If *parallelism* is 3, then at most
three tasks will be allowed to proceed concurrently; this is useful, for
example, when loading resources in a web browser.

### queue.defer(method[, arguments…])

Adds the specified *method* to the queue, with any optional *arguments*.
Use *queue.D* as the placeholder for the callback.

### queue.await(callback)

Sets the *callback* to be notified when all deferred tasks have finished.
