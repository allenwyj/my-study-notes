# redux-saga

# Redux-Saga

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2018.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2018.png)

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2019.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2019.png)

## Introduction to Saga

`redux-thunk` will allows actions to come in as functions, and if it is a function, the thunk will invoke the function because we have dispatch in the function. Thunk will just pass those new dispatch actions into the reducers one by one. 

We will use `redux-saga` to perform asynchronous events requests like `redux-thunk`. Saga is like a function that **conditionally** runs and the condition that it depends on when it runs - is based on whether or not **a specific action is coming into** the saga middleware.

### **Install Saga**

`npm install redux-saga --save` or `yarn add redux-saga`

### **Using Saga**

Normally, we will place the Saga in the same redux folders that are going to handle things related to. i.e. shop saga in the shop redux folder.

Saga expects generator function inside them.

- import saga into our redux (in store.js)

    `import createSagaMiddleware from 'redux-saga';`

- setup saga middleware (in store.js)

    `const sagaMiddleware = createSagaMiddleware();`

    - This can take an object with certain configuration settings on it if needed.
- pass the `sagaMiddleware` into the `middlewares`

    `const middlewares = [sagaMiddleware];`

- run our `sagaMiddleware`

    `sagaMiddleware.run();`

    - Inside the `run()`, we will pass each individual saga.

        i.e. `sagaMiddleware.run(fetchCollectionsStart);`

    - **In order to be able to run multiple sagas, we will create a `rootSaga` to run all sagas at once.**

        The array inside `all` will take all different individual `sagas` together and run them **concurrently**.

        ```jsx
        File: UserSagas.js
        ...

        // Compose all user sagas together and put them into root sagas.
        export function* userSagas() {
          yield all([call(onGoogleSignInStart), call(onEmailSignInStart)]);
        }

        File: RootSaga.js
        import { all, call } from 'redux-saga/effects';

        import { userSagas } from './user/UserSagas';

        import { fetchCollectionsStart } from './shop/ShopSagas';

        export default function* rootSaga() {
          yield all([call(fetchCollectionsStart), call(userSagas)]);
        }

        File: Store.js
        ...
        import rootSaga from './RootSaga';

        sagaMiddleware.run(rootSaga);
        ```

- create the saga file for the related redux folder. i.e. **ShopSaga.js**
    - Saga needs to be written in **generator function**
    - If we want to **catch errors**, we will need to use `try...catch` block.
    - Sagas **do not dispatch actions** using the `dispatch` keyword - **Sagas use `put` effect.**

# Saga Effects

**import certain effects from saga to do different things with either the store like creating actions or listening for actions.**

`**take, takeEvery, call, put, takeLatest, all**`

## take

- `take` - wait to finish the rest of code until the application listened a certain type.
    - Creates an Effect description that instructs the middleware to **wait for a specified action on the Store**. The Generator is **suspended** until an action that matches `pattern`(action type) is dispatched.
    - The result of `yield take(pattern)` is an action object being dispatched.

## takeEvery

- `takeEvery(pattern, fn)` - it will **listen** for every action of a **specific** **type** that we pass to it, and instructs the middleware to dispatch an action to the Store, like `take`.

    `**fn` will have the whole action object which is dispatched as the argument.**

    [redux-saga/redux-saga](https://github.com/redux-saga/redux-saga/tree/master/docs/api#takeeverypattern-saga-args)

    - `takeEvery` creates a **non-blocking call** in order to **not stop our application** to continue running either other sagas or whatevert the user wants to do. It **doesn't pause** the javascript waiting for anything inside of our `actualAction` to come back.
    - We can use these `yield`s in the `sagaMiddleware` to control or cancel any previous started sagas from the other actions that came in. - **That's why we need generator functions for our saga.**
    - **`takeEvery(pattern, saga, ...args)`**
        - Spawns a `saga` on each action dispatched to the Store that matches `pattern`.
            - `pattern:` The first parameter will be the **action type** that we are expecting.
            - `saga: Function` - a Generator function
            - `args: Array<any>` - arguments to be passed to the started task. `**takeEvery` will add the incoming action object to the argument list (i.e. the action will be the last argument provided to `saga`)**

**The difference between ``take`` and ``takeEvery``**

Generator function cannot get any value if we don't return anything. So when the completed property says true and then we would never be able to restart our saga - we cannot go back into the saga and then start from top to bottom again. 

Using ``take`` inside of a generator function, if we want to re-use the generator again and again, we will need to loop ``take``. So, the generator will be looping and get ready to take new increments every time when an increment action comes in (`**yield**` will be resumed). **BUT unlike ``takeEvery``, it may block the application if we have a asynchronous actions inside the `**while**` loop.**

`function* incrementSaga() {
    while (true) {
	yield take('INCREMENT');
	console.log('I am incremented');
    }
}`

``takeEvery`` kicks off a new task using the generator that we passed to it as its second ``arg``. It will spawn a **new** ``saga`` on each action dispatched to the Store that matches ``pattern``. **It won't block the application when we are doing some asynchronous tasks due to non-blocking .**

## takeLatest

- `takeLatest(pattern, fn)` - it will cancel any previous running `saga` task and only run the current one.
    - Spawns a `saga` on each action dispatched to the Store that matches `pattern`. And **automatically cancels any previous `saga` task** started previously if it's still running.
    - Each time an action is dispatched to the store. And if this action matches `pattern`, `takeLatest` starts a new `saga` task in the background. If a `saga` task was started previously (on the last action dispatched before the actual action), and if this task is still running, the task will be cancelled.
    - `takeLatest` will add the incoming action to the argument list (i.e. the action will be the last argument provided to saga)
        - `**fn` will have the whole action object which is dispatched as the argument.**

            ```jsx
            import { takeLatest } from `redux-saga/effects`

            function* fetchUser(**action**) {
              ...
            }

            function* watchLastFetchUser() {
              yield takeLatest('USER_REQUESTED', fetchUser)
            }
            ```

## call

- `call(fn, args)` - it is the effect inside of the generator function that invokes the method.
    - Creates an Effect description that instructs the middleware to call the function `fn` with `args` as arguments.

## put

- `put` - it is the effect inside of the generator function that **dispatches action**
    - Creates an Effect description that instructs the middleware to **dispatch an action** to the Store. This effect is non-blocking and any errors that are thrown downstream (e.g. in a reducer) will not bubble back into the saga.
    - It is like `dispatch`, but we have to `yield` it and is going to use it like a **dispatch**

        `yield put(fetchCollectionsSuccess(collectionsMap));`

## all

- `all` - **concurrently** running different sagas.
    - Creates an Effect description that instructs the middleware to run multiple Effects in parallel and wait for all of them to complete. It's quite the corresponding API to standard *Promise#all*.

```jsx
import { call, put, takeLatest } from 'redux-saga/effects';
import {
  firestore,
  convertCollectionsSnapshotToMap
} from '../../firebase/FirebaseUtils';

import { fetchCollectionsSuccess, fetchCollectionsFailed } from './ShopActions';

import ShopActionTypes from './ShopTypes';

export function* fetchCollectionsAsync() {
  try {
    const collectionRef = firestore.collection('collections');
    const snapshot = yield collectionRef.get();
    // snapshot will be the arg for convertCollectionsSnapshotToMap
    const collectionsMap = yield call(
      convertCollectionsSnapshotToMap,
      snapshot
    );
    yield put(fetchCollectionsSuccess(collectionsMap));
  } catch (error) {
    yield put(fetchCollectionsFailed(error.message));
  }
}

export function* fetchCollectionsStart() {
  // we don't want the API call triggered multiple times.
  yield takeLatest(
    ShopActionTypes.FETCH_COLLECTIONS_START,
    fetchCollectionsAsync
  );
}
```

# Generator Function

It is similar to async await functions, it **pauses their execution** whenever they see a specific key inside of the function and that that key is called a **yield.** 

**All generator functions must have at least one yield in each.**

## **Declare A Generator Function**

### Using yield

`yield` key tells our generator function that it is going to stop the function from here including the line of `yield`, until `next()` gets called.

### Not Using yield - old school way

- Old school way

    ```jsx
    function***** gen() {
    	console.log('a');
    	console.log('b');
    }

    const g = gen(); // unlike the normal function, it won't fire the function gen()

    // g now is a generator object
    // resume the execution
    g.next(); // print: a b ; returning {value: undefined, done: true}

    function***** gen1(i) {
    	**yield** i;
    	**yield** i + 10;
    	return 25; // if we wanna catch the value when it's done.
    }

    const g = gen1(5); // undefined
    g.next(); // {value: 5, done: false}
    const jObj = g.next(); // undefined jObj: {value: 15, done: false}
    g.next(); // {value: 25, done: true}
    ```