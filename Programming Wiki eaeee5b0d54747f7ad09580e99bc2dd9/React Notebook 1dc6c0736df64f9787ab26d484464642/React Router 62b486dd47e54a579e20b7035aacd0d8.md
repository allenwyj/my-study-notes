# React Router

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

### 'exact' keyword

- **`exact`**: `true` or `false`, it determines that the path should be exactly matched, then the component will be shown.
    - If we are not explicitly using it (by default), then it is a `false`.
    - If we just write `exact`, then it is a `true`.
    - If we use it with '`=`', then it depends on the statement after '`=`'

### 'path' keyword

- `path`: string value which determines the path for this component.
    - '/' → base url, like http://localhost:3000/
    - 'hats' → like http://localhost:3000/hats

### 'component' keyword

- `component`: specify which component that we are going to route here.
    - `component={HomePage}`

## Switch

`<Switch> </Switch>`

- `Switch` component wraps the `Route` components within it
- `Switch` will render the assigned component when the moment that:
    1. An `Route` inside of `Switch` is found that there is **a match between this Route's  `path` and the URL in the browser;**
    2. `Switch` will render that route's component. It means `Switch` will make **the first matched** `Route` rendered, 
    3. And then, even though there might be other components matched, it will stop the rendering others.
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

        and user types localhost:3000/hats 

        - **without** `Switch`, it will render localhost:3000/ which is `Home` component first, then after that `locahost:3000/hats` (`Hats` component) will be rendered as well.
        - However, **with** `Switch`, it will stop at rendering `Home` component, **because `Switch` found that there is a matching `Route` for `Home` within it,** and will not render the `Hats` component.
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

# React Route DOM - dynamic routing

When we are using React Route, it will pass some props into the component by default. 

The common cases are `history`, `location` and `match`. 

- They are all stored in `props` parameter on each component.
- Only the component itself can access this `props`,
    - For example, we define `Route` for `HomePage` component in `App` component, only in `HomePage` component, the `props` can be accessed, but the **child component** in `HomePage` will **not** be able to access it unless **we manually pass it** down. (Not a good practice, we should use `withRouter` to perform this task.

## 'match' Object

- Using `match` could allow us to build a nested route structure.
- `props.match` - there are four properties in `match` object: `isExact`, `params`,  `path` and `url`

    ![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2012.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2012.png)

### path

- `path` - it is the **patterns** matched the **strings** that we set in `path` property in the `route`

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

### url

- `url` - it is the url for that component (for example component A), even though Component A is displayed when Component B's path gets called ( like the exact keyword example), it will still only display the url only for the component A. (For example, /HomePage only for HomePage component, event though we typed /HomePage/About and HomePage and About components both displayed).
    - So it can dynamically use the url like `props.match.url` - So now, no matter how the path changes for TopicList component, the links of TopicList doesn't care about what they are, they can just focus on the topicId.

        ![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2010.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2010.png)

        ![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2011.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2011.png)

### isExact

- `isExact` - Property becomes `true` only when the entire URL(the user input in the address bar) matches the pattern.

### params

- `params` - Storing the values that we passed in
    - `:topicId` url parameter, we can access to it by using `props.match.params.topicId` inside the TopicDetail component.

## location

- `props.location` - we can use it to get the entire url that user passed in.
    - By using `pathname`

    ![../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2013.png](../ReactJS%205dbc076fcfc3485585c2cd9ec180d7a0/Untitled%2013.png)

## history and Link - Navigation with React Router DOM

**There are two ways to navigate in react router DOM between our pages.**

- Using `history` props
    - `<button onClick={() => props.history.push('/topics')}> Topics </button>`
- Using `Link` tag

### Link Component - The tag

- The `Link` component is a special component that react route dom gives us to dynamically pass the property `to` and the strings of where we want this link to take us to.
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

[Copy of React Performance](React%20Router%2062b486dd47e54a579e20b7035aacd0d8/Copy%20of%20React%20Performance%201bd6be079a08498bb2889b14cfcbeba1.md)