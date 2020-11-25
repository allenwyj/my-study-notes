# Context API

---

# Context API

## What is Context

- Context provides a way to pass data through the component tree without having to pass props down manually at every level.
- 例如，在组件树中，有很多组件需要共享一个全局的属性，其中根节点可以把他们需要的属性 provide 到Context中，需要的组件可以通过consume context来获取需要的参数。

## When to Use Context

- Context is designed to share data that can be considered "global" for a tree of React components, such as the current authenticated user, theme, or preferred language.

## How to Use Context

- `React.createContext('light')`
    - 定义一个新的context，并设置其默认值
- Provider 是Context的一个组件，这个组件可以接收一个参数叫做value，是一个属性。传给他的属性是什么，那么他下面所有的子节点Consumer拿到的参数就是什么。当value发生改变的时候，子组件都会同步更新。

```jsx
// define the context
// 接收参数，可以是任意的值（可以是object的数组，常量字符串等）
const ThemeContext = React.createContext('light');

class App extends React.Component {
	render() {
		return (
			<ThemeContext.Provider value="dark">
				<ThemedButton />
			</ThemeContext.Provider>
		};
	}
}

function ThemedButton(props) {
	return (
		<ThemeContext.Consumer>
			{theme => <Button {...props} theme={theme} />}
		</ThemeContext.Consumer>
	);
}
```

---

# Using Context-API

Context API is from `react` library, so in order to use this, we need to import it first.

`import {createContext} from 'react';`

## Putting value into Context-API

`createContext` is a method that can take anything (Strings, integers, objects, functions) and what we are passing into it will be the `INITIAL_DATA`.

We will use it like a component.

```jsx
import { createContext } from 'react';
import SHOP_DATA from './shop.data'; // data that we wanna put into.

const CollectionsContext = createContext(SHOP_DATA);
// const CollectionsContext = createContext(undefined); // setting to undefined if we don't have any initial value.

export default CollectionsContext;
```

## Using Context-API inside a Component - Consuming Context

**It only allows to access the initial value that we set in the context (not dynamic value).**

We will need to `import` our `context` into the component that we are going to use it.

### Method A - using OurContext.Consumer Component

We will use `OurContext.Consumer` to wrap our actual component and we will be able to access the value that we stored in the `context` inside the context. Within `<OurContext.Consumer>{values => {//component}}</OurContext.Consumer>`, it is a function which is taking what we stored in the `context` as the parameter and the parameter is passed into the function body.

```jsx
const CollectionPage = ({ match }) => {
  return (
    <CollectionsContext.Consumer>
      {
        // Consumer brings collections into CollectionContext component
      }
      {collections => {
        const collection = collections[match.params.collectionId];
        const { title, items } = collection;
        return (
          <div className="collection-page">
            <h2 className="title">{title}</h2>
            <div className="items">
              {items.map(item => (
                <CollectionItem key={item.id} item={item} />
              ))}
            </div>
          </div>
        );
      }}
    </CollectionsContext.Consumer>
  );
};
```

### Method B - using useContext hook

`function useContext<T>(context: Context<T>): T`

Accepts a context object (the value returned from `React.createContext`) and returns the current context value, as given by the nearest context provider for the given context.

```jsx
const CollectionPage = ({ match }) => {
	// pulling what we stored in the context.
  const collections = useContext(CollectionsContext);
  const collection = collections[match.params.collectionId];
  const { title, items } = collection;
  
  return (
    <div className="collection-page">
      <h2 className="title">{title}</h2>
      <div className="items">
        {items.map(item => (
          <CollectionItem key={item.id} item={item} />
        ))}
      </div>
    </div>
  );
};
```

## OurContext.Provider - dynamic using context

In order to use dynamic value that we stored in the our context, we need to use `provider` to set our new value into the context.

**Important:**

`**consumer` will actually go up the tree of components until the `consumer` finds the same `context` AND it will also look for a `provider`. If there is no provider that has supplied a new value, then it will just use the initial state that we instantiated our context (the initial value). The `provider` has to wrap the components which we need to access this dynamic value.**

- `value` property will be the place to update our context.
- any value that is consuming the `currentUser` will re-render whenever this value changes.

```jsx
<CurrentUserContext.**Provider** **value**={this.state.**currentUser**}>
  <Header />
</CurrentUserContext.**Provider**>
```

### Combining useState and useContext

Leveraging the local state values and local state functions that allow to set those values inside of a context by passing them into the provider and matching the actual context value to have a very similar shape.

```jsx
// we can create context with some value and functions
import { createContext } from 'react';

const CartContext = createContext({
  hidden: true,
  toggleHidden: () => {}
});

export default CartContext;

// inside a parent component, the values of state will be set into CartContext.
const Header = () => {
  const currentUser = useContext(CurrentUserContext);
  const [hidden, setHidden] = useState(CartContext);
  const toggleHidden = () => setHidden(!hidden);
	...

	return (
		...
		<CartContext.Provider
      value={{ hidden: hidden, toggleHidden: toggleHidden }}
    >
      <CartIcon />
    </CartContext.Provider>
		...
	);
};

// we will be able to consume this dynamic context inside the child component which is wrapped by provider
const CartIcon = ({ itemCount }) => {
  const { toggleHidden } = useContext(CartContext);

  return (
    <div className="cart-icon" onClick={toggleHidden}>
      <ShoppingIcon className="shopping-icon" />
      <span className="item-count">{itemCount}</span>
    </div>
  );
};
```

## Pros and Cons of Using Context-API

**If we know the application that we are building is going to be large (a large scaled application that has a lot of complexity and features and reusable components) - choosing Redux would be better (more power and flexibility including all of the asynchronous event handling and the ability to reuse the components)**

**Small to medium sized projects like a portfolio project or a portfolio itself or a landing page etc. - better to go with Context-API**

### Pros

- Efficient and easy to use
- A lightweight solution when it comes to local storage management.

### Cons

- The components are tightly coupled by using Context-API - Component may not be reusable.
- We need to wrap other components inside  `provider` - If the application is going biggier and bigger, then we need to wrap more and more `providers`.