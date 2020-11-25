# Node.JS

# Introduction to Node.js

## Node.JS PROS

- Single-threaded, based on event driven, non-blocking I/O model.
- Perfect for building **fast** and **scalable** data-intensive apps.
- **JavaScript across the entire stack**: faster and more efficient development.
- **NPM**: huge library of open-source packages available for everyone for free.
- **Very activ** developer community**.**

## Use Node.js

- API with database behind it (preferably NoSQL)\
- Data streaming (like YouTube)
- Real-time chat application
- Server-side web application

## DO NOT Use Node.js

- Applications with heavy server-side processing (CPU-intensive, like having image manipulations. video conversion, file compression etc.)
    - Doing with Ruby on Rails, PHP or Python instead.

## Basic Node.js - Running JS outside of the browsers

- type  `node` in the terminal - entering Read-Eval-Print-Loop (REPL)

    `.exit` or `ctrl + D` can quit node

    ![Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled.png](Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled.png)

- type `tab` to check all global variables inside Node.js
- type `_` stands for the previous result

    ![Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled%201.png](Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled%201.png)

- Checking the methods or properties of a constructor by using `Tab`. i.e. `String.` + `Tab`

    ![Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled%202.png](Node%20JS%202a4ea88a696747169eefe6d5ced05ea6/Untitled%202.png)

- Running JS file without using a browser

    In the terminal: `node` + JS filename

## Importing a module - using require() keyword

- `const fs = require('fs');`

## File System - Reading and Writting with Files

```jsx
// accessing the file system
const fs = require('fs');

// reading txt file from node
const textInput = fs.readFileSync('./txt/input.txt', 'utf-8');
console.log(textInput);

// writting txt from node
const textOutput = `This is what we know about the avocado: ${textInput}.\nCreated on ${Date.now()}`;
fs.writeFileSync('./txt/output.txt', textOutput);
console.log('File written!');
```

## Synchronous vs. Asynchronous Code (Blocking vs. Non-Blocking)

```jsx
// accessing the file system
const fs = require('fs');

// Non-blocking code execution
fs.readFile('input.txt', 'utf-8', (err, data) => {
  console.log(data);
});
console.log('Reading file...');

// output
fs.writeFile('./txt/output-asyn.txt', textOutput, 'utf-8', err => {
  if (err) return console.log('Error!');
  console.log('Success');
});
```

## Creating An Real Simple Web Server

- Accepting requests and sending back responses
- Include the needed package in the application

    `const http = require('http');`

### Create the new server

- `createServer` will take a call-back function and get fired every time when a new request hits the server. The function takes the `request` and the `response` objects.

```jsx
const server = http.createServer((req, res) => {
	// This method signals to the server that all of the response headers and body have been sent; that server should consider this message complete. The method, response.end(), MUST be called on each response.
  res.end('Hello from the server!');
});
```

### Listen the incoming requests

```jsx
// Starts the HTTP server listening for connections
server.listen(8000, '127.0.0.1', () => {
  // this call-back function will run as soon as the server starts
  console.log('Listening to requests on port 8000');
});
```

### Simple Routing

- Using `request.url` to fetch the url that user passed into the browser.

```jsx
const server = http.createServer((req, res) => {
  const pathName = req.url;

  if (pathName === '/' || pathName === '/overview') {
    res.end('This is the OVERVIEW');
  } else if (pathName === '/product') {
    res.end('This is the PRODUCT');
  } else {
    // ...
});
```

### response.writeHead() - Sending a header with the specific status code

```jsx
const server = http.createServer((req, res) => {
  const pathName = req.url;

  if (pathName === '/' || pathName === '/overview') {
    res.end('This is the OVERVIEW');
  } else if (pathName === '/product') {
    res.end('This is the PRODUCT');
  } else {
    // sending a header with the specific status code
    **res.writeHead(404, {
      'Content-type': 'text/html',
      'my-own-header': 'hello-world'
    });**
    res.end('<h1>Page not found!</h1>');
  }
});
```

## Creating A Very Simple API

- Specifying the data type that the api will send back.
- Sending back the actual data after specifying its type.

```jsx
// reading data from file
const data = fs.readFileSync(`${__dirname}/dev-data/data.json`, 'utf-8');
const dataObj = JSON.parse(data);

// creating the server
const server = http.createServer((req, res) => {
  const pathName = req.url;

  if ... {
		...
  } **else if (pathName === '/api') {
    // specifying the sending back data is in JSON type.
    res.writeHead(200, {
      'Content-type': 'application/json'
    });
    res.end(data);**
  } else {
    ...
  }
});
```

## Node.js URL Module

The URL module splits up a web address into readable parts.

### Using URL Module

To include the URL module, use the `require()` method:

`const url = require('url');`

### url.parse()

`function parse(urlStr: string, parseQueryString: true, slashesDenoteHost?: boolean): UrlWithParsedQuery;`

Parse an address with the `url.parse()` method, and it will return a URL object with each part of the address as properties:

Some popular properties

`host, pathname, search, query`

```jsx
var url = require('url');
var adr = 'http://localhost:8080/default.htm?year=2017&month=february';
var q = url.parse(adr, true);

console.log(q.host); //returns 'localhost:8080'
console.log(q.pathname); //returns '/default.htm'
console.log(q.search); //returns '?year=2017&month=february'

var qdata = q.query; //returns an object: { year: 2017, month: 'february' }
console.log(qdata.month); //returns 'february'
```

## Passing Variables from URLs

## Create a Custom Module

### module.exports

Using `module.exports` to export our custom module

In `replactTemplate.js`

```jsx
module.exports = (temp, product) => {
  let output = temp.replace(/{%PRODUCT_NAME%}/g, product.productName);
  output = output.replace(/{%IMAGE%}/g, product.image);
  output = output.replace(/{%PRICE%}/g, product.price);
  output = output.replace(/{%FROM%}/g, product.from);
  output = output.replace(/{%NUTRIENTS%}/g, product.nutrients);
  output = output.replace(/{%QUANTITY%}/g, product.quantity);
  output = output.replace(/{%PRODUCT_DESCRIPTION%}/g, product.description);
  output = output.replace(/{%ID%}/g, product.id);

  if (!product.organic) {
    output = output.replace(/{%NOT_ORGANIC%}/g, 'not-organic');
  }

  return output;
};
```

### Using the Custom Module

**Note: it has to be starting with '.' and without specifying the file type '.js'**

`const replaceTemplate = require('./modules/replaceTemplate');`