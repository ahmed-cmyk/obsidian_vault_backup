# Working with Files

## Return working directory

The `process.cwd()` method returns the current working directory of the NodeJS process i.e., the directory which you invoke the `node` command.

> **NOTE**: `process.cwd()` is synonym to `.` for all cases except for `require()` which works relative to current executing file.

`__dirname` returns the directory name of the directory containing the JS source code file. It gives you the **absolute path to the folder (directory) where the current file lives**.

`__filename` and `__dirname` are used to get the filename and directory name of the currently executing file.

---
## `path` module

You can require this module in your files using:

```js
const path = require('node:path');
```

Or, you can import it using:

```js
import path from 'node:path';
```

### Getting information out of a path

You can use the `path` module to extract information such as what is seen below:

```js
import path from 'node:path';

const notes = '/users/joe/notes.txt';

path.dirname(notes); // /users/joe
path.basename(notes); // notes.txt
path.extname(notes); // .txt
```

You can get the file name without the extension by specifying a second argument to `basename`:

```js
path.basename(notes, path.extname(notes)); // notes
```

### Working with paths

You can join two or more parts of a path by using `path.join()`:

```js
const name = 'joe';
path.join('/', 'users', name, 'notes.txt');
```

You can get the absolute path calculation of a relative path using `path.resolve()`:

```js
path.resolve('joe.txt");
```

In this case Node.js will simply append `/joe.txt` to the current working directory. If you specify a second parameter folder, resolve will use the first as a base for the second:

```js
path.resolve('tmp', 'joe.txt'); // '/Users/joe/tmp/joe.txt' if run from my home folder
```

If the first parameter starts with a slash, that means it's an absolute path

```js
path.resolve('/etc', 'joe.txt'); // '/etc/joe.txt'
```

`path.normalize()` is another useful function, that will try and calculate the actual path, when it contains relative specifiers like `.` or `..`, or double slashes:

```js
path.normalize('/users/joe/..//test.txt'); // '/users/test.txt'
```

> **NOTE**: Neither `resolve` nor `normalize` will check if the path exists. They just calculate a path based on the information they got.

---
## `fs` module

File System or `fs` module is a built in module in Node that enables interacting with the file system using JavaScript. All file system operations have synchronous, callback, and promise-based forms, and are accessible using both CommonJS syntax and ES6 Modules.

---

#nodejs