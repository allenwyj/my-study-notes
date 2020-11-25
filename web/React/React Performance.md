# React Performance

- **Only optimise the application in the production (NOT in the local dev environment) once there is a problem and measure it before starting code optimisation.**
- **Do NOT over-optimised**

---

# Speed Performance

[Progressive React](https://houssein.me/progressive-react)

### Hard Refreshing Web Page on Mac

`cmd + shift + r`

- Re-download every single asset on this page, instead of normal refreshing which will use cache.

    ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled.png)

- Normal Refresh

    ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%201.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%201.png)

# Dynamic Import JS Files

## Static Import

- create-reat-app does this import for us by default.
- The way that we normally import files is static import. It will pull all required files together.

## Dynamic Import

- It pulls in the import like a promise.

### React Lazy - Lazy Load

- `import React, { lazy } from 'react';`
- Wrap the components inside of the `lazy` function.

    ```jsx
    const HomePage = lazy(() => import('./pages/homepage/HomePage'));
    ```

- **Route level lazy loading (Page Level)**
    - `Route` works perfectly with `lazy` which allows us to split codes into chunks.
    - So when user navigates to a `Route`, and the code chunk for this routing component has been already chunked during the build step.
    - i.e. Hitting home page, web browser will get home page chunk. (**Home page is the main page and it is not a good place to set the lazy loading, here is just an example)**

    **PROBLEM**: Because using `lazy` is a synchronous, so when we request for our home page from the back-end server and it might take some time and it might cause an error. **So we need to introduce `Suspense`**

- **Using `Suspense` with lazy loading can solve the problem!**
    - `import React, { lazy, Suspense } from 'react';`
    - `Suspense` is actually meant to be used with react `lazy`.
    - `Suspense` can wrap all lazy loading components together and perform its task for the lazy loading individually.
    - `Suspense` must take a `fallback` property, `fallback` points to any HTML element or component itself.
        - `Suspense` will render the `fallback`element or component and **waits** until the actual component is finished being lazy loaded (dynamically imported) and it will automatically switch to the right component.

    ```jsx
    <Switch>
      <Suspense fallback={<div>Loading Page... </div>}>
        <Route exact path="/" component={HomePage} />
        <Route path="/shop" component={ShopPage} />
        <Route exact path="/checkout" component={CheckoutPage} />
        <Route
          exact
          path="/sign-in"
          render={() =>
            currentUser ? <Redirect to="/" /> : <SignInAndSignUpPage />
          }
        />
      </Suspense>
    </Switch>
    ```

- Setting lazy loading for component Level (not page level) is not that beneficial and recommended.
    - Only add lazy loading into components if we have tested it and it is actually slowing down the application in production.

# Error Boundary

After deployment, there might be a chance that the app won't be able to get anything back and the spinner is going to hang indefinitely. In fact if an error comes back, `Suspense` is not going to know what to do to handle it and the **App will break**.The easiest solution is do **error boundary**.

- **Error boundary** is a way to write a component which will **catch an error** and then render some fallback components or UIs just like `Suspense`.
- Error boundary is a **class component** that has some **unique lifecycle** methods(only in error boundary component): `componentDidCatch` lifecycle method and `getDeriveStateFromError` lifecycle method.
- `**ErrorBoundary` component will wrap `Suspense` component.**
- An resource to show a professional 404 page.

    [404 Illustrations - Trendy 404 page images for your next project](https://www.kapwing.com/404-illustrations)

## getDerivedStateFromError

`getDerivedStateFromError(error)`

- This lifecycle method **catches** any `error` that **gets thrown** in any of the **children** of this `ErrorBoundary` component.

## componentDidCatch

`componentDidCatch(error, info)`

- This method allows to catch the error and get some info about the error like error in which component.

## Full Codes

```jsx
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor() {
    super();

    // normally only saving an error flag
    this.state = {
      hasErrored: false
    };
  }

  // a lifecycle method that gets the error if its children throws any error
  static getDerivedStateFromError(error) {
    // do something with the error

    // has to return an object that sets local state, 
		// and the component is aware of there is an error
    return { hasErrored: true };
  }

  // allowing to catch the error and get info about error in which component
  componentDidCatch(error, info) {
    console.log(error);
  }

  render() {
    // conditionally render component based on the state
    if (this.state.hasErrored) {
      return <div> Something went wrong </div>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

# React.Memo - Component Level Re-rendering

**PROBLEM: The parent component updates itself but causes its children components which take props from parent's re-render as well. These unnecessary renderings are costing the application.**

- A functional component will always re-render whenever its parent component renders - no matter the child component's props are still the same as pre-render.
- A **class** component can use `shouldComponentUpdate` to do a shallow comparison and to avoid this kind of unnecessary re-renders.
    - `React.PureComponent` is similar to `React.Component`.
    - The difference between them is that `React.Component` doesn’t implement `shouldComponentUpdate()`, but `React.PureComponent` implements it with a shallow `prop` and `state` comparison.
        - `React.Component` needs to be **manually** implemented `shouldComponentUpdate`.
        - `React.PureComponent` **already** implemented `shouldComponentUpdate`:

            `class ChildName extends React.PureComponent{...}`

- A **functional** component can use `React.memo` to do the similar thing as `shouldComponentUpdate` does in the class component.
    - `React.memo` is a HOC and needs to wrap the child component, which will memorise the component. **This means that React will skip rendering the component, and reuse the last rendered result.**
    - By default it will **only shallowly compare complex objects** in the `props` object. If you want control over the comparison, you can also provide a custom comparison function as the second argument.
    - The **difference** bewteen `reselect` and `React.memo`:
        - `React.memo` checks to make sure that the actual object (the `props` object) values **have not changed** at the **root level,** which is a shallow comparison.

    ```jsx
    import React from 'react';

    const Person({person}) => (....);

    export default React.memo(Person);
    ```

- `React.memo` will increase the mounting time a bit - **So** for those child components which don't take any `prop` from their parent component, no need to use this optimisation.
    - Because the child component won't re-render if it doesn't take any prop from its parent component.

[React Top-Level API - React](https://reactjs.org/docs/react-api.html#reactmemo)

## 理顺思路：组件刷新时，其state和props的内存指向

- `setState()` 是 shallow merge，即使我用原本的 `state` 中的值来 `setState`，他是合并两个 object，然后生成了一个新的 object 作为 re-render 后的 component state，所以 state object 是否是全新的并不能用是否有发生 re-render 来判断，而是要看在 component 内部是否有调用 `setState`，如果没有 `setState`，但 component 要 re-render，那么他会拿组件更新前的 `state` object 作为更新后组件的 `state` object，所以他们的 reference 是一样的。
- 这也是之前，redux 那个 reducer 的组件更新存在效率问题，为什么用户登陆会影响shoping cart 里面的 cart item 组件的刷新，是因为 redux 内部 state 管理也是一层层 shallow merge 实现的。从 user reducer 一直 shallow merge user state到 root reducer（在 reducer 中，我们是通过 spread state（...state），然后再把要更新的属性通过设置 payload 来更新），最后在 mapStateToProps 中，那个 state 变成了一个新的 state object，所以触发了所有用到 redux mapStateToProps 的不相关组件更新。
- 如果子组件是内部 state 更新导致 re-render，他会拿更新前组件的 this.props reference 给更新后的组件 this.props reference。如果是父组件更新导致子组件更新的话，那么子组件的 `this.props` 这个 object 更新前后的内存指向一定是不相同的，至于 `props` 里面的 property 的内存指向，就要取决于传递当时是如何赋值的。
    - 如果是指向某个object，那么 property 的指向不会受到 `this.props` 的内存指向影响，例如以下例子中的 `person={this.state.person}`.
    - 如果是通过直接实例化一个object，在传递给 `props` 的话，那每次父组件的刷新都必然导致重新实例化，内存指向也随即改变，那么子组件中 `this.props` 中的property 指向也会改变，`React.memo` 此时无法阻止子组件的刷新，即使property的实际值是相同的。例如以下例子中的 `person={{ name: 'Allen', age: 25 }}`.
        - `React.memo` doesn't perform on referential equality.
        - in-line functions 和 in-line arrays 同样也属于object，小心其对 `React.memo` 的影响。
    - 图：`props` 传递方式对 `React.memo` 的影响

        ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%202.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%202.png)

        ---

        ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%203.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%203.png)

    - 图：`this.props` 和 `this.state` 的变化：父组件刷新对子组件 和 子组件自己刷新

        ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%204.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%204.png)

        ---

        ![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%205.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%205.png)

# useCallback() - Memorising Functions

![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%206.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%206.png)

- When we declare a function inside a component, it is actually creating an object (function is object type).
- Every time when the component gets re-render, the function will also be instantiated and be an new object even though it doesn't get any change.
- **It doesn't need to be updated if there is no dependency or property coming into the function body and it won't be changed.**
- It is costing the application and `useCallback` hook can **memorise the function.**

## useCallback

`useCallback(function, [dependencies]);`

- `useCallback` wraps a function and memorises it so if **none of the dependencies** that this function depends on **changes**, **the cached memorised version** of the wrapped function will be given back.
- If we have any dependency inside the function, it has to be included in the dependency array.

```jsx
import React, { useState, useCallback } from 'react';

const App = () => {
	const [count1, setCount1] = useState(0);

	// Only when the count1 gets updating, 
	// incrementCount1 function will be re-instantiated.
	// If the count1 remains the same value,
	// then the function won't be re-instantiated though the component re-render.
	const incrementCount1 = useCallback(() => setCount1(count1 + 1), [count1]);
	const logName = useCallback(() => console.log('Allen'), []);

	return (....);
}
export default App;
```

# useMemo() - Memorising the result of a function

![React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%207.png](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9/Untitled%207.png)

- As we can see in the above screenshot, when we click the button 2, even though the count1 doesn't have any change, `doSomethingComplicated` function still gets called every time when the component re-render.
- In this example, the calculation is not that much computationally expensive. But **what if we have a very expensive for the browser to do and it will take lots of browser's performance power to run it.** In that situation, we won't expect the browser will re-calculate it for us **if there is no change on the output like the value**.

## useMemo

`useMemo(function, [dependency])`

- `useMemo` is in a similar manner as `useCallback`.
- `useMemo` takes a step beyond just memorising the function like `useCallback`. It will memorise the output of the function - **memorising the returns of the function.**
    - We don't want the memorised function run again if there is nothing change.
    - The function wrapped by `useCallback` still needs to be called in the application
- `useMemo` will get the `return` value of the wrapped function and then **cache** that value and use it instead of invoking the function.
    - It will **only invoke** the wrapped function if **its dependencies changed**.
- Now, `doSomethingComplicated` becomes a value instead of a function. Like what we have `selectors` in Redux with `reselct`.

```jsx
import React, { useMemo } from 'react';

export default function App() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleClick1 = () => {
    setCount1(count1 + 1);
  };

  const handleClick2 = () => {
    setCount2(count2 + 1);
  };

  const doSomethingComplicated = useMemo(() => {
    console.log('I am doing something complicated.');
    return ((count1 * 10000) / 3) * 2020 - 1000;
  }, [count1]);

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <button onClick={handleClick1}> Click Me 1 </button>
      <>   </>
      <button onClick={handleClick2}> Click Me 2 </button>
      <h2>{**doSomethingComplicated**}</h2>
    </div>
  );
```

# Profiler

- A tool that can test the performance of components render and re-render time.
- Wrap the component that you want to test.

```jsx
import React, { Profiler } from 'react';

const HomePage = () => (
	<HomePageContainer>
		<Profiler id='Directory' onRender={(id, phase, actualDuration) => {
			console.log({
				id,
				phase,
				actualDuration
			});
		}}>
			<Directory />
		</Profiler>
	</HomePageContainer>
);
```