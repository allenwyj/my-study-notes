# styled-component

---

# CSS in JS - styled-components

## Style Leaking across Components

In a large JS project, due to CSS sharing one single global namespace, it may happen that a namespace of CSS is reused in different places.

- In order to solve this problem, we can write CSS codes inside JS code. (like, putting CSS in an object of JS code, and use it in the `style` property of the tag) - It does solve the problem, but it won't allow the selectors to be accessed.
    - property's name needs to be in camelCase.
- Therefore, we can introduce a library - styled-components

# styled-components

This library will help to create a new component with a styled HTML element, and it allows props to be passed into.

## Install Library

normal js/jsx file:

`npm install styled-components --save`

`yarn add styled-components`

normal ts/tsx file

`yarn add --dev @types/styled-components` for ts, This package contains type definitions for styled-components

## Import Library

`import styled from 'styled-components';`

## Using Library

### Specifying Type

- we can specify it in any HTML elements, etc: `div, p...`

### Passing Props into styled-component

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