# React-Hooks

---

# React-Hooks

## Introduction to react-hooks

`react-hooks` is introduced due to the diffficulity of managing those internal state and determining the order in which those lifecylce methods are going to fire and where we should be doing different functionality of our component in what lifecycle method.

**Hooks** allow us to write functional components but still gain access to new functionality that was previously only available in the class components. 

- So we can only use `react-hooks` inside of the functional component.

`react-hooks` only works under react version 16.8 or higher.

# Using react-hooks

## Import react-hooks

**`useState`** - allow us to use State and set State

`import React, { useState } from 'react';`

## useState - internal state

- `useState` - it is similar to setState in the class component (it will make the component rerender). It gives back two array values:
    - the variable that we want useState() to hold;
    - a function that allows us to set that variable.

```jsx
const [name, setName] = useState('Allen') ;
const [varA, setVarA] = useState(someValue) ;
const [varB, setVarB] = useState(someValue) ;
... // can have many as we need

console.log(name); // print: Allen
setName('Hi, Yujie'); // name is 'Hi Yujie' now
...
```

---

## useEffect - performing lifecycles' jobs

`function useEffect(effect: EffectCallback, deps?: DependencyList): void;`

Accepts a function that contains imperative, possibly effectful code.

*@param* `effect` — Imperative function that can return a cleanup function

*@param* `deps` — If present, effect will only activate if the values in the list change.

### Hook Rule

**HOOK RULE: If we want to write a conditional hooks, we should put our condition inside the hooks instead of wrapping the hooks in the condition.** 

- `**useEffect**` - allow us to perfom a function when the component is render/update/re-render. It has two parameters:
    - the first parameter: the function that we want to execute
    - the second parameter: an **array** of properties that we want our function get fired when those properties are changed (cause the rerender). **If present, effect will only activate if the values in the list change.**
        - In some case, if we don't specify an **empty** array, it will cause the `useEffect` keeps firing again and again (if we set state in the effect function, it will cause this component re-render, and we didn't say we are not watching anything, then it will monitor all state changes, and fired the effect function when these states changed)
    - `useEffect` will be fired when the component is rendered at the first time.

    ```jsx
    // Building the componentDidMount() method:
    const UseEffectExample = () => {
    	const [user, setUser] = useState(null);
    	const [searchQuery, setSearchQuery] = useState('Allen');

    	useEffect(() => {
    		**// Writting our condition to trigger this hook if needed
    		// if (...) {}**
    		const fetchFunc = async () => {
    			const response = await fetch(`Some API Call/users?username=${searchQuery}`);
    			const resJson = await response.json();
    			setUser(resJson[0]); //in the example, the returning result always in an array
    		}

    		fetchFunc();
    	}, []);

    	...
    }
    ```

---

`cleanUp` function - it performs the similar job like `componentWillUnmount` lifecycle method

We will return a function inside the `useEffect` to perform the cleanUp function. And this function will be called when the component is unmounted.

```jsx
useEffect(() => {
	console.log('I am subscribing');
	const unsubscribeFromCollections = firestore.collection('collections').onSnapshot(snapshot => console.log(snapshot));
	
	**// cleanUp function
	return () => {
		console.log('I am unsubscribing');
		unsubscribeFromCollections();
	};**
}, []);
```

- react 里的 ajax 应该这么写

    ```jsx
    useEffect(()=>{
     发请求
     return ()=> 取消请求
    },[userInput])
    ```

---

## useReducer - complex state management

We will use this `useReducer` when we need more complex local state management than `useState`.

Using `useReducer` is very similar to what we do in redux.

### **Returning from `useReducer`**

- state - the current state
- dispatch

### **Passing into `useReducer` to instantiate**

- reducer - gets both a state in an action and using a `switch` statement returns us back some object.
- INITIAL_STATE - the initial value of the state

```jsx
import { useReducer } from 'react';

const INITIAL_STATE = {
	user: null,
	searchQuery: 'Allen'
};

const reducer = (state, action) => {
	switch (action.type) {
		case 'SET_USER':
			return { ...state, action.payload}
		case 'SET_SEARCH_QUERY':
			return {...state, action.payload}
		default:
			return state;
	}
};

const setUser = user => ({
	type: 'SET_USER',
	payload: user
});

const setQuery = query => ({
	type: 'SET_SEARCH_QUERY',
	payload: query
});

const userReducerExampleComponent = () => {
	const [state, dispatch] = useReducer(reducer, INITIAL_STATE);
	// destructuring the state
	const { user, searchQuery } = state;
	
	...

	return (
		...
		dispatch(setUser(resJson[0]));
		dispatch(setQuery(resJson[0]));
		...
	)
}
```

---

# React Hooks Cheat Sheet

```jsx
ComponentDidMount
//Class
componentDidMount() {
    console.log('I just mounted!');
}
 
//Hooks
useEffect(() => {
    console.log('I just mounted!');
}, [])

ComponentWillUnmount
//Class
componentWillUnmount() {
    console.log('I am unmounting');
}
 
//Hooks
useEffect(() => {
    return () => console.log('I am unmounting');
}, [])

ComponentWillReceiveProps
//Class
componentWillReceiveProps(nextProps) {
    if (nextProps.count !== this.props.count) {
        console.log('count changed', nextProps.count);
    }
}
 
//Hooks
useEffect(() => {
    console.log('count changed', props.count);
}, [props.count])
```

---

# Custom Hooks

**We need to actually think about when should we need to pass the second parameter to our `useEffect` method.**

The reason why it doesn't have the second parameter to specify which dependency is for in this custom hook is because we want our application can always access the latest data from the API. So every time the `useFetch` get called (may component render or re-render), `useFetch` will be fired. If we specify an empty array for `useEffect`, then it will only be fired once when this `useEffect` is instantiated.

```jsx
import { useState, useEffect } from 'react';

const useFetch = (url) => 
	const [data, setData] = useState(null);

	useEffect(() => {
			const fetchFunc = async () => {
				const data = await fetch(url);
				const dataJson = await data.json();
				setData(resJson[0]); 
			}
	
			fetchFunc();
		});

	return data;
}

export default useFetch;
```