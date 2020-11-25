# ReactJS

---

**React does these for us**

1. Don't touch the DOM, I'll do it
2. Build websites like lego blocks
3. Unidirectional data flow
4. UI, The rest is up to you

---

**The job of a react developer**

1. Decide on Components
2. Decide the State and where it lives
3. What changes when state changes

# What is React

React 始终整体“刷新”页面，无需关心细节

- 1个新概念
    - 组件，用组件的方式描述UI  ⇒ 解决传统UI管理的问题
- 4个必须API
- 单向数据流 - One way data flow (unidirectional data flow)
    - 数据流单向流

        ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled.png)

- 完善的错误提示
    - 若犯了低级错误，console会有完善提示

**Client side JavaScript library**

- single page application
- created & maintained by facebook
- used to build dynamic user interfaces
- "Component" based - every piece of the front end interface is an individual component

## Installation of React

- `npx create-react-app folderName`

### Some basic clean up things

- To change the app component to a class based component ⇒ For learning purpose
    - serviceWorker.js - if we don't need to do with PWA and cache and something like these.
    - logo.svg
    - index.css
    - App.test.js - not going to do anything test here.

## Keep React Version Updating and Safe

- Check the current version in the project folder

    `npm list react react-dom react-scripts`

- Adding `^` to the version number for react, react-dom and react-script in the package.json file.
- Update the packages

    `npm update`

- Vulnerabilities

    Some minor security concerns that has to do with either a dependency that you have installed or a dependency that  your packages depend, but you don't know that you also installed. To Quick Fix it:

    `npm audit fix` - update those packages to the version which doesn't have the security concerns. 

## Some methods

### `render()`

- render is actually wha't called the lifecycle method and it runs at a certain point when the components are loaded.
- `render()` renders the output

## onChange in React and HTML

- React `onChange` and HTML onchange are different.
- React `onChange` will be triggered as soon as **user types one character**.
- HTML `onchange` will be triggered when **user moves the mouse away**. It gets triggered before `onBlur`.

# React Element

- React elements are immutable. Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.
- With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`.
- However, most React apps only call ReactDOM.render() once.

# JSX

JavaScript Syntax Extension

- JSX allows us to directly write HTML tag in JS coding.
- It's basically syntacitic sugar to be able to write the output of our component in an XML or HTML-like way. **dynamic**
- If we use pure JS codes, it will be very messy.
    - because each child element will be inside its parent element and created by using `React.createElement()`

    ```jsx
    // Using JSX
    class App extends Component {
      render() {
        return (
          <div className="App">
            <h1>Hello from React</h1>
          </div>
        );
      }
    }

    // Using pure JS codes
    class App extends Component {
    	render() {
    		return React.createElement(
    			'div',
    			{ className: 'App' },
    			React.createElement('h1', null, 'Hello from React')
    		);
    	}
    }
    ```

## Pure HTML and JS code to Perform React

### Using functional component

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="root">React not Rendered</div>
    <script src="https://unpkg.com/react@16.8.6/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16.8.6/umd/react-dom.development.js"></script>
    <script>
      const Person = props => {
        return React.createElement('div', {}, [
          React.createElement('h1', {}, props.name),
          React.createElement('p', {}, props.occupation),
        ]);
      };
      const App = () => {
        return React.createElement(
          'div',
          {},
          React.createElement('h1', {}, 'React is rendered~~'),
          **// the first argument is to define the class of the element,
          // the second argument is to pass in props
          // the third argument is to pass the child element**
          React.createElement(Person, {
            name: 'Allen',
            occupation: 'developer',
          })
        );
      };

      ReactDOM.render(
        React.createElement(App),
        document.getElementById('root')
      );
    </script>
  </body>
</html>
```

### Using class based component

```jsx
<script>
  class App extends React.Component {
    render() {
      return React.createElement(
        'div',
        {},
        React.createElement('h1', {}, 'React is rendered~~'),
        // the first argument is to define the class of the element,
        // the second argument is to pass in props
        // the third argument is to pass the child element
        React.createElement(Person, {
          name: 'Allen',
          occupation: 'developer',
        })
      );
    }
  }

  const Person = props => {
    return React.createElement('div', {}, [
      React.createElement('h1', {}, props.name),
      React.createElement('p', {}, props.occupation),
    ]);
  };

  ReactDOM.render(
    React.createElement(App),
    document.getElementById('root')
  );
</script>
```

## 在JSX中使用表达式

1. JSX 本身也是表达式

    `const element = <h1> Hello, world!</h1>;`

2. 在属性中使用表达式

    可以在属性中，使用JS代码

    `<MyComponent foo={1 + 2 + 3 + 4} />`

3. 延展属性

    `const props = {firstName: 'Ben', lastName: 'Hector'};`

    `const greeting = <Greeting {...props} />;`

4. 表达式作为子元素

    `const element = <li>{props.message}</li>;`

## JSX Class

- using `className` instead of using `class`

## htmlFor

- using `htmlFor` instead of using `for`

## In-line Style

React does give us to specify the in-line style in JSX, the css syntax in JSX will be written in camelCase style.

- Using **double curly brackets**
    - We can use in-line style in JSX, using **double curly brackets**

        `<div style={{ backgroundImage: `url(${imageUrl})` }} className='menu-item'></div>`

        `<img ... style={{ }} />`

    - The attribute in the style is a little different, instead of using the dash '-' `background-color`, in JSX, it uses **Camel case** like `backgroundColor`
    - And passing the **string** to the JSX.

        `<img ... style={{ backgroundColor:'red' }} />`

---

- Using **variable**
    - we can pass a variable for the style

        ```jsx
        export class Users extends Component {
            ...

            render() {
                return (
                    <div **style={userStyle}**>
                        {this.state.users.map(user => (
                            <UserItem key={user.id} user={user} />
                        ))}
                    </div>
                )
            }
        }

        **const userStyle = {
            display: 'grid',
            gridTemplateColumns: 'repeat(3, 1fr)',
            gridGap: '1rem'
        };**
        ```

## Rules

- JSX expression MUST only have one parent element
- If we don't want to have another `div` tag to contain our stuffs, we can use Fragment which we need to import it first from React.

    `<React.Fragment> </React.Fragment>` OR `<> </>` using these angle brackets

    - Fragment way is better
- React will consider ervey tag which is in camelCase is a native DOM node, like `div`
    - every tage which is in CamelCase is a customised component.
- We can directly use 属性语法 in JSX tag, like `<menu.Item />`

## Expression & Conditionals in JSX

### Using {} Curly Brackets

- `{expression}`
    - You can define the variable inside the render method and use curly brackets to call it.
    - Also, you can do any JS expression inside the curly brackets.
        - `{variableName}`, `{1+1}`, `{name.toUpperCase()},` `{foo()}`,  `{this.foo()}` (if foo() is defined as method) or `{loading ? <h4>Loading...</h4> : <h1> Hello {name} </h1>}`
        - using `{... && ...}` instead of using ternary operator.

        ```jsx
        const showName = false;
        const name = 'Allen Wu';
        **{showName && name} => If showName is true, then do name. 
        Same as => {showName ? name : null}** 
        ```

---

# Virtual DOM

## How to Compare the Difference between DOMs

### 广度优先分层比较 - 性能优化到时间复杂度为O(n)

- 一层一层的比较
- 节点类型发生变化
    - 删除旧节点，创建一个新节点，然后分配到旧节点parent的位置
- 节点跨层移动
    - 若发现旧节点在新的DOM中没有了，React会将这个节点及其子元素全部删除
    - 若发现有新的节点出现，会创建这个新的节点及其所有子元素到当前DOM tree上

### 虚拟DOM的两个假设

- 组件的DOM结构是相对稳定的，少跨层移动
    - 利用了HTML DOM的结构相对较稳定的一个特点进行的算法优化
- 类型相同的兄弟节点可以被唯一标识，主要用在节点之间的位置顺序发生变化
    - 在实际开发过程中，需要我们手动的做一个介入，每个相同类型的子元素用一个Key去标识
        - key的使用，可以提高性能

---

# Components, Props & PropTypes

## Import & Export Component

- If we are using `export default`, then when we import this component, we don't need to use {} to wrap it.

    `import Card from './components/Card';`

- If we are using named export, then we need to use {} to wrap it.

    `import { Card } from './components/Card';`

## Components

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%201.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%201.png)

- Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called props) and return React elements describing what should appear on the screen.
- Components let us split the UI into independent, reusable pieces, and think about each piece in isolation.
- We can put our components together in one file, or we can have a folder to split them based on their functionality (preferred way)
- Always start component names with a capital letter.
- For example, create a JS file: /src/components/layout/Navbar.js
    - Thanks to ES7 React... snippets, we can use shortcut to automatically generate the basic codes for us.
    - `rce` - Generate a class based components

        ```jsx
        import React, { Component } from 'react'

        export class Navbar extends Component {
            render() {
                return (
                    <div>
                        <h1>Navbar</h1>
                    </div>
                )
            }
        }

        export default Navbar
        ```

    - `racf` - Generate functional components

        ```jsx
        import React from 'react'

        export const Spinner = () => {
            return (
                <div>
                    
                </div>
            )
        }
        ```

- In the App.js, we can simply use this Navbar component by importing it and typing `<Navbar />`

### Class Component

```jsx
import React, { Component } from 'react';
import './App.css';

class App extends Component {

  constructor() {
		// calling the constructor in Component class.
    super();

    this.state = {
      monsters: [],
      searchField: '',
    };
  }
	
	...

  render() {
    return (
      <div className="App">
        <SearchBox
          placeholder="Search Monsters..."
          handleChange={this.handleChange}
        />
        <CardList monsters={filteredMonsters} />
      </div>
    );
  }
}

export default App;
```

- We might have problem when we code `this` with a class method

    ```jsx
    class App extends Component {
    	...

    	// this keyword will see the context of the arrow function
    	// which is the context of handleChange method
    	// So, it won't cause any problem
    	handleChange = e => {
        this.setState({ searchField: e.target.value });
      };

    	// Causing error, because JS doesn't determine the context for
    	// a function by default, we need to explicitly state what 
    	// context you want this to be.
    	handleChange(e) {
    		this.setState({searchField: e.target.value });
    	};
    	...
    }
    ```

### Stateless Functional Components

- shortcut - `rafce`
- Traditionally, functional components don't have state and lifecycle methods. We just want to use them to render some HTML.
- A functional component is just a component that gets some props and returns some HTML.
- Easy to read and easy to test.
- Traditionally, we will have stateless functional components and class based components which we need them to perform state and lifecycle method. But with hooks, they can have state in functional components as well.

### Converting Stateless Class-based Component to Functional Component

- The class-based components are converted to functional components because they no longer have state so they don't need to be classes
- No need to extends from Component.
- The functional component will be a function which have `props` as the parameter

    `const UserItem = props ⇒ {...}`

- Since we are not going to use class based component, so the functional components are defined as functions. And there is no need to have render() method
- In the functional component, we are not gonna use `this.props` like what we did in the class based component.
- Defining Props and PropTypes
    - They are no loner as static method, so they will be defined by `ComponentName.props` and `ComponentName.propTypes`

```jsx
import React from 'react';
import PropTypes from 'prop-types';

// props can be destructured
**const Navbar = (props) => {**
    return (
        <nav className="navbar bg-primary">
            <h1>
                <i className={**props.icon**} /> {**props.title**}
            </h1>
        </nav>
    );
};

// setting up the default prop values
**Navbar.defaultProps** = {
    title: 'Github Finder',
    icon: 'fab fa-github'
};

// telling the expected data types for these props
**Navbar.propTypes** = {
    title: PropTypes.string.isRequired,
    icon: PropTypes.string.isRequired
};

export default Navbar;
```

- Destructuring the props when it is passing into the function.
    - For example

        ```jsx
        const UserItem = (**{user: { login, avatar_url, html_url }}**) => {
        	// we can directly use login, avatar_url, html_url now.
        };

        **const Navbar = ({ icon, title }) => {**
            return (
                <nav className="navbar bg-primary">
                    <h1>
                        <i className={**icon**} /> {**title**}
                    </h1>
                </nav>
            );
        };
        ```

### 受控组件 Controlled Component vs 非受控组件 Uncontrolled Component

主要针对的是form这个元素

- 表单元素状态由使用者维护 - 受控
    - We have to determine `value` and `onChange`， `value`是什么取决于外面给它的属性，而不是user的输入，用户输入任何值，若外部没有告诉这个input发生变化的话，它的值是不会变化的。

    ```jsx
    <input
    	type="text"
    	value={this.state.value}
    	onChange={evt => 
    		this.setState({ value: evt.target.value })
    	}
    />
    ```

- 表单元素状态DOM自身维护 - 非受控
    - 它的状态是由内部维护的，外部要用的时候是用其它方式去拿到它的值。这样的组件是不需要传递`value`和`onChange`给它的。但是一定要让它知道，你的原声DOM node是什么。

    ```jsx
    <input
    	type="text"
    	ref={node => this.input = node}
    />
    ```

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%202.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%202.png)

### 数据状态管理：DRY 原则

- 能计算得到的状态就不要单独存储
- 组件尽量无状态，所需数据通过props获取

## Higher-Order Components

- A higher-order component is a function that takes a component and returns a new component. But we don't export any JSX in HOC.

    `const EnhancedComponent = higherOrderComponent(WrappedComponent);`

- 高阶组件的出现是可以减少一些代码的重复使用；可以避免在使用某些功能时，进行一些不必要的属性传递。在WrappedComponent组件中，我们输出的不是这个组件自身，而是以下新组建：

    `export default withTimer(WrappedComponent);`

- 此时，这个组件可能会同时接收来自父组件的属性，以及高阶组件传递的属性。例如withTimer里面，会对WrappedComponent在其封装之前传入一个time的属性。所以，在WrappedComponent中，如果我们想要使用这个传入的time属性，只需要用this.props.time即可

```jsx
import React from "react";

export default function withTimer(WrappedComponent) {
  return class extends React.Component {
    state = { time: new Date() };
    componentDidMount() {
      this.timerID = setInterval(() => this.tick(), 1000);
    }

    componentWillUnmount() {
      clearInterval(this.timerID);
    }

    tick() {
      this.setState({
        time: new Date()
      });
    }
    render() {
      return <WrappedComponent time={this.state.time} {...this.props} />;
    }
  };
}
```

## Function as Child Component - 函数作为子组件

- "函数作为子组件“ 是一个组件，这个组件接收一个函数作为其子组件。
- MyComponent会是一个<div> </div>，使用的时候，他会把一个函数作为它的子组件进行对<div> tag之间的child element的生成

```jsx
class MyComponent extends React.Component { 
  render() {
    return (
      <div>
        {this.props.children('Scuba Steve')} {// <div>Scuba Steve</div>}
      </div>
    );
  }
}
MyComponent.propTypes = {
  children: React.PropTypes.func.isRequired,
};
```

- 使用MyComponent 组件： name 就是在render MyComponent的时候传入的值

    子组件为：`{(name) => (<div>{name}</div>)}`

    or：`{(name) => (<img src="/scuba-steves-picture.jpg" alt={name} />)}`

    ```jsx
    <MyComponent>
      {(name) => (
        <div>{name}</div>
      )}
    </MyComponent>

    // OR
    <MyComponent>
      {(name) => (
        <img src="/scuba-steves-picture.jpg" alt={name} />
      )}
    </MyComponent>
    ```

## Props

- Props are basically properties that you can pass into a component from outside.
- And it is an Object
- Pass-in - title is one of the feilds in props

    `<Navbar title='Hello' />`

- Read props

    `console.log(this.props.title); // Hello`

### Child Props - props.children

- Anything between the beginning tag and the closing tag is the `children` props

    So everything in the `<h1></h1>` is the `children` prop

    ```jsx
    <CardList name='Allen'>
    	<h1>
    		Hello
    	</h1>
    </CardList>
    ```

- We can also read `<h1> Hello </h1>` inside the `CardList` component by:

    `{this.props.children}` - if it is a class based component.

    `{props.children}` - if it is a functional component.

### Passing props down

- In App.js file, the `title`and `icon` are the properties that we want to pass in to the `Navbar`.

    ```jsx
    import React, { Fragment, Component } from 'react';
    import Navbar from './components/layout/Navbar';
    import './App.css';

    class App extends Component {
      render() {
        const **title='Github FInder'**
        return (
          <div className="App">
            <Navbar **title={title} icon='fab fa-github'**/>
          </div>
        );
      }
    }

    export default App;
    ```

- In Navbar.js, the way we **use the properties** is using `{this.props.title}` and `{this.props.icon}`

    ```jsx
    <nav className="navbar bg-primary">
        <h1>
            <i className={this.props.icon} /> {this.props.title}
        </h1>
    </nav>
    ```

### Setting default values for props

- We also can define a **default property** inside the class by defining:
    - If we have values passed in, the default value will be overwritten.

    ```jsx
    static defaultProps = {
        title: 'Github Finder',
        icon: 'fab fa-github'
    };
    ```

    - Functional Component

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    // props can be destructured
    **const Navbar = (props) => {**
        return (
            <nav className="navbar bg-primary">
                <h1>
                    <i className={**props.icon**} /> {**props.title**}
                </h1>
            </nav>
        );
    };

    // setting up the default prop values
    **Navbar.defaultProps** = {
        title: 'Github Finder',
        icon: 'fab fa-github'
    };

    // telling the expected data types for these props
    **Navbar.propTypes** = {
        title: PropTypes.string.isRequired,
        icon: PropTypes.string.isRequired
    };

    export default Navbar;
    ```

- **Hints**: Using destructuring when we are passing props down
    - Every other key value pair we want to pass in
        - `{...otherSectionProps}` is equivalent to `title={title} imageUrl={imageUrl} linkUrl={linkUrl}`

    ```jsx
    <div className="directory-menu">
      {
        // destructuring each section and passing them into MenuItem
        this.state.sections.map((**{ id, ...otherSectionProps }**) => (
          // desturcturing and pass the rest fields of section, and use the same name as the field name.
          <MenuItem key={id} **{...otherSectionProps}** />
        ))
      }
    </div>
    ```

### Passing Props To A Component without Value

This often comes up when using **HTML form elements**, with attributes like **disabled**, **required**, **checked** and **readOnly**. **Omitting the value** of an attribute causes JSX to treat it as **true**. To pass **false** an attribute expression **must be used**.

- *These two are **equivalent** in JSX for disabling a button*

```jsx
<input type="button" disabled />;
<input type="button" disabled={true} />;
```

- *And these two are **equivalent** in JSX for not disabling a button*

```jsx
<input type="button" />;
<input type="button" disabled={false} />;
```

[How to pass props without value to component](https://stackoverflow.com/questions/37828543/how-to-pass-props-without-value-to-component)

### super(props) - Using Props in the Constructor

- If we want to use `this.props` inside of the constructor, we need to pass it into `constructor()` and `suer()`

    ```jsx
    class App extends Component {
      constructor(props) {
    		super(props);
    		this.state = {
    			state1: []
    		};
    		this.props = props; // will be error if we didn't pass props in.
    }

    ```

### Props are Read-Only - Strict Rule

- **All React components must act like pure functions with respect to their props**
- It means we can't do something like, we pass an array as the argument and we mutate one of the array element.
- In order to overcome this rule, React introduces **state** which allows React components to change their ouput over time in response to user actions, network responses, and anything else, without violating this rule.
    - pass the props down to the component and save it as one of the state property.

### Passing props up

- We can use props to pass data from the child component to the parent component.
    - In Search component (child), once we submit the form, it's calling `onSumbit()`.
    - Then, we are calling a `searchUser()` function in the props and we are passing the `text` into it.
    - In App component (parent), we set the prop here to call `this.searchUsers` in the App.js, which is calling `searchUsers` function in the App.js.

    ```jsx
    File: App.js
    // Search Github users
    **searchUsers = (text) => {
      console.log(text);
    }**

    render() {
      return (
        <div className='App'>
          <Navbar title='Github Finder' icon='fab fa-github'/>
          <div className="container">
            **<Search searchUsers={this.searchUsers} />**
            <Users loading={this.state.loading} users={this.state.users} />
          </div>
        </div>
      );
    }

    File: Search.jsx
    onSubmit = e => {
        e.preventDefault();
        **this.props.searchUsers(this.state.text);**
        this.setState({ text: '' })
    }
    ```

## PropTypes - Props Type Checking

- `PropTypes` is basically type checking, it will tell you if you know your `prop` should be a `string` or a `number` or an `array` or `object` or anything.
    - It will show the warning in the console, but it can still compile successfully.
- Using `impt` shorcut to import proptypes
- Using `ptor`, `ptar`, `ptsr`, `ptnr` etc.. to define the rule.
    - `p`: Prop, `t`: Types, `s`: string, `o`: object, `a`: array, `r`: isRequired
- Defining propTypes in where we actually use the props.

    ```jsx
    **File: Navbar.js**
    static propTypes = {
        title: PropTypes.string.isRequired,
        icon: PropTypes.string.isRequired
    };
    ```

---

# State and Lifecycle

## Component State

- State is similar to props, but it is private and fully controlled by the component.
- There is a couple of ways to add state to a class based component
    1. Using `constructor` - wouldn't recommend it unless you need to use a constructive for something else
        - need to call `super()` inside the constructor.
        - define the state by

            ```jsx
            this.state = {
            	// state is an object
            };
            ```

        - getting the value from the state

            `{this.state.fieldName}`

    2. Directly defining state inside the component class as a class field declaration.

        ```jsx
        class UserItem extends Component {
          state = {
              id: 'id',
              login: 'allen',
              avatar_url: 'https://avatars2.githubusercontent.com/u/48901631?s=460&u=f33c04b183eebc59a4b194a85845cf9ef884647f&v=4',
              html_url: 'https://github.com/allenwyj'
          };
        	...
        }
        ```

- **Destructuring** the value of state to avoid type `this.state` again and again
    - NOTE: Can be improved when we are using in functional component.

    ```jsx
    class UserItem extends Component {
    	...
      render() {
    		const { login, avatar_url, html_url } = this.state;
    		// using the variable
    	}
    }
    ```

## Using State Correctly

### Shallow Merge - 浅合并

In a shallow merge, the properties of the first object are overwritten with the same property values of the second object.

Lets look at an example. Setup:

```jsx
var obj1 = {
  foo: {
    prop1: 42,
  },
};

var obj2 = {
  foo: {
    prop2: 21,
  },
  bar: {
    prop3: 10,
  },
};

```

Shallow:

```jsx
var result = {
  foo: {          // `foo` got overwritten with the value of `obj2`
    prop2: 21,
  },
  bar: {
    prop3: 10,
  },
};

```

Deep:

```jsx
var result = {
  foo: {
    prop1: 42,
    prop2: 21,    // `obj2.foo` got merged into `obj1.foo`.
  },
  bar: {
    prop3: 10,
  },
};
```

### Batch Updating

"The main idea is that no matter how many setState calls you make inside a React event handler or synchronous lifecycle method, it will be batched into a single update."

### Overview

- Update to a component state should be done using `setState()`
- You can pass an object or a function to `setState()`
- Pass a function when you can to update state multiple times
- Do not depend on `this.state` immediately after calling `setState()` and make use of the updater function instead.

### setState()

It will tell React there is a modification on the State. So React will re-render the component and update the DOM accordingly.

- **Never run setState() directly inside render(), it will cause a loop bug.**
- **Do NOT modify State directly**
    - The only place where you can assign `this.state` is the constructor.
    - Correct way to update the state and trigger the re-render of a component ⇒ **using** `setState()`

        `this.setState({comment: 'Hello'});`

    - Wrong way which will not re-render a component

        `this.state.comment = 'Hello';`

- **State updating may be Asynchronous - React Event Handler and Lifecycle Methods**

    **`setState()` does NOT always immediately update the component. It may batch or defer the update until later. This makes reading `this.state` right after calling `setState()` a potential pitfall.**

    `setState()` takes a argument `isBatchUpdating: boolean` to determine whether component should do batch updating or not. **React event handler** (i.e. `onClick`, `onChange` etc.) and **lifecycle methods** will pass `true` to `setState()`. Others will be `false`, then updating will be **synchronous**.

    - React may batch multiple `setState()` calls into a single update for performance. - **Shallow Merge**
    - Because `this.props` and `this.state` may be updated asynchronously, you should **NOT** rely on their values for calculating the next state.
    - For example, this code may **fail** to update the counter:

        The reason why this doesn't work is because React will batch these `setState()` together and perform a shallow merge with these three `{count: state.count + 1}`, that will result in the equivalent of:

        ```jsx
        // Shallow Merge
        Object.assign(
          previousState,
          {count: state.count + 1},
          {count: state.count + 1},
        	{count: state.count + 1},
          ...
        )
        // **Subsequent calls will override values from previous calls in the same cycle, so the count will only be incremented once.**
        ```

        **Wrong** Example:

        ```jsx
        // Wrong
        this.setState({
          counter: this.state.counter + this.props.increment,
        });

        -----------------------------------

        // 假设 state.count === 0
        this.setState({count: state.count + 1});
        this.setState({count: state.count + 1});
        this.setState({count: state.count + 1});
        // state.count === 1, 而不是 3
        ```

    - To fix it, use a second form of `setState()` that accepts a **helper function** rather than an object. That function will receive **the previous state** as the first argument, and **the props at the time** the update is applied as the second argument:
        - This can gurantee the state and props are the latest versions. - **If you need to set the state based on the previous state.**
        - **Both state and props received by the updater function are guaranteed to be up-to-date. The output of the updater is shallowly merged with state. But it is still batch updating the state and re-render the component once.**

        ```jsx
        // Correct
        // () returns the object

        // 假设 state.count === 0
        // 每次 setState 传进来的state就是上一个 setState(helper) 加过的值
        this.setState((preState, preProps) => **(**{
          counter: preState.counter + preProps.increment
        }**)**);

        this.setState((preState, preProps) => **(**{
          counter: preState.counter + preProps.increment
        }**)**);

        this.setState((preState, preProps) => **(**{
          counter: preState.counter + preProps.increment
        }**)**);

        // state.count === 3
        ```

        ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%203.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%203.png)

        ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%204.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%204.png)

    - If we want to **do something right after** we set it in the state.
        - To avoid having the asynchronous issue. SO first argument finished, then run the second one.

        we can pass an anonymous function to `setState()`

        For example, when we are accepting the user input from an input box, and use it to search something. If we `console.log` the state right after the `setState()`, we might have asynchronous problem. 

        So, we can pass an anonymous function as **the second argument** into `setState()`, then it will wait until the **state merge** is **finished**, then the anonymous **will be executed**.

        ```jsx
        this.setState({ searchField: e.target.value }, () => {console.log(this.state)});
        ```

- **setState() 也有同步发生的时候**
    - Note that **batch updating** does not always work. In an event handler that uses an asynchronous operation of different kinds, such as **`async/await`**, **`then/catch`**, **`setTimeout`**, **`setInterval`, `addEventListener`,`fetch`**, etc. Separate state updates will not be batched.

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%205.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%205.png)

    [](https://medium.com/swlh/react-state-batch-update-b1b61bd28cd2#:~:text=%E2%80%9CReact%20may%20batch%20multiple%20setState,%E2%80%9D%2C%20according%20to%20React's%20documentation.&text=%E2%80%9CCurrently%20(React%2016%20and%20earlier,%E2%80%9D%20%2C%20according%20to%20Dan%20Abramov.)

- **State updates are merged**
    - When you call `setState()`, React merges the object you provide into the current state.
    - If the state contains different independent variables, we can update any of them independently with seperate `setState()` calls.
- setState() 循环调用的风险

    当调用 setState 时，实际上会执行 enqueueSetState 方法，最终通过 enqueueUpdate 执行 state 的更新。但如果在 shouldComponentUpdate 或者 componentWillUpdate 方法里调用 this.setState 会导致调用 updateComponent 方法进行组件更新，而 updateComponent 方法又会调用 shouldComponentUpdate 和 componenitWillUpdate 方法，因此造成循环调用，使得浏览器沾满后崩溃。

- **总结**

    setState的异步和同步取决于调用的环境：在一般的生命周期函数和react event handler中调用setState，setState的调用是异步发生的，所以我们需要使用helper function 来获取最新的state（最新的state存储在dirty component中？），但 helper function更新state并不会触发组件刷新，所以我们只能“看”到的 state 是组件上一次渲染完成的时候的值（即state更新的值无法被外部获取），当所有setState都被批量处理完成后，才会在最后触发组件更新。

    若在类似addEventListener， async/await，setTimeout等函数中调用setState()， 将是同步调用，每一次setState的结束都会触发组件更新，然后在运行下一次的setState()，并不会batch updating。

## Lifecycle Methods

- In applications with many components, it's very important to free up resources taken by the components when they are destroyed.
    1. **组件将要挂载时触发的函数：componentWillMount**
    2. **组件挂载完成时触发的函数：componentDidMount**
    3. **是否要更新数据时触发的函数：shouldComponentUpdate**
    4. **将要更新数据时触发的函数：componentWillUpdate**
    5. **数据更新完成时触发的函数：componentDidUpdate**
    6. **组件将要销毁时触发的函数：componentWillUnmount**
    7. **父组件中改变了props传值时触发的函数：componentWillReceiveProps**

### React Lifecycle Methods Diagram

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%206.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%206.png)

- Render
    - 计算一些当前的状态
- Pre-Commit
    - React还没有真正的更新DOM，但可以去读取DOM的内容的。
- Commit
    - React把当前的状态映射到DOM的时候，需要实际的去更新DOM的节点。
- **Mounting** 挂载时
    - constructor()
        - 组件在更新到界面上之前，需要先创建出来。
    - getDerivedStateFromProps()
        - 用从外部的属性去初始化一些内部的状态，返回的状态都可以merge到当前状态上。
    - render()
        - 用于描述DOM UI结构的地方
    - componentDidMount()
        - 发起ajax请求，处理、定义外部资源的事情。
- **Updating** 更新时
    - 这个情况发生在有新的属性传入(New props) 或者是状态发生变化(setState()) 或是我们希望对UI进行一个强制更新即使状态属性都没有变化(forceUpdate())
    - getDerivedStateFromProps()
        - 用从外部的属性去初始化一些内部的状态，返回的状态都可以merge到当前状态上。
    - shoudComponentUpdate()
        - 告诉组件是不是真的需要render，这个地方用户是可以介入的过程，在这个过程可以进行一些性能优化的事情，例如有props即使发生变化了，但是不需要进行更新UI。
    - getSnapshotBeforeUpdate()
        - 在更新之前，查看当前DOM的一些事情
    - componentDidUpdate()
        - 每次在React更新完DOM后，我们都可以在这里做一些自己需要做的事情，比如说有一篇文章根据当前的ID参数决定展示什么内容，可在这个方法里面根据ID获取不同的内容然后更新到UI上。
- **Unmounting** 卸载时
    - 当一个组件从界面上消失时，需要自己去销毁自己
    - componentWillUnmount
        - 做一些资源释放的事情。
- Mounting phase with fetching data from API.
    - constructor and render methods get called twice due to StrictMode in the developing mode.

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%207.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%207.png)

    [](https://www.lizenghai.com/archives/50409.html)

### render()

- render是一个地方去计算背后的虚拟DOM，这个虚拟DOM是用来维持一个内部的UI的状态，从而去计算diff 等等。
- In the phase of mounting, React will render the base components with the data declared in the `constructor` first, then when the first `render()` finished, React will call `componentDidMount()` method to fetch some data from outside sources (like APIs) if exists. During the method, if any `setState()` gets called, `render()` method will be fired again. After the business logic in the `componentDidMount()` finished, then the mounting phase is over when the `componentDidMount()` finished.

### getDerivedStateFromProps()

- When we need to initialise our state from props, we can use it.
- 然而，这个方法不推荐使用。通常情况下，当我们需要从props中得到某个状态，我们都可以通过对props的计算动态的得到state，不需要单独存储这个状态。如果选择单独的存储这个值，我们需要一直对它的值进行更新，从而保持两者的一致，这无疑是增加了复杂度和出现bug的可能性。
- 每次render都会调用
- 典型场景：表单控件获取默认值。
    - 因为在表单中，除了接收用户的输入值以外，一开始还会给一个默认值，一旦用户修改后，这个默认值就没有用了。所以一开始的内部state是来自内部的默认值，改变之后，这个state就是来自用户的输入值。

### **shouldComponentUpdate() - Updating**

`shouldComponentUpdate(nextProps, nextState)`

- Use this method to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.
- This method is able to get the new props or state before they are actually set to `this.state` or `this.props`
- If return `false`, then `render()`, `componentDidUpdate()` won't be called.

    ```jsx
    shouldComponentUpdate(nextProps, nextState) {
    	return nextProps.text !== this.props.text;
    }
    ```

### getSnapshotBeforeUpdate() - Updating

`getSnapshotBeforeUpdate(prevProps, prevState)`

- This method is invoked **right before the most recently rendered output is committed to e.g. the DOM** (渲染结果提交到DOM节点之前调用）. It enables your component to capture some information from the DOM (e.g. scroll position) before it is potentially changed.
- **Any value returned by this lifecycle will be passed as a parameter to `componentDidUpdate()`.**
- This use case is not common, but it may occur in UIs like a chat thread that need to handle scroll position in a special way.
- A snapshot value (or `null`) should be returned.
- 在页面render之前调用，此时的props 和 state 已更新
- 常用于获取render之前的DOM状态。例如，消息滚动条，我们可以知道更新前消息DOM的高度，通过使用这个高度，进行对滚动窗口的一个锁定。

### componentDidUpdate() - Updating

`componentDidUpdate(prevProps, prevState, snapshot)`

- prevProps 和 prevState 是 update 之前的 props 和 state，React自己帮我们传进这个方法里的
- If your component implements the `getSnapshotBeforeUpdate()` lifecycle (which is rare), the value it returns will be passed as a third “snapshot” parameter to `componentDidUpdate()`. Otherwise this parameter will be `undefined`.
- 每次UI发生更新的时候都会被调用。组件会在外部属性或内部state变化的时候进行从新渲染，进行一个整体的更新，所以这个更新是十分频繁的，通过这个方法我们可以进行对每次更新的捕获，去判断是否需要进行而外的操作（例如对UI的操作）。
- 可以理解的是，每次React完成对DOM的更新后，我们可以在componentDidUpdate()中做一些我们想要做的事情，例如对DOM的一些操作（**因为还在commit阶段的最后部分，commit还尚未完成**）。然后DOM又会对我们的修改进行最后的渲染。

    ```jsx
    getSnapshotBeforeUpdate() {
    	return this.rootNode.scrollHeight;
    }

    componentDidUpdate(prevProps, prevState, prevScrollHeight) {
    	// get the current value of scroll top
      const scrollTop = this.rootNode.scrollTop;
      if (scrollTop < 5) return;

    	// update the current scroll top based on the difference 
    	// between the prev height and the current height.
    	// Then, react will take this new scrollTop to update DOM
      this.rootNode.scrollTop =
        scrollTop + (this.rootNode.scrollHeight - prevScrollHeight);
    }
    ```

[在React的componentDidUpdate阶段，浏览器的渲染工作并没有结束_阿超的博客-CSDN博客](https://blog.csdn.net/huangpin815/article/details/80023480)

### componentDidMount() - Mounting

- UI首次渲染完成后调用，且只执行一次，常用于获取外部资源。
- We would like to load the base components first, then we try to fetch our data from the outside source (like APIs), because we don't know how long will it take and we cannot make sure it as well. When we successfully fetch data, then we update our components.
- For example, we want to set up a timer whenever the Clock is rendered to the DOM for the first time. This is called "mounting" in React.
- The method that we use for mounting is `componentDidMount() {}`
- An example snippet:

    Every second the browser calls the `tick()` method. Inside it, the `Clock` component schedules a UI update by calling `setState()` with an object containing the current time. Thanks to the `setState()` call, React knows the state has changed, and calls the `render()` method again to learn what should be on the screen. This time, `this.state.date` in the `render()` method will be different, and so the render output will include the updated time. React updates the DOM accordingly.

### componentWillUnmount() - Unmounting

- We can do something and remove anything that might cause a memory leak which just means that there's some leftover code that the computer doesn't need to be aware of that.
- 组件移除时被调用，常用于资源释放。
- 例如：在使用firebase auth时，我们需要在组件关闭时取消对 user auth 的监听。
- We also want to clear the timer whenever the DOM produced by the Clock is removed. This is called "unmounting" in React.
- The method that we use for mounting is `componentWillUnmount() {}`

## Lists & Passing State with Props

- Simply defining a list by `[]`, and we use `map()` to parse the list.

    **Main diffience between list and array: the elements of a list can be in different data types. The elements of an array must be in the same data type.**

    ```jsx
    state = {
    	users: [
    		user1,
    		user2,
    		user3
    	]
    }
    ```

### Lists

Declared a separate `listItems` variable and included it in JSX:

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

in-line `map()` in JSX:

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

"Sometimes this results in clearer code, but this style can also be abused. Like in JavaScript, it is up to you to decide whether it is worth extracting a variable for readability. Keep in mind that if the `map()` body is too nested, it might be a good time to extract a component."

### Defining 'key' for Each Child Element

- Each child in a list should have a unique "key" prop.
    - It helps React to **only update and render the changed child element**, instead of rendering all child elements.
- Output a `div` for each user by returning its JSX.

    ```jsx
    render() {
      return (
        <div>
            {this.state.users.map(user => (
    	        <div **key={user.id}**>{user.login}</div>
            ))}
        </div>
      )
    }
    ```

- Output a user item component
    - `user` is passed in to `UserItem` as a **prop**
        - `UserItem` doesn't have any state. In the `UserItem`, it only performs the destructuring of `user` which is passed in.

    ```jsx
    render() {
      return (
        <div>
          {this.state.users.map(user => (
            **<UserItem key={user.id} user={user} />**
          ))}
        </div>
      )
    }
    ```

## HTTP Requests & Updating State

### Render() - Lifecycle Method

- The `render()` method is **required**, and is the method that actual outputs HTML to the DOM.

### componentDidMount() - Lifecycle Method

- The `componentDidMount()` method is called after the component is rendered.
- This is where you run statements that requires that the component is already placed in the DOM.
- `componentDidMount()` is the place where you can make an request in HTTP requests.

## Environment Variables - global variable

### Storing in the .env.local file

- Create the .env.local under the project folder
- The variable name in this env local file should be all in **uppercase**

    `REACT_APP_GITHUB_CLIENT_ID='90f07e44cbb0207ccff8'`

- We don't need to import this file, we can get the value by using process.env.YOURVARIABLENAME

    `process.env.REACT_APP_GITHUB_CLIENT_ID`

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

# Forms

- In React, it is better to define a seperate function to handle the submit behavior and the function should have the access of user input.
- Because we want to directly control the form behavior instead of using the default behavior of HTML form. Then, we can do anything that we want with the newest user input data.

## Controlled Components

- React component renders and controls the change of the form when user input is in. An input form element's value is controlled by React.

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%208.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%208.png)

---

# Events, Passing Props, React Router & More

## Events & Search Component

### Handling Events

- React **events** are named using **camelCase**, rather than lowercase on DOM elements.
- With JSX you **pass a function** as the event handler, rather than a string on DOM elements.
    - For example, in React

        ```html
        <button **onClick**=**{activateLasers}**>
        	Activate Lasers
        </button>
        ```

    - the HTML

        ```html
        <button onclick="activateLasers()">
        	Activate Lasers
        </button>
        ```

- In React, in order to prevent default behavior, we must call `preventDefault` explicitly.

    该方法将通知 Web 浏览器不要执行与事件关联的默认动作（如果存在这样的动作）。例如，如果 type 属性是 “submit”，在事件传播的任意阶段可以调用任意的事件句柄，通过调用该方法，可以阻止提交表单。注意，如果 Event 对象的 cancelable 属性是 fasle，那么就没有默认动作，或者不能阻止默认动作。无论哪种情况，调用该方法都没有作用。

    ```jsx
    function ActionLink() {
      function handleClick(e) {
        **e.preventDefault();**
        console.log('The link was clicked.');
      }

      return (
        **<a href="#" onClick={handleClick}>**
          Click me
        </a>
      );
    }
    ```

### * Avoid 'this' error when handling events

```jsx
import ReactDOM from 'react-dom';
import './styles.css';

class App extends React.Component {
	constructor() {
		super();
		this.handleClick2 = this.handleClick1.bind(this);
		// will work for click 2 to point this -> App object
		// this.handleClick1 = this.handleClick1.bind(this); 
	}

	// 直接写了一个normal function，this 没有上下文
	handleClick1() {
		console.log(this);
	}

	// automatically binds this to the place where this arrow function was defined
	// in the first place, and the context of this is the App component
	handleClick3 = () => console.log(this);

	render() {
		return (
			<div>
				<button onClick={this.handleClick1()}> click 1 </button>
				<button onClick={this.handleClick1}> click 2 </button>
				<button onClick={this.handleClick2}> click 3 </button>
				<button onClick={this.handleClick3}> click 4 </button>
			</div>
		);
	}
}
// 1 => App object #Fired immediately
// 2 => window object
// 3 => App object
// 4 => App object
		
```

- For the lifecycle methods in React, `this` is already set/bound when we called `super()` in the constructor. So those `this` represent the App class.
- click 2's `this` points to `handleClick1()`in the App class, but it doesn't provide the context for this class method. Because by default, JS doesn't set the scope of `this` on functions. It actually decides the execution context when the function or class method gets called. Like `this.handleClick1()` works in click 1.
- So `this` inside `handleClick1()` is not determined until an object calls it. When the button is clicked, `window` object will call this call-back function if there is no any specific declaration.

**Conclusion**

- **Using arrow function, the context of `this` is defined when the arrow function itself is defined. So when the App class is constructed, it sees that there is a method `handleClick3()` points to an arrow function and `this` in the arrow function will be automatically bound to the place where this function was defined and the context of this function will be the App component. So `this` points to the App component now.**

### Search

In the input tag:

- `onChange` monitors if the user input is changing
- `value` gives the default value
- `name` gives the input tage a unique name to identify it
    - we can access the value of the `name` attribute:

        `e.target.name`

```jsx
export class Search extends Component {
  state = {
    text: ''
  }

  onChange = e => {
    this.setState({ text: e.target.value })
  }

	onSubmit = e => {
    e.preventDefault();
    console.log(this.state.text);
  }

  render() {
    return (
      <div>
        <form onSubmit={this.onSubmit} className='form'>
          <input 
							type='text' 
							name='text' 
							placeholder='Search Users...' 
							value={this.state.text} 
							onChange={this.onChange} 
						/>
          <input 
							type='submit' 
							value='Search' 
							className='brn btn-dark btn-block' 
						/>
        </form>
      </div>
    );
  }
}

export default Search;
```

## SVG in React

### ReactComponent

- Because .svg is not a js file, so we need to use special syntax to import it.
- `import { ReactComponent as Logo } from 'yourSVG.svg';`
    - importing in this SVG as the react component keyword, but setting it to the logo keyword.
- Use it like the normal component
    - `<Logo className='logo' />`

### svg-sprite-loader

- We can add svg by using this **loader**. However, when we need to define the loader manually in the application, we need to `eject` react application to get and edit webpack.config.

    `yarn eject`

- Install svg-sprite-loader

    `yarn add --dev svg-sprite-loader`

- Add this loader into webpack.config.js

    Add into ***module.exports → modules → rules → oneOf***

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%209.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%209.png)

- Import SVG

    svg-sprite-loader will create a svg tag which contains all imported svgs as symbol tags. Then, we can use #id to `use` it.

    - If we don't `console` it, the `import` will be removed from the production version. Due to **TreeShaking**
    - If we don't want to `console` it, but we still want it to be in the production version, we can use `require` instead of `import`.
        - TreeShaking doesn't work on `require`keyword.

    ```jsx
    // import dollar from '../../icons/dollar.svg';
    // console.log(x); // in order to use the loader.

    // OR
    require('../../icons/dollar.svg');

    <svg>
      <use xlinkHref="#dollar" />
    </svg>
    ```

### Avoid Duplicated Import Svg

- import all svg files and require them from a folder

    ```jsx
    // TS doesn't support gobal variable
    let importAll = (requireContext: __WebpackModuleApi.RequireContext) =>
      requireContext.keys().forEach(requireContext);
    try {
      importAll(require.context('../assets/icons', true, /\.svg$/));
    } catch (error) {
      console.log(error);
    }
    ```

- In order to fix the problem that TS doesn't support global variable, we just need to install `@types/webpack-env`

    `yarn add --dev @types/webpack-env`

# React Router

[React Router: Declarative Routing for React](https://reacttraining.com/react-router/web/guides/quick-start)

## Installation of React Router

`npm i react-router-dom --save` or `yarn add react-router-dom`

If we are using `.ts/.tsx`, we need to install its `ts` dependency as well.

`yarn add --dev @types/react-router-dom`

## BrowserRouter - Enable Routing within Sub-components

- Import package

    `import { BrowserRouter } from 'react-router-dom';`

- `BrowserRouter` is a component that we're actually going to **wrap around our application** and it gives our application that's sitting between this component **all of the functionality of routing**.

    ```jsx
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import { BrowserRouter } from 'react-router-dom';

    ReactDOM.render(
      <BrowserRouter>
        <React.StrictMode>
          <App />
        </React.StrictMode>
      </BrowserRouter>,
      document.getElementById('root')
    );
    ```

- It is like saying 'oh, now you can implement the router in your application now'

## Route

`<Route exact path component />`

- **`exact`**: `true` or `false`, it determines that the path should be exactly matched, then the component will be shown.
    - If we are not explicitly using it (by default), then it is a `false`.
    - If we just write `exact`, then it is a `true`.
    - If we use it with '`=`', then it depends on the statement after '`=`'
- `path`: string value which determines the path for this component.
    - '/' → base url, like http://localhost:3000/
    - 'hats' → like http://localhost:3000/hats
- `component`: specify which component that we are going to route here.
    - `component={HomePage}`

## Switch

`<Switch> </Switch>`

- `Switch` component wraps the `Route` components within it
- `Switch` does when the moment that an `Route` inside of `Switch` is found **a match between its `path` and the URL**, `Switch` does not render anything else but that route's component. It means `Switch` will make **the first matched** `Route` rendered, then even though there might be other components matched, it will stop the rendering.
    - `<Switch>` will search the Routes in order.
    - For example, if we does not specificaly have `exact` for the routes:

        ```jsx
        import { Route, Switch } from "react-router-dom";

        let routes = (
          <Switch>
            <Route path="/"> // without specifying exact
              <Home />
            </Route>
            <Route path="/hats">
              <About />
            </Route>
            <Route path="/:user">
              <User />
            </Route>
            <Route>
              <NoMatch />
            </Route>
          </Switch>
        );
        ```

        and user types localhost:3000/hats, **without** `Switch`, it will render localhost:3000 which is `Home` component first, then after that `locahost:3000/hats` (`Hats` component) will be rendered as well. 

        However, **with** `Switch`, it will stop at rendering `Home` component, **because `Switch` found that there is a matching `Route` for `Home` within it,** and will not render the `Hats` component. 

    - This is also **useful for animated transitions** since the matched <Route> is rendered in the same position as the previous one.

        ```jsx
        let routes = (
          <Fade>
            <Switch>
              {/* there will only ever be one child here */}
              <Route />
              <Route />
            </Switch>
          </Fade>
        );

        let routes = (
          <Fade>
            {/* there will always be two children here,
                one might render null though, making transitions
                a bit more cumbersome to work out */}
            <Route />
            <Route />
          </Fade>
        );
        ```

## React Route DOM - dynamic routing

When we are using React Route, it will pass some props into the component. The common cases are `history`, `location` and `match`. 

- They are all stored in `props` parameter for each component.
- Only the component can access this `props`, for example we define `Route` for HomePage component in App component, only in HomePage component, the `props` can be accessed, but the child component in HomePage will not be able to access it unless **we manually pass it** down. (Not a good practice, we should use `withRouter` to perform this task.
- Using `match` could allow us to build a nested route structure.
- `props.match` - there are four properties in `match` object: `isExact`, `params`,  `path` and `url`
    - `path` - it is the patterns matched the strings that we set in `path` property in the `route`
    - `url` - it is the url for that component (for example component A), even though Component A is displayed when Component B's path gets called ( like the exact keyword example), it will still only display the url only for the component A. (For example, /HomePage only for HomePage component, event though we typed /HomePage/About and HomePage and About components both displayed).
        - So it can dynamically use the url like `props.match.url` - So now, no matter how the path changes for TopicList component, the links of TopicList doesn't care about what they are, they can just focus on the topicId.

            ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2010.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2010.png)

            ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2011.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2011.png)

    - `isExact` - Property becomes `true` only when the entire URL(the user input in the address bar) matches the pattern.
    - `params` - Storing the values that we passed in
        - `:topicId` url parameter, we can access to it by using `props.match.params.topicId` inside the TopicDetail component.

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2012.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2012.png)

    ```jsx
    function App() {
    	return (
    		<div>
    			<Route exact path='/' component={HomePage} />
    			<Route exact path='/topics' component={TopicsList} />
    			<Route exact path='/topics/:topicId' component={TopicDetail} />
    		</div>
    	);
    }
    ```

- `props.location` - we can use it to get the entire url that user passed in.
    - By using `pathname`

    ![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2013.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2013.png)

## Navigation with React Router DOM - history and Link

**There are two ways to navigate in react router DOM between our pages.**

- Using `history` props
    - `<button onClick={() => props.history.push('/topics')}> Topics </button>`
- Using `Link` tag

### Link Component - The tag

- The `link` component is a special component that react route dom gives us to dynamically pass the property `to` and the strings of where we want this link to take us to.
- React is a single page application which means that it's not actually re-directing us and rebuilding the entire application every time the URL chages. It is just highjacking the URL's position to determine with JS what part of the DOM should be mount or remount.

    ```jsx
    import { Link } from 'react-router-dom';

    // it displays a <a> tag like link on the webpage
    <Link to='/topics'> Topics</Link>
    ```

### NavLink Component - Enable active with Link.

- If we want to know which link is being selected and we want to do something like `active class`, we can use `NavLink` component instead of `Link` component

    ```jsx
    import { NavLink } from 'react-router-dom';

    <NavLink to="/statistics" activeClassName="selected">
      <Icon name="chart" />
      Statistics
    </NavLink>
    ```

### withRouter() - Access the Route props (history)

- You can get access to the `history` object’s properties and the **closest** `<Route>`'s `match` via the `withRouter` higher-order component. `withRouter` will pass updated `match`, `location`, and `history` props to the wrapped component whenever it renders.
- withRouter is a higher order component (a function that takes a component as an argument and will returns you a new modified component.
- `import { withRouter } from 'react-router-dom';`

    ```jsx
    import React from 'react';

    import { withRouter } from 'react-router-dom';

    import './MenuItem.scss';

    const MenuItem = ({ title, imageUrl, size, history, linkUrl, match }) => {
      return (
        // navigate to new page /somematchedURL/linkURL
        <div
          className={`${size} menu-item`}
          onClick={() => history.push(`${match.url}${linkUrl}`)}
            {/* the reason why we have a new class for holding the background img, because we want to fix the img box
              * when we hover the img, and the size remains the same. Not wrapping the context div is because we don't
              * want the background gets affect by the content box.
    					*/
            }
            className="background-image"
            style={{
              backgroundImage: `url(${imageUrl})`
            }}
          />
          <div className="content">
            <h1 className="title">{title.toUpperCase()}</h1>
            <span className="subtitle">SHOP NOW</span>
          </div>
        </div>
      );
    };

    // by using withRouter, now we can access the history, location, match props
    // which App passes into HomePage in the Route.
    export default withRouter(MenuItem);
    ```

## Lazy Loading - React Performance for Routing

[React Performance](React%20Performance%20e37eda0610b7428594ec9fc28a5c44c9.md)

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

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2014.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2014.png)

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2015.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2015.png)

## Redux Reducer

- `return` creates a new object in order to trigger React to update the DOM when a new `props` is passed in.
- `currentUser: action.payload`
    - the reducer takes the same key-value data from the `currentState`, and overwrites the `currentUser` field in the `currentState` object with `action.payload` data. After that, the new object becomes the newest `state`.

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

### Using Redux

- Create a folder 'redux' under src folder.
- Create RootReducer
    - RootReducer.js
- Create User Reducer
    - Create UserReducer.js under a new folder in the src folder. (/src/user)
- Create the Store
    - import Store into index.js to allow `Provider` to have the context of store.
- Create Actions
    - These actions return objects.

## Reducer in Redux

- A reducer is actually just a function that gets two properties as its parameters - state and action
    - State will be the current state when the action is fired.
    - when the component is first run or something, there will not be a state there, so we need to set an initial state object

### state

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

### Root Reducer

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

### Middleware

Middleware is like some functions which receive our actions and these functions will perform some tasks with the actions, then pass them to our rootReducer.

Add middleware to our store, so whenever actions get fired or dispatched, we can catch them and display them on the console (using `logger`).

In order to bring these middleware printing works in, we need to use `logger` from `redux-logger`.

Middleware is expected as an array by the store 

### Store

"A store **holds the whole state tree** of the application. The only way to change the state inside it is to dispatch an action on it. A store is **not a class.** It's just an **object** with a few **methods** on it. To create it, pass your root reducing function to `createStore`."

**createStore**

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

## Use Redux in a Component - `connect()`

After the settings above, we can now use `connect()` to get our needed `state` from redux store. 

`connect(mapStateToProps, mapDispatchToProps, mergeProps, options)`

- **NOTE: if we don't need to use any of the function as the parameter, simply pass `null` to `connect()`.**

"The `connect()` function connects a React component to a Redux store. It provides its connected component with the pieces of the data it needs from the store, and the functions it can use to dispatch actions to the store."

`connect` lets us modify the components which need to access to things related to redux.

- **Connect** is a higher-order component, it will return a new modified component.

`connect()` accepts two functions and one component, the first one is the function (`mapStateToProps`) which can access the states **(the state is our root reducer)** as its parameter. The second argument is the component that we want to modify, which will take the returning object from `mapStateToProps` 

### mapStateToProps - 数据绑定

读取store state中的数据

`mapStateToProps?: (state, ownProps?) => Object`

- If `mapStateToProps` takes only one parameter, it will be **called** whenever the **store state changes**, and given the **store state** as the only parameter to **the wrapped component**.
- If `mapStateToProps` takes two parameters (`state, ownProps`), it will be **called** whenever the **store state changes** or when the wrapper component **receives new props** (based on shallow equality comparisons). It will be given **the store state as the first parameter, and the wrapper component's props as the second parameter.**
    - if your component needs the data from its own props to retrieve data from the store. This argument will contain all of the props given to the wrapper component that was generated by connect.

 "The new wrapper component will subscribe to Redux store updates. Any time the store is updated, `mapStateToProps` will be called. The **returning object** of `mapStateToProps` will be merged into the wrapped component's **props**."                                                  

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2016.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2016.png)

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

### mapDispatchToProps - 事件绑定

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

## Reselector - memorising state

Everytime that the part of store is updated, even though the component is not using that updated data, it is still re-rendered. It affects the performance of the application. So we are using reselector to increase the performance.

- For example, in the e-commerce app, when the current login user is changed, the cart component will be re-rendered but none of the cart items is changed.

**Selector will return piece of the state, as we always use the smaller and more specific selectors.**

### InputSelector

- Doesn't need to have `createSelector`

**How to use it:**

`const selectCart = state => state.cart;`

- It selects the partial of the state.

### OutputSelector

- Using InputSelector and createSelector

**How to use it:**

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

[Memoising A Whole Function](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Memoising%20A%20Whole%20Function%20bf6974aede0a43e7a3691cb3a05d4950.md)

### createStructuredSelector() - avoiding passing `state` manually to selectors

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

## Redux Persist - local storage in the browser

### Install redux-persist

`npm install redux-persist --save`

### Configurations

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

### Overview of the Persistence

- When the browser is **reloaded** or **refreshed**, redux will check if **there is an existing value** in the `persist`, then it will fired the `rehydrate` those value back to the `store`.

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2017.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2017.png)

## Redux Compose - Compose Multiple Higher-order Components

If we have a component which needs to be wrapped inside many other components, it will be very hard to read and maintenance.

- We can compose all our higher-order components together as parameters into a function.

### Import compose from 'redux'

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

# Firing Multiple Actions - redux-thunk & redux-saga

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/1.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/1.png)

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

# Redux-Saga

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2018.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2018.png)

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2019.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2019.png)

## Introduction to Saga

`redux-thunk` will allows actions to come in as functions, and if it is a function, the thunk will invoke the function because we have dispatch in the function. Thunk will just pass those new dispatch actions into the reducers one by one. 

We will use `redux-saga` to perform asynchronous events requests like `redux-thunk`. Saga is like a function that **conditionally** runs and the condition that it depends on when it runs - is based on whether or not **a specific action is coming into** the saga middleware.

**Install Saga**

`npm install redux-saga --save` or `yarn add redux-saga`

**Using Saga**

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

## Saga Effects

**import certain effects from saga to do different things with either the store like creating actions or listening for actions.**

`**take, takeEvery, call, put, takeLatest, all**`

- `take` - wait to finish the rest of code until the application listened a certain type.
    - Creates an Effect description that instructs the middleware to **wait for a specified action on the Store**. The Generator is **suspended** until an action that matches `pattern`(action type) is dispatched.
    - The result of `yield take(pattern)` is an action object being dispatched.
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

- `call(fn, args)` - it is the effect inside of the generator function that invokes the method.
    - Creates an Effect description that instructs the middleware to call the function `fn` with `args` as arguments.
- `put` - it is the effect inside of the generator function that **dispatches action**
    - Creates an Effect description that instructs the middleware to **dispatch an action** to the Store. This effect is non-blocking and any errors that are thrown downstream (e.g. in a reducer) will not bubble back into the saga.
    - It is like `dispatch`, but we have to `yield` it and is going to use it like a **dispatch**

        `yield put(fetchCollectionsSuccess(collectionsMap));`

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

### Generator Function

It is similar to async await functions, it **pauses their execution** whenever they see a specific key inside of the function and that that key is called a **yield.** 

**All generator functions must have at least one yield in each.**

**Declare A Generator Function**

`yield` key tells our generator function that it is going to stop the function from here including the line of `yield`, until `next()` gets called.

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

---

# React-Hooks

## Introduction to react-hooks

`react-hooks` is introduced due to the diffficulity of managing those internal state and determining the order in which those lifecylce methods are going to fire and where we should be doing different functionality of our component in what lifecycle method.

**Hooks** allow us to write functional components but still gain access to new functionality that was previously only available in the class components. 

- So we can only use `react-hooks` inside of the functional component.

`react-hooks` only works under react version 16.8 or higher.

## Using react-hooks

### Import react-hooks

**`useState`** - allow us to use State and set State

`import React, { useState } from 'react';`

### useState - internal state

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

### useEffect - performing lifecycles' jobs

`function useEffect(effect: EffectCallback, deps?: DependencyList): void;`

Accepts a function that contains imperative, possibly effectful code.

*@param* `effect` — Imperative function that can return a cleanup function

*@param* `deps` — If present, effect will only activate if the values in the list change.

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

### useReducer - complex state management

We will use this `useReducer` when we need more complex local state management than `useState`.

Using `useReducer` is very similar to what we do in redux.

**Returning from `useReducer`**

- state - the current state
- dispatch

**Passing into `useReducer` to instantiate**

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

### React Hooks Cheat Sheet

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

## Custom Hooks

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

# Context-API

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

# Deploy on Heroku

## Install Heroku Cli

Heroku is like a git repository.

## Create a New App on Heroku

`heroku create yourAppName --buildpack https://github.com/mars/create-react-app-buildpack.git`

- `buildpack`: specific configuration for the build that we want, so that it deploys our React as a static website. It the best and the most efficient way for us to host our React project created with create React app.
- `buildpack`: it will also use the `production` build of our react app for deployment.

## Push our app to Heroku

`git push heroku master`

- `heroku`: it is the repository that our current Heroku project that we just created is connected to the project in our local disk.
- `master`: Push our master branch.

`heroku open`

- open our heroku page.

## Gzipping and Compression

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2020.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2020.png)

![ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2021.png](ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2021.png)

- File sizes after gzip:
    - gzip is actually a compression strategy that the back-end servers do.
    - So that when the back-end servers pass these chunks through HTTPs from the server to the client, it's in a smaller package and it gets unzipped in the actual client browser. At that point, it will be the full size that the actual project is.
- Heroku doesn't offer **Gzipping** right out of the gate.
- Gzipping has to be done manually in the express server.
- It can be done by using Compression library.

### Compression

- Adding compression that express maintains themselves to provide gzipping to our Node server.
- Compression can compress and gzip all of the files and chunks that we end up sending out from our server.

**Install compression library**

`yarn add compression` or `npm install compression`

**Use compression**

```jsx
**File: server.js**

const compression = require(compression);
...

// with app.use()
app.use(compression());
// the rest app.use
...
```

# CSS in JS - styled-components

## Style Leaking across Components

In a large JS project, due to CSS sharing one single global namespace, it may happen that a namespace of CSS is reused in different places.

- In order to solve this problem, we can write CSS codes inside JS code. (like, putting CSS in an object of JS code, and use it in the `style` property of the tag) - It does solve the problem, but it won't allow the selectors to be accessed.
    - property's name needs to be in camelCase.
- Therefore, we can introduce a library - styled-components

## styled-components

This library will help to create a new component with a styled HTML element, and it allows props to be passed into.

### Install Library

normal js/jsx file:

`npm install styled-components --save`

`yarn add styled-components`

normal ts/tsx file

`yarn add --dev @types/styled-components` for ts, This package contains type definitions for styled-components

### Import Library

`import styled from 'styled-components';`

### Using Library

- we can specify it in any HTML elements, etc: `div, p...`
- The new created component (like Text) can take `props` and we can do function with it.

    ```jsx
    import styled from 'styled-components';

    ...

    // Text will be a component.
    const Text = styled.div`
    	color: red;
    	font-size: 28px;
    	border: ${
    		({isActive}) => isActive ? '1px solid black' : '3px dotted green'
    	}
    `;

    ...

    // Passing props into Text component
    <Text isActive={true}> Hello World </Text>
    ```

- We can use `styled` called like a function which will pass a Component that we want to wrap it. After that, we will have a styled `Link` component.
    - So, `LogoContainer` component will act as a `Link` component except **being styled**.

        ```jsx
        // Link is a component
        export const LogoContainer = styled(Link)`

        `;
        ```

- we can also use `import { css } from 'styled-components';` which can help us to write a block of CSS that we can pass in and render as CSS inside of any of styled component.
- Using `as` to reuse a existing element as  another element.
    - we can pass a String which specifies the name of some normal HTML tags' name (like div, p)
    - we can also pass the name of any regular components by `as={}`.

```jsx
// using css function
{currentUser ? (
  <OptionDiv onClick={() => auth.signOut()}>SIGN OUT</OptionDiv>
) : (
  <OptionLink to="/sign-in">SIGN IN</OptionLink>
)}
```

```jsx
// for example, besides using css, we can do in this way
{currentUser ? (
  <OptionLink **as='div'** onClick={() => auth.signOut()}>SIGN OUT</OptionLink>
) : (
  <OptionLink to="/sign-in">SIGN IN</OptionLink>
)}
```

New `styles` sheet:

```jsx
import styled, { css } from 'styled-components';
import { Link } from 'react-router-dom';

// Using css 
const OptionContainerStyles = css`
  padding: 10px 15px;
  cursor: pointer;
`;

export const HeaderContainer = styled.div`
  height: 70px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-bottom: 25px;
`;

export const LogoContainer = styled(Link)`
  height: 100%;
  width: 70px;
  padding: 25px;
`;

export const OptionsContainer = styled.div`
  width: 50%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-end;
`;

// reuse the same css code for different elements by using css from styled-components
export const OptionLink = styled(Link)`
  ${OptionContainerStyles}
`;

export const OptionDiv = styled.div`
  ${OptionContainerStyles}
`;
```

Before:

```jsx
const Header = ({ currentUser, hidden }) => (
  <div className="header">
    <Link className="logo-container" to="/">
      <Logo className="logo" />
    </Link>
    <div className="options">
      <Link className="option" to="/shop">
        SHOP
      </Link>
      <Link className="option" to="/contact">
        CONTACT
      </Link>
      {currentUser ? (
        <div className="option" onClick={() => auth.signOut()}>
          SIGN OUT
        </div>
      ) : (
        <Link className="option" to="/sign-in">
          SIGN IN
        </Link>
      )}
      <CartIcon />
    </div>
    {hidden ? null : <CartDropdown />}
  </div>
);
```

After:

```jsx
const Header = ({ currentUser, hidden }) => (
  <HeaderContainer>
    <LogoContainer to="/">
      <Logo className="logo" />
    </LogoContainer>
    <OptionsContainer>
      <OptionLink to="/shop">SHOP</OptionLink>
      <OptionLink to="/contact">CONTACT</OptionLink>
      {currentUser ? (
        <OptionDiv onClick={() => auth.signOut()}>SIGN OUT</OptionDiv>
      ) : (
        <OptionLink to="/sign-in">SIGN IN</OptionLink>
      )}
      <CartIcon />
    </OptionsContainer>
    {hidden ? null : <CartDropdown />}
  </HeaderContainer>
);
```