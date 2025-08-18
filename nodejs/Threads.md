# Threads

## Child Process

The `child_process` module gives node the ability to run the child process, established through IPC (inter-process communication) by accessing operating system commands.

**The 3 main processes inside this module are:**

1. `child_process.spawn()`
2. `child_process.fork()`
3. `child_process.exec()`

### Example Usage

```js
import { spawn } from 'node:child_process';
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`child process exited with code ${code}`);
});
```

---
## Cluster

The `cluster` module allows you to easily create child processes that each runs simulatenously on their own single thread, to handle workloads among their application threads.

```js
import cluster from 'node:cluster';
import http from 'node:http';
import { availableParallelism } from 'node:os';
import process from 'node:process';

const numCPUs = availableParallelism();

if (cluster.isPrimary) {
  console.log(`Primary ${process.pid} is running`);

  // Fork workers.
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`worker ${worker.process.pid} died`);
  });
} else {
  // Workers can share any TCP connection
  // In this case it is an HTTP server
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
  }).listen(8000);

  console.log(`Worker ${process.pid} started`);
}
```

---
## Worker Threads

Worker threads is a continous parallel thread that runs and accepts messages until it is explicity closed or terminated. 

> **NOTE \#1**: With worker threads, we can achieve a much more efficient application without created a deadlock.

> **NOTE \#2**: Workers unlike children’s processes, can exchange memory.

**Example:**

```js
import {
  Worker,
  isMainThread,
  parentPort,
  workerData,
} from 'node:worker_threads';

if (!isMainThread) {
  const { parse } = await import('some-js-parsing-library');
  const script = workerData;
  parentPort.postMessage(parse(script));
}

export default function parseJSAsync(script) {
  return new Promise((resolve, reject) => {
    const worker = new Worker(new URL(import.meta.url), {
      workerData: script,
    });
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0)
        reject(new Error(`Worker stopped with exit code ${code}`));
    });
  });
};
```

---

#nodejs