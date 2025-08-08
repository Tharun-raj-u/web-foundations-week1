# JavaScript Runtime, Event Loop, and Execution Order

## Objective

Understand how JavaScript runs code behind the scenes. Learn how the call stack, event loop, microtasks, and macrotasks work together. This helps you understand how async behavior affects real-world apps.

---

## Call Stack and Synchronous Code

JavaScript runs one thing at a time using something called the **call stack**. It works like a stack of plates — the last one added is the first one removed.

Example:

```js
function greet() {
  console.log("Hello");
}
greet();
```

- `greet()` is added to the stack.
- Inside it, `console.log()` is also added and runs.
- Once done, both are removed from the stack.

This is how **synchronous** (normal) code runs — line by line.

---

## Microtasks and Macrotasks

Some code runs **later**, not right away. These are scheduled as tasks.

**Microtasks:**
- Run right after the current code is done.
- Examples: `Promise.then()`, `queueMicrotask()`

**Macrotasks:**
- Run after microtasks.
- Examples: `setTimeout()`, `setInterval()`

Microtasks always run before macrotasks.

---

## Promises, `queueMicrotask()`, and `setTimeout()`

These behave differently because they’re scheduled in different queues:

- `Promise.then()` → microtask
- `queueMicrotask()` → microtask
- `setTimeout()` → macrotask

Even if `setTimeout` has 0 delay, it waits for microtasks to finish first.

---

## How the Event Loop Works

1. Run all normal (synchronous) code first.
2. Then run all microtasks.
3. Then run one macrotask.
4. Go back to step 2.

---

## Exercise 1

```js
console.log('1');

setTimeout(() => {
  console.log('2');
}, 0);

Promise.resolve().then(() => {
  console.log('3');
});

queueMicrotask(() => {
  console.log('4');
});

console.log('5');
```

### What it prints:

```
1
5
3
4
2
```

### Why?

- `1` and `5` are synchronous — they run right away.
- `3` and `4` are microtasks — run after sync code.
- `2` is a macrotask — runs last.

---

## Exercise 2

```js
console.log('start');

setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => console.log('Promise 1'));
}, 0);

setTimeout(() => {
  console.log('setTimeout 2');
}, 0);

Promise.resolve().then(() => console.log('Promise 2'));

console.log('end');
```

### What it prints:

```
start
end
Promise 2
setTimeout 1
Promise 1
setTimeout 2
```

### Why?

- `start` and `end` are sync.
- `Promise 2` is a microtask.
- `setTimeout 1` runs first as a macrotask.
  - Inside it, `Promise 1` is scheduled and runs as a microtask.
- Then `setTimeout 2` runs.

---

## Key Takeaways

- JS runs code in this order: sync → microtasks → macrotasks
- Microtasks always run before macrotasks.
- This helps in writing smooth and predictable apps.
