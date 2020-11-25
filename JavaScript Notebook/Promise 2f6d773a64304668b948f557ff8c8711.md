# Promise

## Key Points

- `new Promise()`
- `then()` and `catch()`
- Promise execution chain
- Microtask queue
- `Promise.all` and `Promise.race`

## Promise

- Promise is an object that we can use it to perform asynchronous events.

### Why Promise?

- It avoids asynchronous concurrency in JavaScript.
- It solves the call-back hell

### Promise Status

**Pending → (event happens) → settled / resolved → fulfilled or rejected**

- Once a promise is settled, its status cannot be changed anymore. (fulfilled or rejected)
    - Pending → Fulfilled
    - Pending → Rejected

### Create an Instance of Promise

```jsx
let promise = new Promise(function(resolve, reject) {
	// Your asynchronous event...
	if (//successful) {
		resolve(value);
	} else {
		reject(error);
	}
}
```

When we instantiate a new promise — the callback function is running **synchronously.**

- The `constructor` of `Promise` takes a function (***executer***) with two functions as the arguments - `resolve` and `reject`.

    `resolve`

    - It can move the status of promise from ***pending*** to 'successful' (***fulfilled***).
    - When the asynchronous event succeeds, we can put the value that the asynchronous event returned into `resolve` as the parameter, and consume this value later.

    `reject`

    - It changes the status of promise from ***pending*** to 'failed' (***rejected***).
    - When the asynchronous event succeeds, we can pass the result of the asynchronous into `reject`.
- Every promise instance has `.then` method.

### then()

`p.then(onFulfilled[, onRejected]);`

- `then()` returns a Promise - this makes the Promise chain possible.
    - The promise returned by `then()` does not have `resolve` method (not returned by the *callback* ). In this case, when the codes in `then` is finished and returns `undefined`, this promise is in *resolved* status.
    - **If the call-back function in `then()` returns a `new Promise`**, it will wait until this promise being resolved. Then, the promise that the `then` originally returns is finally resolved ( An ***resolve*** task which is used to resolve this promise will be pushed into ***microtask queue*** ).
- `then()` can take two call-back functions as its arguments for fulfilled or rejected respectively.
    - `onFulfilled` is required.
    - `onRejected` is optional, and only works when the Promise is rejected. It will **throw an error** if there is no this function in the argument.

```jsx
// promise return chain
let aPromise = new Promise((resolve) => {
  resolve("abc");
});

// If then() returns a value
aPromise
  .then((value) => {
    // do something
    return "1";
  })
  .then((value) => console.log(value)); // prints: 1

// If then() doesn't have a return statement
aPromise
  .then((value) => {
    // do something
  })
  .then((value) => console.log(value)); // prints: undefined

// If then() returns a pending Promise(new Promise)
aPromise
  .then((value) => {
    // do something
    return new Promise((resolve, reject) => {
      resolve("I am new promise");
    });
  })
  .then((value) => console.log(value)); // prints: I am new promise
```

### catch()

- If `catch()` is not used, Promise will stop running the codes when there is an internal error, but the error won't be thrown to the outside of the promise unless we use `catch()` to catch the error.
- Handling error can be also done by passing the second arugment into `then()`.
    - It won't stop the promise chain.
    - We can do some compatible settings
    - It can only handle the error that was thrown before `.then`, it won't be able to handle the error inside `onFulfilled`.

### In-depth Understanding of Promise

- Before we further understand Promise, we should keep in mind that in JavaScript, executing codes is always synchronous. The asynchronous is performed by the browser.
- The asynchronous is done by `.then`and `.catch` method.
- In terms of instantiating a Promise instance, the *executer* function is **synchronous** and it is executed **as soon as** the promise instance is created.
- For the browser, it has multiple queues for handling asynchronous events. Promise will be kept in  ***Microtask queue.*** Other asynchronous events will be holden in others queues.
- The browser has an ***event loop*** which will always check whether the execution context is empty or not. If it is empty, the event loop will place the asynchronous event into the execution context.
- The task in **microtask queue will be executed before the end of each event loop. The tasks in other queues will be executed after the start of each new event loop.**
    - **SO THIS CAUSE PROMISE WILL BE EXECUTED IN A HIGHER PRIORITY.**
- **When JavaScript is going to execute `then()` method, if the promise is in resolved status, then the call-back function in `then()` will be placed into the microtask queue. Then, it will jump into the next `then()` if available.**
- **When a promise is resolved, all call-back functions registerred from this promise's by `then()`s will be iterated and will be placed into the microtask queue one by one.**

    How to understand this: 

    ```jsx
    let aPromise = new Promise((resolve, reject) => {
    	setTimeout(resolve, 3000);
    });
    aPromise.then(() => console.log('the first then');
    aPromise.then(() => console.log('the second then');
    aPromise.then(() => console.log('the third then');
    ```

**Some good examples for understanding**

- The below example shows inside promise executer, it executes in synchronous way. When we have `setTimeout` and `.then`, JavaScript will do the task in microtask queue first, then do the normal asynchronous event.

    ```jsx
    let a = new Promise((resolve, reject) => {
      console.log(1);
      setTimeout(() => console.log(2), 0);
      console.log(3);
      console.log(4);
      resolve(true);
    });
    a.then((v) => {
      console.log(8);
    });

    // prints: 1 3 4 8 2
    ```

- Another example for a deeper understanding:

    ```jsx
    new Promise((resolve, reject) => {
      console.log("log: Outer promise");
      resolve();
    })
      .then(() => {
        console.log("log: First outer then");
        new Promise((resolve, reject) => {
          console.log("log: Inner promise");
          resolve();
        })
          .then(() => {
            console.log("log: First inner then");
          })
          .then(() => {
            console.log("log: Second inner then");
          });
      })
      .then(() => {
        console.log("log: Second outer then");
      });

    // log: Outer promise
    // log: First outer then
    // log: Inner promise
    // log: First inner then
    // log: Second outer then
    // log: Second inner then
    ```

    The executing order:

    1. **Pink**: In this case, the codes inside the callback of **the new promise** will be executed first (`log: Outer promise`), then this promise is resolved.
    2. **Green: The first outer `then`** checks the promise in front of it is in resolved, so the callback inside of `then` is pushed into miscrotask queue.
    3. **Yellow: The second outer `then`** runs and checks the first outer then is not resolved, so the callback of the second outer `then` is registered and stored in the second outer `then`.
        - **Yellow:** When the **green** then is resolved, yellow codes will be pushed into the miscrotask queue.
    4. **Green:** The callback of **the first outer `then`** is moved from miscrotask queue into the main thread. (prints: `log: First outer then`).
    5. **Green**: **The inner promise** is instantiated and prints `log: Inner promise`.
    6. **Purple**: The **first inner `then`** registers and **pushes its callback in miscrotask queue** because the inner promise is resolved.
    7. **Red** : Because the purple then is pending, so the **second inner `then`** **registered** its callback, but keeps it in `then`'s promise.
    8. **Green**: Green promise is finished and returns `undefined`. The green promise is resolved. Therefore, the **yellow** then is **pushed into the microtask queue**.
    9. **Purple**: The codes in purple runs. It returns undefined and resolved as well.
    10. **Red**: Because the purple promise is resolved, the codes in red is **pushed into the microtask queue**. Right after the yellow codes.
    11. **Yellow**: Yellow codes runs and returns.
    12. **Red**: Red codes runs and returns.

    ### Promise.all

    `Promise.all(iterable);`

    `iterable` - An iterable object such as an Array.

    `return` 

    - An **already resolved `Promise`**if the  passed is empty.
    - An **asynchronously resolved `Promise`** if the  passed contains no promises. Note, Google Chrome 58 returns an **already resolved** promise in this case.
    - A **pending** `Promise` in all other cases.
    - Successful **only when all** input promises are **resolved**. Rejected **as soon as any** of the input promise **rejected**
        - ***Resolved*** - Return **an Array** contains the result of all promises.
        - ***Rejected*** - Return the **first** rejected promise and **its rejected message or error**.

    `Promise.all` is very useful when we handle multiple asynchronous events. For example, loading a component, but wait until all AJAX requests have returned their values.

    `Promise.all` will wait until all input promises resolved, then its return array will **based on the order of input promises** to place the result into the result array. This feature can be useful when we need to handle using values based on the order of requesting.

    ```jsx
    let wake = (time) => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(`After ${time / 1000}s wake up`);
        }, time);
      });
    };
    let p1 = wake(3000);
    let p2 = wake(2000);

    Promise.all([p1, p2])
      .then((result) => {
        console.log(result);
      })
      .catch((err) => console.log(err));

    // result: ["After 3s wake up", "After 2s wake up"]
    ```

    ### Promise.race

    Return the result of any of the input promises in the iterable object based on which one is faster, **no matter** the result is ***resolved*** or ***rejected***.