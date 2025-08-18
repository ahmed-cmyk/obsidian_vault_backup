# setTimeout and setInterval

## setTimeout

The `setTimeout` runs a function after the specified period expires. Times are declared in milliseconds.

```js
// setTimeout schedules a function to run *once* after a delay

console.log("Before setTimeout");

// This function will run *after 2000ms (2 seconds)*
setTimeout(() => {
  console.log("Hello after 2 seconds!");
}, 2000);

console.log("After setTimeout");
```

**Output:**

```js
Before setTimeout
After setTimeout
Hello after 2 seconds!
```

**Notes:**
* setTimeout does **not block** the program — it schedules the function to run later.
* First two console.log lines run immediately.
* After ~2 seconds, the callback function runs.

## setInterval

The `setInterval()` method helps us to repeatedly execute a function after a fixed delay. It returns a unique interval ID which can later be used by the `clearInterval()` method, which stops further repeated execution of the function.

`setInterval()` is similar to `setTimeout`, with a difference. Instead of running the callback function once, it will run it forever, at the specific time interval you specify (in milliseconds):

```js
// setInterval schedules a function to run *repeatedly* at fixed intervals

let count = 0;

const intervalId = setInterval(() => {
  count++;
  console.log(`Interval message ${count}`);

  // Stop the interval after 5 times
  if (count >= 5) {
    clearInterval(intervalId); // stops the interval
    console.log("Interval stopped.");
  }
}, 1000); // runs every 1000ms (1 second)
```

```js
Interval message 1
Interval message 2
Interval message 3
Interval message 4
Interval message 5
Interval stopped.
```

**Notes:**
* setInterval keeps calling the function every 1000ms until stopped.
* clearInterval(intervalId) stops it.
* Useful for repeated actions like timers, polling, animations, etc.

---

#nodejs