# Redux

---

# Redux

Because when the app is growing and growing, it will be very hard to manage the data flows, so Redux is to help us understand how data flows.

- Good for managing large state
- Useful for sharing data between components
- Predictable state management using the 3 principles.
    1. Single source of truth
        - one big object that describes how the whole project looks.
    2. State is read only
    3. Changes using pure functions

This is a common pattern to keep **only important state** in Redux Store while keeping UI specific state like form inputs in `this.state`.

## Redux Flow

Action → (Middleware) →  Root Reducer (pure functions) → (outputs) → Store → (React notices the state changed) → DOM updates

- Many actions go into one reducer

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2014.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2014.png)

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2015.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2015.png)

## Redux Reducer

- `return` creates a new object in order to trigger React to update the DOM when a new `props` is passed in.
- `currentUser: action.payload`
    - the reducer takes the same key-value data from the `currentState` (spreading), and overwrites the field that we need to update (i.e. `currentUser` field in the `currentState` object) with `action.payload` data. After that, the new object becomes the newest `state`.

```jsx
const userReducer = (currentState, action) => {
	switch (action.type) {
		case 'SET_CURRENT_USER':
			return {
				...currentState,
				currentUser: action.payload // overwrites the currentUser field in currentState
			};
		default: 
			return currentState;
	}
};
```

## Install Redux

- `npm install redux redux-logger react-redux`

## Setup Redux

### Using Provider

We will use the `Provider` component which is from **react-redux** in the index.js file

Import the `Provider`, and we need to wrap all of other components.

- Wrapping other components is to allow everything inside this component to have access to this store object that we get from redux.

```jsx
... // other imports
import { Provider } from 'react-redux';

ReactDOM.render(
	**<Provider store={store}>**
		<BrowserRouter>
			<App />
		</BrowserRouter>
	**</Provider>**,
	document.getElementById('root');
);

```

### The Basic Files Structure with Redux

- Create a folder 'redux' under src folder.
- Create RootReducer
    - RootReducer.js
- Create User Reducer
    - Create UserReducer.js under a new folder in the src folder. (/src/user)
- Create the Store
    - import Store into index.js to allow `Provider` to have the context of store.
- Create Actions
    - These actions return objects.

# Reducer in Redux

- An reducer is actually just a function that gets two properties as its parameters - state and action
    - State will be the current state when the action is fired.
    - when the component is first run or something, there will not be a state there, so we need to set an initial state object

## state

- A state object which represents the last state or an initial state.
    - An action is just an object that has a **type**
    - **type**:
        - which is a string value. It tells what specific action this is. It helps redux to update the store state when it is needed.
    - **payload**:
        - the value that we want to update in our state.

```jsx
const INITIAL_STATE = {
  currentUser: null
};

const userReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
    case 'SET_CURRENT_USER':
      return {
        // set others state value which we don't care for this action
        ...state,
        // update the value that we care about
        currentUser: action.payload
      };
    default:
      return state;
  }
};

export default userReducer;
```

## Root Reducer

In root reducer, it is a base reducer object which contains all of the state of our application. In order to combine all reducers that we will pull into, we need to import `combineReducers` to do this task

- `import { combineReducers } from 'redux;'`

For each reducer, it will return an JSON object into the root reducer, and the root reducer will return a giant object which will contains all of them together.

```jsx
import { combineReducers } from 'redux';

import userReducer from './user/UserReducer';

export default combineReducers({
  // each reducer will return an JSON object
  user: userReducer
});
```

## Middleware

Middleware is like some functions which receive our actions and these functions will perform some tasks with the actions, then pass them to our rootReducer.

Add middleware to our store, so whenever actions get fired or dispatched, we can catch them and display them on the console (using `logger`).

In order to bring these middleware printing works in, we need to use `logger` from `redux-logger`.

Middleware is expected as an array by the store 

## Store

"A store **holds the whole state tree** of the application. The only way to change the state inside it is to dispatch an action on it. A store is **not a class.** It's just an **object** with a few **methods** on it. To create it, pass your root reducing function to `createStore`."

### **createStore**

Note: We can also apply middleware to our store when we create our store.

```jsx
File：store.js
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';

import rootReducer from './RootReducer';

// add middleware to store
const middlewares = [logger];

// Spread all of the methods or all of the values in the array
const store = createStore(rootReducer, applyMiddleware(...middlewares));

export default store;
```

- Passing the store into index.js to allow redux.provider to use the context of store in the rest of the application. In order to passing in, we will pass the store as the props into Provider.

    ```jsx
    import store from './redux/store';

    <Provider store={store}>
    	...
    </Provider>
    ```

- Now, we will have the access for the context of store in the components within the Provider.

### Create Actions

Each action returns an object which matches the format that we want it to be in the reducer.

```jsx
File: UserAction.js
export const setCurrentUser = user => ({
  type: 'SET_CURRENT_USER', // won't change it, so uppercase
  payload: user
});
```

# Use Redux in a Component - `connect()`

After the settings above, we can now use `connect()` to get our needed `state` from redux store. 

`connect(mapStateToProps, mapDispatchToProps, mergeProps, options)`

- **NOTE: if we don't need to use any of the function as the parameter, simply pass `null` to `connect()`.**

"The `connect()` function connects a React component to a Redux store. It provides its connected component with the pieces of the data it needs from the store, and the functions it can use to dispatch actions to the store."

`connect` lets us modify the components which need to access to things related to redux.

- **Connect** is a higher-order component, it will return a new modified component.

`connect()` accepts two functions and one component, the first one is the function (`mapStateToProps`) which can access the states **(the state is our root reducer)** as its parameter. The second argument is the component that we want to modify, which will take the returning object from `mapStateToProps` 

## mapStateToProps - 数据绑定

读取store state中的数据

`mapStateToProps?: (state, ownProps?) => Object`

- If `mapStateToProps` takes only one parameter, it will be **called** whenever the **store state changes**, and given the **store state** as the only parameter to **the wrapped component**.
- If `mapStateToProps` takes two parameters (`state, ownProps`), it will be **called** whenever the **store state changes** or when the wrapper component **receives new props** (based on shallow equality comparisons). It will be given **the store state as the first parameter, and the wrapper component's props as the second parameter.**
    - if your component needs the data from its own props to retrieve data from the store. This argument will contain all of the props given to the wrapper component that was generated by connect.

 "The new wrapper component will subscribe to Redux store updates. Any time the store is updated, `mapStateToProps` will be called. The **returning object** of `mapStateToProps` will be merged into the wrapped component's **props**."                                                  

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2016.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2016.png)

- `state` is the value of the store state, and  it will be changed whenever the store state changes. And the function will be called with the new `state`.
- `{currentUser: state.user.currentUser}` will be the `props` of `Header` component.
    - `state` points to the `store` state
    - `state.user` points to `user` property of `store` state which is from `rootReducer`.
        - Because store is created by `createStore()` function and it binds `rootReducer` which exports an object of different reducers.
    - `state.user.currentUser` points to `currentUser` property of `userReducer`
- So, we don't need to pass the `props` to `Header` component inside the `App` component. Because it is already passed into by using higher-order component.

    ***before:*** `<Header currentUser={this.state.currentUser} />`

    ***now:*** `<Header />`

- The connected component may re-render depends on the the returned object of `mapStateToProps()`.

```jsx
File: Header.js

import { connect } from 'react-redux'; 

...

/**
 * A standard function in redux, which deals with returning the redux store's state
 * @param {Object} state the value will come from the reducer. state is the combinedReducers object
 * @return {object} returns will be passed into this component
 */
const mapStateToProps = state => ({
  // combinedReducers -> userReducer (the value of the user key) -> currentUser (userReducer returns)
  currentUser: state.user.currentUser
});

export default connect(mapStateToProps)(Header);
```

## mapDispatchToProps - 事件绑定

`mapDispatchToProps?: Object | (dispatch, ownProps?) => Object`

调用action作为参数传入store中的reducer，存储数据到store state

`mapDispatchToProps` is used for dispatching actions to the store. `dispatch` is a function of the Redux store. You can call `store.dispatch` to dispatch an action. **This is the only way to trigger a state change**

In order to update the reducer value, we will use the action that we set in the reducer.

The `mapDispatchToProps` function will be called with `dispatch` as the first argument.

`mapDispatchToPropss` will normally be **a function** which will take `dispatch` as argument and **return** **an object** which contains couples of functions as its fields.  `dispatch()` will be called inside these new function. If we already defined the actions in the `rootReducer`, 

- `dispatch()` will take a **action object** as argument, which includes a `type` field and may have `payload` field.
- `dispatch()` will dispatch an action through executing **all reducers** to see which reducer has this action. **So make sure the name of action type is unique.**

`mapDispatchToProps` 返回的object会以props的形式传入到正在连接的组件，我们可以通过直接调用这些dispatch，从而调用其对应的action。dispatch帮助我们将数据store和操作事件action绑定到需要使用的组件上

- the properties in the `mapDispatchToProps` or `mapStateToProps` will be the props which are passed into the component.
- **不要忘记作为props传进组件的action是需要onClick 或是 onChange去trigger的！！！**

```jsx
import { setCurrentUser } from './redux/user/UserAction';

// Dispatch accepts action objects and pass it to every reducer
const mapDispatchToProps = dispatch => ({
  // setCurrentUser(user) returns an Action object, each field is a function
  setCurrentUser: user => dispatch(setCurrentUser(user))
});

export default connect(null, mapDispatchToProps)(App);
```

此时我们可以这样在App组件中使用`setCurrentUser`，因为`mapDispatchToProps`返回的是一个新的`object`，这个`object`被作为`props`传入了 App 中，`setCurrentUser`是其中的一个property。当 `setCurrentUser` 被调用时， 指向的是`UserAction.js`中的function，即先是调用的绑定的action，这个action会生成一个action object，再通过`dispatch()` 在`rootReducer`中找到 action object 中的 action type 所对应的 reducer，再执行 reducer 中的内容，从而实现更新 store 中的数据。

- 其中若有组件对对应的数据有设置好connect的话(mapStateToProps)，react会对其进行局部刷新DOM

```jsx
this,props.setCurrentUser(newUser);
```

[redux 终于搞明白store reducer action之间的关系](https://www.jianshu.com/p/21960f78937d)

**如果我们没有将mapDispatchToProps 作为param传进connect(), 那么，connect会自己将dispatch作为props传入connect的组件中。**

# Reselector - memorising state

Problem: 

- redux provides `state` which is the whole `store` object to the components connected with redux. So when the redux’s `state` updated, it will trigger these components rerender.
- Everytime that the part of store is updated, even though the component is not using that updated data, it is still re-rendered. It affects the performance of the application. So we are using reselector to increase the performance.
- For example, in the e-commerce app, when the current login user is changed, the cart component will be re-rendered but none of the cart items is changed.

**Selector will return piece of the state, as we always use the smaller and more specific selectors.**

## InputSelector

- Doesn't need to have `createSelector`

### **How to use it**

`const selectCart = state => state.cart;`

- It selects the partial of the state.

## OutputSelector

- Using InputSelector and createSelector

### **How to use it**

- It is taking two arguments
    1. The collection of the input selector
    2. The function with the argument(the return of input selector)

```jsx
**File**: CartIcon.jsx
...

// It passes the whole reducer into the selector: selectCartItemsCount 
const mapStateToProps = state => ({
  itemCount: selectCartItemsCount(state)
});

/*
    selectCartItemsCount will first check its inputSelector collection: selectCartItems
    selectCartItems will check its inputSelector collection: selectCart
    selectCart is taking the state argument that we passed in from selectCartItemsCount
    selectCart returns state.cart which will be taken by the selectCartItems as the 
    second argument's argument (cart).
    then, selectCartItems returns cart.cartItems to selectCartItemsCount's second argument
    cart.cartItems is cartItems
    reduce() will take cartItems and return the result to the value of property itemCount.
*/
**File**: CartSelector.js
import { createSelector } from 'reselect';

// input selector
const selectCart = state => state.cart;

// output selector
export const selectCartItems = createSelector(
  [selectCart],
  cart => cart.cartItems
);

export const selectCartItemsCount = createSelector(
  [selectCartItems],
  cartItems =>
    cartItems.reduce(
      (accumalatedQuantity, cartItem) =>
        accumalatedQuantity + cartItem.quantity,
      0
    )
);
```

**When** **some variables needs to be passed into the selector, a currying function is introduced:**

```jsx
...

const mapStateToProps = (state, ownProps) => ({
  collection: selectCollection(ownProps.match.params.collectionId)(state)
});

...

// selectCollection is a function which returns another function.
// The returned function will take the state to perform the transmission with other selectors.
// collectionUrlParam will be the value passed into from the url
export const selectCollection = collectionUrlParam =>
  createSelector([selectCollections], collections =>
    collections.find(
      collection => collection.id === COLLECTION_ID_MAP[collectionUrlParam]
    )
  );
```

[Copy of Memoising A Whole Function](Redux%204d8adaeaa8ee47a8baf22aa9361676ab/Copy%20of%20Memoising%20A%20Whole%20Function%203cfc461ccd5c434195e327e5c900e9b5.md)

## createStructuredSelector() - avoiding passing `state` manually to selectors

We are using this to avoid duplicated code about passing `state` into our selector for different properties.

- and we just simply map the selector to where we want to be used.

```jsx
import { createStructuredSelector } from 'reselect';

...

// with using createStructuredSelector
const mapStateToProps = createStructuredSelector({
  currentUser: selectCurrentUser,
  hidden: selectCartHidden
})

// without using createStructuredSelector
const mapStateToProps = state => ({
	currentUser: selectCurrentUser(state),
	hidden: selectCartHidden(state)
});
```

# Redux Persist - local storage in the browser

## Install redux-persist

`npm install redux-persist --save`

## Configurations

import `redux-persist` in the **store.js** file

- `import { persistStore } from 'redux-persist';`

### persistStore - step 1

It allows the browser actually caches our store now, depending some configurations.

- So we need to make a little configurations on `store.js`

    ```jsx
    const store = createStore(rootReducer, applyMiddleware(...middlewares));

    // Persist versiong of the store
    const persistor = persistStore(store);

    export default { store, persistor };
    ```

### persistReducer - step 2

It is used to modified our root reducer in order to use `storage`.

- we also need to make a little configurations on `RootReducer.js`
    - Tell the `redux-persist` the `storage` that the application will be used.
        - you can point to `localStorage` or `sessionStorage` depending on the needs.con
    - Define the new `persist config`
        - `key` - defining at what point inside of the reducer object that we want to start storing everything.
        - `storage` - pointing to the storage object from redux
        - `whitelist` - an array containing the string names of any of the reducers that we want to store.
            - the field that we used in `combineReducers()`

    ```jsx
    ...
    import { persistReducer } from 'redux-persist';

    // it tells redux-persist that the default storage is the localStorage from the window object.
    import storage from 'redux-persist/lib/storage';

    // defining the new persist config
    const persistConfig = {
    	key: 'root', // storing from the root.
    	storage, // pointing the storage object that we imported from redux
    	whitelist: ['cart']
    };

    // modified
    const rootReducer = combineReducers({
    	user: userReducer,
    	cart: catReducer
    });

    // export the modified version of the rootReducer
    export default persistReducer(persistConfig, rootReducer);
    ```

### Bringing the Actual Persistor into the Application - step 3

In `index.js`, we need to bring in the persistor.

- `import { PersistGate } from 'redux-persist/integration/react';`

Brining `store` and `persistor`

- `import { store, persistor } from './redux/store';`

Wrapped our `App` component inside `PersistGate`

```jsx
...
		<PersistGate persistor={persistor}>
        <App />
     </PersistGate>
...
```

## Overview of the Persistence

- When the browser is **reloaded** or **refreshed**, redux will check if **there is an existing value** in the `persist`, then it will fired the `rehydrate` those value back to the `store`.

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2017.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2017.png)

# Redux Compose - Compose Multiple Higher-order Components

If we have a component which needs to be wrapped inside many other components, it will be very hard to read and maintenance.

- We can compose all our higher-order components together as parameters into a function.

## Import compose from 'redux'

`import { compose } from 'redux';`

**The composed order will be from the right-to-left: WithSpinner → connect(mapStateToProps)**

- It will pass CollectionOverview into `WithSpinner()` first, then pass the wrapped component into `connect(mapStateToProps)()`

    ```jsx
    **before:**
    const CollectionsOverviewContainer = connect(mapStateToProps)(WithSpinner(CollectionOverview));
    ```

    ```jsx
    **after:**
    import { compose } from 'redux';
    const CollectionsOverviewContainer = compose(connect(mapStateToProps), WithSpinner)(CollectionOverview);
    ```