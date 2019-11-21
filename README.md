## Description

[`process.umask()`](https://nodejs.org/api/process.html#process_process_umask_mask) "sets or returns the Node.js process's file mode creation mask". 

A number of packages depend on `process.umask()` returning a mask, but [worker threads](https://nodejs.org/api/worker_threads.html) on Node v10 (and potentially other environments) don't provide `process.umask()`, leading to this error:

```
TypeError: process.umask is not a function
```

This library monkey patches `process` to provide the `umask()` method, returning a default mask that should allow your code to proceed without error.

## Usage

First,

```bash
npm install --save @dylburger/umask
```

Then, wherever you need access to `process.umask()`, add this at the top of your code:

```js
require("@dylburger/umask")()
```


## Notes

[A patch](https://github.com/nodejs/node/pull/25526) was introduced to provide a read-only stub of the `process.umask()` method in worker threads on Node v12 and up. I've asked the Node team if they're willing to provide a backport of this functionality to Node v10, but haven't heard back as of this commit.
