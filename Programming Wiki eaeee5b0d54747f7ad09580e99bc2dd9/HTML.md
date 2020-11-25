# HTML

# Link CSS File to HTML

- Inside the `head` tag, we can use `link` to link the css file to the current html file.

    `<link rel="stylesheet" type="text/css" href="file-location/style.css">`

# Self-closing Tags

## br

`<br>` : line break.

## img

`<img src="url.jpg" alt="Hi" width="42" height="42" >`

**Note: It doesn't need `/` for the closing bracket. In React, we are actually writing JSX not HTML, so we need `<img ... />` for it.**

# Anchor Tag

`<a href="www.google.com">Click me </a>` or you can wrap any tag within it.

# Container Tag

## div

`<div></div>`

- Block container

## span

`<span></span>`

- in-line container
- we can use it to only apply CSS to a specific line.

## HTML data-* Attribute

- The data-* attribute is used to store custom data private to the page or application.
- The data-* attribute gives us the ability to embed custom data attributes on all HTML elements.

### <nav:> The Navigation Section element

- The HTML `<nav>` element represents a section of a page whose purpose is to provide navigation links, either within the current document or to other documents. Common examples of navigation sections are menus, tables of contents, and indexes.