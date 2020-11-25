# redux-thunk

---

# Firing Multiple Actions - redux-thunk & redux-saga

![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/1.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/1.png)

# redux-thunk - Asynchronous Event/Action Calls

A react library : allows us to handle **asynchronous** event handling and firing multiple actions.

## Install redux-thunk

`npm install react-thunk --save`

## Using redux-thunk

**redux-thunk** is a piece of middleware that allows us to fire functions.

```jsx
import thunk from 'redux-thunk';

const middlewares = [thunk]; // Adding into middlewares.
```

- If `redux-thunk` middleware is enabled, any time you attempt to `dispatch` a function instead of an object, the middleware will call that function with `dispatch` method itself as the first argument.
    - If `dispatch()` takes a **function** with `dispatch` as the first argument, `react-thunk` in the middleware will take effect to handle the action of calling this returned function.

```jsx
const mapDispatchToProps = dispatch => ({
  fetchCollectionsStartAsync: () => dispatch(fetchCollectionsStartAsync())
});

// currying function
const fetchCollectionsStartAsync = () => {
  return dispatch => {
    const collectionRef = firestore.collection('collections');
    dispatch(fetchCollectionsStart());

    collectionRef
      .get()
      .then(snapshot => {
        const collectionsMap = convertCollectionsSnapshotToMap(snapshot);
        dispatch(fetchCollectionsSuccess(collectionsMap));
      })
      .catch(error => dispatch(fetchCollectionsFailed(error.message)));
  };
};
```