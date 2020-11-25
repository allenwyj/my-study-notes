# webpack

## webpack config - bundle multiple js files

`yarn add --dev webpack webpack-cli`

1. Add `dev` and `build`scripts into `package.json` 
2. Config `webpack.config.js` file

---

**File**: **webpack.config.js**

***Everything has to be written in ES5 syntax.***

- `development mode` - simply builds our bundler without minifying our code, in order to be as fast as possible.
- `production mode` - will automatically enable all kinds of optimisation in order to reduce to final bundle size.
- `entry` - the entry property of this project, where the webpack will start the bundling.
    - we can specify more than one entry file.
- `output` - where to save the output bundle file.
    - `path` - MUST be an absolute path
        - `__dirname` - the current absolute path of the project
        - `'dist'` - the folder that we want our bundle.js in.
    - `filename` - a standard name for webpack output.

```jsx
File: webpack-config.js
// using build-in node package
const path = require('path'); 

module.exports = {
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/bundle.js'
    }
		//mode: 'development' // can move this to npm script
};

File: package.json
{
  "name": "forkify",
  "version": "1.0.0",
  "description": "forkify project",
  "main": "index.js",
  "scripts": { // npm script
    **"dev": "webpack --mode development", // npm run dev => set to development mode
    "build": "webpack --mode production" // npm run build => set to production mode**
  },
  "author": "Yujie Wu",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "jquery": "^3.5.1"
  }
}
```

***webpack***

- By using bundle, now we can use the export something via `export` keyword and then we can have a `default` export or a `named` export.
    - `export default 23;` - in test.js file.
    - then we can simply by using `import` from another js file and we don't need to specify the .js for the export file.
        - `import num from './test';` - in index.js
- add an npm script, in **package.json** file
    - changed `"test"` to `"dev"` which stands for development
    - and the value to `"webpack --mode development"` (to call webpack) - so as soon as we start the npm script called dev, it will open the webpack and loop up our configuration and do the work that we specify in the config file.
        - npm script is the best way for now to lanuch our locally installed dev dependency, like webpack.

***webpack-cli***

- install the webpack command line interface (something that allows us to access the webpack throught the command line interface.)
- To actually run our npm script in the command line, type `npm run dev`
    - it bundles our test.js and index.js together and output the bundle.js file.

## webpack server - stimulate a real http server

- Automatically bundle all our JavaScript files, and then reload the app in a browser whenever we change a file.
- When we are running the development server, **webpack will bundle our modules together, but it will actually not write it to a file on disk. It will automatically inject it into the html.**
- Install - run in command line
    - `npm install webpack-dev-server --save-dev`
- A script start will always be a script that keeps running in the background and updates the browser as soon as we changed the code. Add into package.json and `"script"` property.
    - `**"start": "webpack-dev-server --mode development --open"**`
        - `--open` - open up the page in the browser automatically
- Run the html page in the webpack server
    - stimulate a real http server
    - `npm run start`
- **specify the output folder is 'dist' which MUST be same as the contentBase folder**

```jsx
File: webpack.config.js

const path = require('path');

module.exports = {
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname, **'dist'**), // the output folder 'dist'
        filename: **'js/bundle.js'**
    },
    devServer: {
        contentBase: './dist' // specify from which webpack should serve our files
    }   
};
```

## webpack plugin - copy and use the template

- Install the webpack plugin
    - `npm install html-webpack-plugin --save-dev`
- require it in the webpack.config.js
- we use the index.html in src folder as the template and inject it into the index.html in dist folder.

    ```jsx
    const path = require('path');
    **const HtmlWebpackPlugin = require('html-webpack-plugin');**

    module.exports = {
        entry: './src/js/index.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'js/bundle.js'
        },
        devServer: {
            contentBase: './dist'
        }**,
        plugins: [
            new HtmlWebpackPlugin({
                filename: 'index.html',
                template: './src/index.html'
            })
        ]**
    };
    ```

## babel and loaders

### JavaScript Loader - Inject and Convert codes down to ES5

`npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader`

OR 

`yarn add --dev @babel/core @babel/preset-env @babel/preset-react babel-loader`

***@babel/core***

- contains the core functionality of the compiler

***@babel/preset-env***

- it will take care that all the modern JavaScript features are converted back to ES5 and process them.
- Like SASS → CSS or ES6 → ES5

***@babel-loader***

- loader is the actual package which is a Transpiler to make the codes down to the version that we want.
- loaders in webpack allow us to import or to load all kinds of different files
- `test` - using regex to test these .js file at the end and if they are js file, then it will apply the babel loader
- `exclude` - using regex, we dont need to include those package files.

---

### CSS Loader - Inject and Convert CSS

`yarn add --dev style-loader css-loader`

***style-loader***

- Inject CSS into the DOM.
- This loader is to convert different types of Style files and allow we to **import** style files into React component files. i.e. `import './style.css';`

***css-loader***

- Convert CSS into a version which can be read by JavaScript files.

---

### HTML Loader - Inject and Convert

`yarn add --dev html-loader html-webpack-plugin`

***html-loader***

- Export HTML as string. Read HTML files inside the React application.

***html-webpack-plugin***

- simplifies creation of HTML files for webpack bundles.

---

So in React application, ***babel-loader*** will always look for `index.js` in `src` folder, ***html-webpack-plugin*** will use index.html as the template, and run `index.js` on the `index.html`.

### webpack.config.js for React Application

```jsx
**File: webpack.config.js**

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  module: {
    // rules property defines what webpack needs to do
    // based on certain file types that are defined in test property.
    rules: [
      {
        // Defining what type of files are looking for
        test: /\.(js|jsx)$/, // looking for all .js and .jsx files
        exclude: /node_modules/, // no need to include these files
        // Defining what loader will be used
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.(css)$/,
        use: ['style-loader', 'css-loader'] // loaders evaluate from right-to-left
      },
      {
        test: /\.(html)$/,
        use: 'html-loader'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ]
};
```

### Config .babelrc File

under the project folder, create a new file named **.babelrc** (standard name)

- `presets` - a collectioin of code transform plugins which actually apply the transformation of our codes
    - `preset-env` - standards for the environment
- `options` - written in a new list with the `preset-xxx`
    - `useBuiltIns` - directly injects the correct version of core-js
- `npm install --save core-js@3 regenerator-runtime`

```jsx
File: .babelrc
{
  "presets": [
    **[**
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "corejs": "3",
        "targets": {
          "browsers": [
            // which browsers we want to target
            "last 5 versions",
            "ie >= 8" // the ie is more than 8
          ]
        }
      }
    **]**,
    "@babel/preset-react"
  ]
}
```

### polyfill - for codes that are not in ES5

- convert those codes which are not in ES5, like promise, array.from
- `npm install babel-polyfill --save` (**we don't need to install babel-polyfill anymore**)
    - polyfill will be the code which is in our final code. It is not a development tool but it will be the code goes into our final bundle. So this is not a devdependency