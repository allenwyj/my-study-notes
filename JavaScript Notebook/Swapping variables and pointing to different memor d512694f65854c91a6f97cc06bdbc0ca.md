# Swapping variables and pointing to different memory location

Created: Oct 12, 2020
Created by: Allen Wu
Tags: JavaScript

It's a common task to swap variables. In JavaScript, there are several different approaches we can take and some of them may require the use of additional memory.

### 1. Destructuring Assignment

By using this method, we can simply swap variables with only one statement. Works with number, string, boolean and object.

```jsx
let a = 'Hello';
let b = 'Hi';

[a, b] = [b, a];

console.log(a); // 'Hi'
console.log(b); // 'Hello'
```

### 2. Temporary Variable

This is the classic method that involves an additional variable to store the value of a variable and swaps between them. This method will need to use additional memory.

```jsx
let a = 'Hello';
let b = 'Hi';
let temp;

temp = a;
a = b;
b = temp;

console.log(a); // 'Hi'
console.log(b); // 'Hello'
```

### Swapping and modifying variables

Let's look at the codes first, what will be the results for two prints?

```jsx
let a = {
  name: "Hello"
};
let b = {
  name: "Hi"
};

function ex(a, b) {
  let c = b;
  b = a;
  a = c;

  a.name = a.name + "1";
  b.name = b.name + "2";
  console.log(a, b);
}
ex(a, b);
console.log(a, b);
```

**result**:

```jsx
{name: "Hi1"} {name: "Hello2"}
{name: "Hello2"} {name: "Hi1"}
```

**reason**:

When a and b are passed into the function ex,

The inner of function ex:

`let c = b;` : c points to the memory location of b now - {name: 'Hi'}

`b = a;` - b points to the memory location of a now - {name: 'Hello'}

`a = c;` - a points to the memory location of c now - {name: 'Hi'}

The following lines of codes are actually modifying the value at that memory location

`a.name = a.name + '1';` - {name: 'Hi1'}

`b.name = b.name + '2';` - {name: 'Hello2'}

so, `console.log(a,b);` - {name: 'Hi1'} {name: 'Hello2'}

The outer of function ex:

Due to the running of `function ex`

`a` still points to the **memory location** where it was originally storing {name: 'Hello'} and now is {name: 'Hello2'}

`b` still points to the **memory location** where it was {name: 'Hi'} and now is {name: 'Hi1'}

therefore, `console.log(a, b)` - {name: "Hello2"} {name: "Hi1"}

Check this graph if still confused:

![Swapping%20variables%20and%20pointing%20to%20different%20memor%20d512694f65854c91a6f97cc06bdbc0ca/Untitled.png](Swapping%20variables%20and%20pointing%20to%20different%20memor%20d512694f65854c91a6f97cc06bdbc0ca/Untitled.png)

### Summary

We can use different approaches to swap variables:

- Using the destructuring method
- Using the `temp` variable method

Most importantly, we need to be careful when we swap and modify variables which are in object type. Because there is a memory pointing crossing behind the scene. We might need to consider to use deep copy of the object or figure out the pointing correctly.