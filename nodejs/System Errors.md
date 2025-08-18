# System Errors

NodeJS generates system errors when exceptions occur within its runtime environment. These usually occur when an application violates an OS constraint.

**Common systems encountered when writing a NodeJS program:**

- `EACCES` (Permission denied): An attempt was made to access a file in a way forbidden by its file access permissions.
* `EADDRINUSE` (Address already in use): An attempt to bind a server (~[net](https://nodejs.org/api/net.html)~, ~[http](https://nodejs.org/api/http.html)~, or ~[https](https://nodejs.org/api/https.html)~) to a local address failed due to another server on the local system already occupying that address.
* `ECONNREFUSED` (Connection refused): No connection could be made because the target machine actively refused it. This usually results from trying to connect to a service that is inactive on the foreign host.
* `ECONNRESET` (Connection reset by peer): A connection was forcibly closed by a peer. This normally results from a loss of the connection on the remote socket due to a timeout or reboot. Commonly encountered via the ~[http](https://nodejs.org/api/http.html)~ and ~[net](https://nodejs.org/api/net.html)~ modules.
* `EEXIST` (File exists): An existing file was the target of an operation that required that the target not exist.
* `EISDIR` (Is a directory): An operation expected a file, but the given pathname was a directory.
* `EMFILE` (Too many open files in system): Maximum number of ~[file descriptors](https://en.wikipedia.org/wiki/File_descriptor)~ allowable on the system has been reached, and requests for another descriptor cannot be fulfilled until at least one has been closed. This is encountered when opening many files at once in parallel, especially on systems (in particular, macOS) where there is a low file descriptor limit for processes. To remedy a low limit, run ulimit -n 2048 in the same shell that will run the Node.js process.
* `ENOENT` (No such file or directory): Commonly raised by ~[fs](https://nodejs.org/api/fs.html)~ operations to indicate that a component of the specified pathname does not exist. No entity (file or directory) could be found by the given path.
* `ENOTDIR` (Not a directory): A component of the given pathname existed, but was not a directory as expected. Commonly raised by ~[fs.readdir](https://nodejs.org/api/fs.html#fsreaddirpath-options-callback)~.
* `ENOTEMPTY` (Directory not empty): A directory with entries was the target of an operation that requires an empty directory, usually ~[fs.unlink](https://nodejs.org/api/fs.html#fsunlinkpath-callback)~.
* `ENOTFOUND` (DNS lookup failed): Indicates a DNS failure of either `EAI_NODATA` or `EAI_NONAME`. This is not a standard POSIX error.
* `EPERM` (Operation not permitted): An attempt was made to perform an operation that requires elevated privileges.
* `EPIPE` (Broken pipe): A write on a pipe, socket, or FIFO for which there is no process to read the data. Commonly encountered at the ~[net](https://nodejs.org/api/net.html)~ and ~[http](https://nodejs.org/api/http.html)~ layers, indicative that the remote side of the stream being written to has been closed.
* `ETIMEDOUT` (Operation timed out): A connect or send request failed because the connected party did not properly respond after a period of time. Usually encountered by ~[http](https://nodejs.org/api/http.html)~ or ~[net](https://nodejs.org/api/net.html)~. Often a sign that a `socket.end()` was not properly called.

---

#nodejs