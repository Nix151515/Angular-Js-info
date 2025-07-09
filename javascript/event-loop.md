## Event loop
<img src="event loop.webp">


The **event loop** in JavaScript is a fundamental concept that enables **non-blocking asynchronous operations** despite JavaScript being **single-threaded** (i.e., it can only do one thing at a time).

---

### 🧠 How It Works

JavaScript has a **runtime model** based on:

* The **Call Stack**
* The **Event Queue (aka Callback Queue)**
* The **Web APIs** (provided by the browser)
* The **Microtask Queue**
* The **Event Loop**

---

### 🔁 Event Loop Step-by-Step

1. **Call Stack**

   * Executes function calls.
   * If a function calls another, it's stacked.
   * When a function finishes, it's popped off the stack.

2. **Web APIs** (in browsers)

   * Handles asynchronous tasks (e.g., `setTimeout`, DOM events, fetch requests).
   * When complete, the callback is sent to a queue.

3. **Callback Queue (Task Queue)**

   * Holds messages/callbacks ready to run once the call stack is empty.

4. **Microtask Queue**

   * Used for promises and `queueMicrotask`.
   * Has **higher priority** than the callback queue.

5. **Event Loop**

   * Constantly checks:

     * Is the call stack empty?
     * If yes, it processes:

       1. All microtasks.
       2. Then one task from the callback queue.
   * Repeats endlessly.

---

### 📊 Visual Example

```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');
```

**Output:**

```
Start
End
Promise
Timeout
```

**Why?**

* `console.log('Start')` → Call stack
* `setTimeout(..., 0)` → Timer → Task queue
* `Promise.then(...)` → Microtask queue
* `console.log('End')` → Call stack
* Then:

  * Microtasks run first → "Promise"
  * Then the event loop picks from the callback queue → "Timeout"

---

### 🔧 Summary

| Component       | Role                                                 |
| --------------- | ---------------------------------------------------- |
| Call Stack      | Executes code line-by-line                           |
| Web APIs        | Handles async operations (timers, fetch, events)     |
| Microtask Queue | Stores resolved promises, runs before callback queue |
| Callback Queue  | Stores async callbacks from Web APIs                 |
| Event Loop      | Manages execution order                              |

---



