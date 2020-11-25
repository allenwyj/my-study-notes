# CSS

# CSS Rules

- Normally, CSS follows the cascade rule.
- Determining what selectors win out in the cascade depends on the following three rules.

## Specificity

- The more specific something is the more likely it will win out.

    [Specificity Calculator](https://specificity.keegan.st/)

    ![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled.png)

## Importance

- Using `!important`

## Source Order

- The import order of different CSS stylesheets. The later import will replace its previous stylesheet(s).

# CSS Units

[Absolute Lengths](CSS%20ad773fcf4b8f4eae893912b205730b6c/Absolute%20Lengths%2036812a026b94423c82f0da3e1074abce.csv)

- The absolute length units are fixed and a length expressed in any of these will appear as exactly that size.
- Absolute length units are not recommended for use on screen, because screen sizes vary so much. However, they can be used if the output medium is known, such as for print layout.

[Relative Lengths](CSS%20ad773fcf4b8f4eae893912b205730b6c/Relative%20Lengths%20fa9db97f350f4f2db0e62bb239fff955.csv)

- Relative length units specify a length relative to another length property. Relative length units scale better between different rendering medium.
- Viewport = the browser window size. If the viewport is 50cm wide, 1vw = 0.5cm.

[CSS-Tricks](https://css-tricks.com/)

# Selectors

## * Sign

- Specifying to all elements.
- Normally, put this selector rule on the top. So the rest of CSS rules can replace the properties of this selector.

## . Sign

- Specifying to a **class name** of a HTML element

```html
<div class="container">
	...
</div>

<style>
	.container {
		...
	}
</style>
```

## # Sign

- Specifying to an **id** of a HTML element.

```html
<div id="container">
	...
</div>

<style>
	#container {
		...
	}
</style>
```

## @mixin

`@mixin className` 

- define a class and reuse it in somewhere else by using `@include className();`

## Using Commas(,) to Separate Selectors

`th, td, p.red, div#firstred { color: red; }`

- The comma character basically acts as the word **"or"** inside the selector.

## Space - all children

- the part of the selector that occurs right of the space **is within (a child of)** the part of the selector to the left. e.g. target all p tags within container div, including all p elements inside any sub-element.
- `.container div {...}`
    - select all div elements that are the child of any element with a class name of "container".
- `#header .callout {...}`
    - Select all elements with the class name callout that are decendents of the element with an ID of header.

## > Sign - only the direct children

- It will target elements which are **DIRECT** children of a particular element.
- The below codes:

    It will target all P tags which are direct children of container div, not children of child div (Not Second).

```html
<div id="container">
	<p>First</p>
	<div>
		<p>Second</p>
	</div>
	<p>Third</p>
	<p>Fourth</p>
</div>

<style>
	**div#container > p {
	  border: 1px solid black;
	}**
</style>
```

## + Sign

- Select an element that directly follows another element
- It is Adjacent sibling combinator.
- It combines two **sequences** of simple selectors having the **same parent** and the second one must come **IMMEDIATELY** after the first.
    - And **only select** the first matched element.
    - Third and 6 are the selected element.

```html
<div id="container">
	<p>First</p>
	<div>
		<p>Second</p>
	</div>
	**<p>Third</p>**
	<p>Fourth</p>
	<div>
		<p>5</p>
	</div>
	**<p>6</p>**
	<div>
		<p>7</p>
	</div>
	<span>Hi</span>
	<p>8</p>
</div>

<style>
	**div + p {
	  color: green;
	}**
</style>
```

## ~ Sign

- It is general sibling combinator and similar to Adjacent sibling combinator.
- It will select **all** elements that are after the first selector and are the second selectors.

```html
<div id="container">
	<p>First</p>
	<div>
		<p>Second</p>
	</div>
	**<p>Third</p>**
	**<p>Fourth</p>**
</div>

<style>
	**div ~ p {
	  color: green;
	}**
</style>
```

## Tag Qualifying - having multiple selectors together

- `div.container`
    - select any div element that has a class name of "contianer".
- `#header.callout {}`
    - Select the element which has an ID of header and also a class name of callout.
- `.snippet#header.code.red { color: red; }`
    - Target an element that has all of multiple classes.

## !important

`!important`

- Breaking the cacesading rule, and specifying this CSS rule to all of the matched cases, no matter what they have been defined in the later CSS codes.

## Input

CSS `:focus` Selector

- Select and style an input field when it gets focus:

## Using ：colon

### :hover

### :first-child

- 若`:`前面的 element 是所在容器的第一个 element，选中。
- 可应用到所有符合的情况

### :last-child

- 若`:`前面的 element 是所在容器的最后一个 element，选中。
- 可应用到所有符合的情况

### Examples

Pseudo-classes (`:`) allow you to style the different states of an element e.g. when you hover over it, when it is disabled, when it is the first child of its parent, etc.

```css
/* 
	Select and style every <i> element of every <p> element, 
	where the <p> element is the first child of its parent: 
	p标签得是其所在容器中的第一个子元素，可以有多个符合条件的p标签
	符合条件的 p标签中所有的 i标签 将应用括号内的CSS样式。
*/
p:first-child i {
  background: yellow;
}

/*
	Select and style the first child element of all elements if it is li element:
*/
li:first-child {
  background: yellow;
}

/*
	Select and style the first child element of every <ul> element:
*/
ul > :first-child {
  background: yellow;
}

/*
	* means the first-child can be any of all tags, same as the above one.
*/
ul > *:first-child {
	margin-bottom: 50px;
}
```

Pseudo-elements (`::`) allow you to style different parts of an element e.g. the first line, the first letter, inserting content before or after, etc.

Originally they all used a single colon, but CSS3 introduced the double colon to separate the two.

### Centering an Element

- instead of using negative margins, we can use negative translate() transforms.

```css
position: absolute;
left: 50%;
transform: translate(-50%);
```

## width

```css
width: 22%
width: 22vw
```

`%`: the size of the containing div's width

`vw`: the width of the actual window size

# Text and Fonts

`text-decoration`

- underline
- line-through

`text-transform`

- uppercase
- lowercase

`text-height`

- Define the height of the text. By default, it's 20% bigger than the font size.

`font-size`

- Define the size of the font.

`font-style`

- Define the style of the font, like bold, italic.

`font-weight`

- The weight of the font

`font-family`

- Define the type of font.
- If the type contains multiple words, use double quotes.
- We can add multiple font types and that will allow browser to select if it is available in the user's computer based on the order of the types.

## Import Font Family

### Embed Font

- Add a `<link>` tag into the `<head>` section

### Specify in CSS

- Use CSS rules to specify the font family

# Display Property

[display](CSS%20ad773fcf4b8f4eae893912b205730b6c/display%20190cf841a7024eee8f5a3c6f9134c71d.csv)

---

## display: block

- Displays an element as a block element (like <p>).
- It starts on a new line, and takes up the whole width.

## display: inline-block

- Displays an element as an inline-level block container.
- The element itself is formatted as an inline element, but you can apply height and width values

## display: flex

[A Complete Guide to Flexbox | CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

### Flex Box

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%201.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%201.png)

---

### Move Item to the Center

```css
.container {
    display: flex;
    align-items: center;
    justify-content: center;
}
```

### flex-direction

The `flex-direction` property defines in which direction the container wants to stack the flex items.

- The `column` value stacks the flex items vertically (from top to bottom)

    ```css
    .flex-container {
      display: flex;
      flex-direction: column;
    }
    ```

- The `column-reverse` value stacks the flex items vertically (but from bottom to top)

    ```css
    .flex-container {
      display: flex;
      flex-direction: column-reverse;
    }
    ```

- The `row` value stacks the flex items horizontally (from left to right)

    ```css
    .flex-container {
      display: flex;
      flex-direction: row;
    }
    ```

- The `row-reverse` value stacks the flex items horiztonally (but from right to left)

    ```css
    .flex-container {
      display: flex;
      flex-direction: row-reverse;
    }
    ```

### flex-wrap

By default, flex items will all try to fit onto one line. You can change that and allow the items to wrap as needed with this property.

```css
.container {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

### justify-content

- 以`flex-direction`的方向为x轴的情况下，在x轴上调整所有flex items的位置，以安排extra free space

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%202.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%202.png)

### align-content

- 以`flex-direction`的方向为x轴的情况下，在y轴上调整所有flex items的位置。将cross-axis的多余空位进行调整。只能作用于multi-line flexible container的情况下

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%203.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%203.png)

### justify-items

- 以`flex-direction`的方向为x轴的情况下，在x轴上调整所有flex items**内部子元素在flex item中**的位置

### align-items

- 以`flex-direction`的方向为x轴的情况下，在y轴上调整所有flex items**内部子元素在flex item中**的位置

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%204.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%204.png)

---

## display: grid

- `container` is the grid container which applies `display:grid`.
- The children of the grid container is grid items, `item` elements are grid items, but `sub-item` isn't.

```html
Grid Container
<div class="container">
  <div class="item item-1"> </div>
  <div class="item item-2"> </div>
  <div class="item item-3"> </div>
</div>

Grid Item
<div class="container">
  <div class="item"> </div>
  <div class="item">
    <p class="sub-item"> </p>
  </div>
  <div class="item"> </div>
</div>
```

[A Complete Guide to Grid | CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-template-columns-rows)

[Auto-Sizing Columns in CSS Grid: `auto-fill` vs `auto-fit` | CSS-Tricks](https://css-tricks.com/auto-sizing-columns-css-grid-auto-fill-vs-auto-fit/)

### The Most Powerful Lines in Grid

- Fluid width columns that break into more or less columns as space is available, with no media queries!
- 只需要告诉grid我们需要最小值和最大值一个grid-item，剩下一行能放多少就交给grid自己去计算。

```jsx
import styled from 'styled-components';

// This will fill up the number of columns automatically based on the available
// pixels against the min size of a column.
export const CoinGridContainer = styled.div`
  display: grid;
  grid-template-columns: **repeat(auto-fill, minmax(130px, 1fr))**;
  grid-gap: 15px;
  margin-top: 40px;
`;
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%205.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%205.png)

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  /* This is better for small screens, once min() is better supported */
  /* grid-template-columns: repeat(auto-fill, minmax(min(200px, 100%), 1fr)); */
  grid-gap: 1rem;
  /* This is the standardized property now, but has slightly less support */
  /* gap: 1rem */
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%206.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%206.png)

---

### grid-template-area

- Defines a grid template by referencing the names of the grid areas which are specified with the `grid-area` property.
- **Repeating** the name of a grid area causes the content to span those cells.
- A period (.) signifies an empty cell.
- The syntax itself provides a visualization of the structure of the grid.

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%207.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%207.png)

```css
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

### item position - start and end

```css
.item {
	grid-column: 1/3; /* Column: Start at line 1 and end up at line 3 */
	grid-row: 1/3; /* Row: Start at line 1 and end up at line 3 */
}
```

### **column-gap row-gap**

- Specifies the size of the grid lines.
- The syntax was `grid-column-gap` and `grid-row-gap`

```css
.container {
  /* standard */
  column-gap: <line-size>;
  row-gap: <line-size>;

  /* old */
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}

.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  column-gap: 10px;
  row-gap: 15px;
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%208.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%208.png)

### gap grid-gap

- A shorthand for `row-gap` and `column-gap`. Setting both values with one single statement.
- If no column-gap specified, then it’s set to the same value as row-gap.

```css
.container {
  /* standard */
  gap: <grid-row-gap> <grid-column-gap>;

  /* old */
  grid-gap: <grid-row-gap> <grid-column-gap>;
}

.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  gap: 15px 10px;
}
```

### justify-self - grid item

- Aligns a grid item inside a cell along the inline axis. This value applies to a grid item inside a single cell.

```css
.item {
  justify-self: start | end | center | stretch;
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%209.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%209.png)

### align-self - grid item

- Aligns a grid item inside a cell along the block (column) axis. This value applies to the content inside a single grid item.

```css
.item {
  align-self: start | end | center | stretch;
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2010.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2010.png)

### place-self - grid item

- `place-self` sets both the `align-self` and `justify-self` properties in a single declaration.
- If the second value is omitted, the first value is assigned to both properties.

```css
.item {
	place-self: <align-slef> / <justify-self>
}
```

### justify-items - all grid items in grid container

- The CSS `justify-items` property defines the default `justify-self` for all items of the box, giving them all a default way of justifying each box along the appropriate axis.

```css
.container {
  justify-items: start | end | center | stretch;
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2011.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2011.png)

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2012.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2012.png)

### align-items - all grid items in grid container

- Aligns grid items along the block (column) axis. This value applies to all grid items inside the container.

```css
.container {
  align-items: start | end | center | stretch;
}
```

![CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2013.png](CSS%20ad773fcf4b8f4eae893912b205730b6c/Untitled%2013.png)

### place-items - all grid items in grid container

- `place-items` sets both the `align-items` and `justify-items` properties in a single declaration.
- If the second value is omitted, the first value is assigned to both properties.

```css
.container {
	place-items: <align-items> / <justify-items>
}
```

### justify-content - all grid items 比 grid container 小

- 所有 grid items 所占面积要比 grid container 宽度要窄，在**横坐标**上调整所有 grid items 的位置。
- This property aligns the grid along the inline (row) axis (as opposed to align-content which aligns the grid along the block (column) axis).

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
```

### align-content

- 所有 grid items 所占面积要比 grid container 高度要矮，在**纵坐标**上调整所有 grid items 的位置。
- This property aligns the grid along the block (column) axis (as opposed to justify-content which aligns the grid along the inline (row) axis).

```css
.container {
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
```

### place-content

- `place-content` sets both the `align-content` and `justify-content` properties in a single declaration.
- If the second value is omitted, the first value is assigned to both properties.

```css
.container {
  place-content: <align-content> / <justify-content>
}
```

# Media Query

- `@media only screen` - For screen.

    ```css
    @media only screen and (max-width: 600px) {
        
    }

    @media screen and (max-width: 500px) {
      > input {
        height: 30px;
      }
    }
    ```

- `@media only print` - For printing layout