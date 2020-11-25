# Sass

# Import package to use Sass

In order to use Sass, we need to install `node-sass` or `dart-sass`.

`npm install node-sass` or `yarn add node-sass`

If we want to install `dart-sass`, we cannot directly install it, we need to use the below command to install.

`yarn add node-sass@npm:sass`

**dart-sass** is better and faster.

# Import Another File into scss

`@import "folderName/fileName";`

![Sass%202cfd87b3273e426a9a497858089f27a8/Untitled.png](Sass%202cfd87b3273e426a9a497858089f27a8/Untitled.png)

# Sass Nested Rules

## & Symbol

- using `&` is to define the style for the element which **has both classes**.
- In Sass, we can **nest the child elements** inside the parent element.

    ```scss
    .parentClassName {
    	// only for child element
    	.customClassName {

    	}

      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }

      li {
        display: inline-block;
      }

      a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
      }
    	
    	// child element
    	& .childName {
    		...
    	}
    	

    	// has both class names
    	&.bothClassNames {
    		...
    	}
    }
    ```

- In CSS, the rules are defined one by one

    ```css
    nav ul {
      margin: 0;
      padding: 0;
      list-style: none;
    }
    nav li {
      display: inline-block;
    }
    nav a {
      display: block;
      padding: 6px 12px;
      text-decoration: none;
    }

    nav .childName {
    	...
    }

    nav.bothClassNames {
    	...
    }
    ```

## 4 Columns Grid

![Sass%202cfd87b3273e426a9a497858089f27a8/Untitled%201.png](Sass%202cfd87b3273e426a9a497858089f27a8/Untitled%201.png)

```css
.collection-page {
  display: flex;
  flex-direction: column;

  .title {
    font-size: 38px;
    margin: 0 auto 30px;
  }

  .items {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 10px;

    & .collection-item {
      margin-bottom: 30px;
    }
  }
}
```

## Add an underline

![Sass%202cfd87b3273e426a9a497858089f27a8/Untitled%202.png](Sass%202cfd87b3273e426a9a497858089f27a8/Untitled%202.png)

```css
/* 
  Create an underline without giving impact on other elements 
  Using border will take space and stretch the container.
*/
position: relative;
&.selected::after {
  content: '';
  display: block;
  height: 3px;
  background: #333;
  position: absolute;
  bottom: 0;
  width: 100%;
  left: 0;
}
```