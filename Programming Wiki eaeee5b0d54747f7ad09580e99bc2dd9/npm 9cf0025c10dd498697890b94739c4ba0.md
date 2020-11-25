# npm

# Install Package

### Install the Package Globally

`sudo npm install packageName -g`

### Install the Package Locally

`npm install packageName`

### Install the Package Locally and Only for Dev Purpose

`npm install packageName --save-dev`

### Install all Packages listed in package.json

`npm install`

### Update Package

`npm update packageName@version`

### Delete Pacakge

`npm uninstall packageName`

### **Update npm 5**

- As of [npm 5.0.0](http://blog.npmjs.org/post/161081169345/v500), installed modules are added as a dependency by default, so the `--save` option is no longer needed. The other save options still exist and are listed in the [documentation](https://docs.npmjs.com/cli/install) for `npm install`.

*Original answer:*

- Before version 5, NPM simply installed a package under `node_modules` by default. When you were trying to install dependencies for your app/module, you would need to first install them, and then add them (along with the appropriate version number) to the `dependencies` section of your `package.json`.
- The `--save` option instructed NPM to include the package inside of the `dependencies` section of your `package.json` automatically, thus saving you an additional step.
- In addition, there are the complementary options `--save-dev` and `--save-optional` which save the package under `devDependencies` and `optionalDependencies`, respectively. This is useful when installing development-only packages, like `grunt` or your testing library.

# Package.json

## Package Version

```json
{
  ...
  "dependencies": {
    "slugify": "^1.4.5"
  },
  "devDependencies": {
    "nodemon": "^2.0.4"
  }
}
```

- `1.4.5`

    `1` - Introduced some breaking chages

    `4` - Introduced some new features, but not slightly changing the package

    `5` - Fixing bug and released

- Fixing package version

    `~` - fixes major and minor numbers. 

    **“Approximately equivalent to version”**, will update you to all future patch versions, without incrementing the minor version. `~1.2.3` will use releases from 1.2.3 to <1.3.0.

    `^` - fixes the major number only.

    **“Compatible with version”**, will update you to all future minor/patch versions, without incrementing the major version. `^2.3.4` will use releases from 2.3.4 to <3.0.0.

# package-lock.json

Listing the exact version of all packages (including their dependencies) that are used when the developers were developing the project. And other developers could be able to use `package.json` and `package-lock.json` files to reconstruct the `node_modules` folder.