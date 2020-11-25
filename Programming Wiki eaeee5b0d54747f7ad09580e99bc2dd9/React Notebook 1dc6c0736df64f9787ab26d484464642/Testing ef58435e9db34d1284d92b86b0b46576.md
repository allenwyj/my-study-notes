# Testing

---

# Introduction to Testing

## Type of Tests

- Unit Tests
- Integration Tests
- Automation Tests or UI Tests
    - Can be done by human or robots

## Testing Tools

- **Testing Library** - To use some function/method calls to write tests.
    - Jasmine, Just, MOCHA
- **Assertion Library** - To use assertion functions that test the variables contain the expected values.
    - Jasmine, Just, chai(paried with MOCHA)
- **Test Runner** - To run the tests, like `npm run test`
    - Jasmine, Just, MOCHA, Karma.js (run tests in the browser)
    - The places can be run tests: DOM, Puppeteer (a headless browser), **jsdom (a fake JS browser - the ideal one)**
- **Mocks spies and Stubs**
    - Jasmine, Just, Sinon.js
    - ***Mocks*** will fake a function to test different parts of a process.
    - ***Spies*** provides information about **the number of times** function calls, the **cases** and called **by** **whom**.
    - ***Stubs*** are similar to Spies, but they replace the target functions with the designed expected behaviours or returns. (can fake a server)
- **Code Coverage**
    - istanbul, Just

create-react-app uses Just.

AVA runs tests very fast.

tape - low level and simple light library.

### Unique Test Feature/Library Only For React.JS

Just - snapshot testing

enzyme - helps developers to write better tests for React components.

## Unit Tests

- Focus on pure function which may take an input and give an output.
- Stateless React components are pure components as well. They take `props` and return view.

## Integration Tests

- Connecting components to see how they work together.
- Testing the connection between different units in a system.

## Automation Tests

- UI tests
- Simulate user behaiours, like clicking ,typing or selecting etc.
- TestCafe is cheaper, Web Drier IO has good documentation, Nightmare.js is simple.

# Jest

## Install Jest

`yarn add --dev jest;`

## Setup in package.json

- It can allow Jest to monitor the code changes in the test file, and run the test automatically once the code changed.

```tsx
...
"scripts": {
    "test": "jest --watch *.js"
  },
...
```

## Simple Test Function

### Test Output

![Testing%20ef58435e9db34d1284d92b86b0b46576/Untitled.png](Testing%20ef58435e9db34d1284d92b86b0b46576/Untitled.png)

### it()

`it(testName, testFunc);`

- When we need to use the common js

    ```tsx
    const googleDatabase = [
      'cats.com',
      'souprecipes.com',
      'flowers.com',
      'animals.com',
      'catpictures.com',
      'myfavouritecats.com',
      'myfavouritecats2.com'
    ];

    const googleSearch = (searchInput, db) => {
      const matches = db.filter(website => {
        return website.includes(searchInput);
      });

      return matches.length > 3 ? matches.slice(0, 3) : matches;
    };

    **// export the unit which needs to be tested by using common js
    module.exports = googleSearch;**
    ```

    ```tsx
    **const googleSearch = require('./script');**

    // a mock databse only for test purpose
    dbMock = ['dog.com', 'cheesepuff.com', 'disney.com', 'dogpictures.com'];

    it('sample test', () => {
      // test code
      expect('hello').toBe('hello');
    });

    **it('search test', () => {
      // expecting the output of the googleSearch match the toEqual()
      expect(googleSearch('testtest', dbMock)).toEqual([]);
      expect(googleSearch('dog', dbMock)).toEqual(['dog.com', 'dogpictures.com']);
    });

    it('work with undefined and null input', () => {
      expect(googleSearch(undefined, dbMock)).toEqual([]);
      expect(googleSearch(null, dbMock)).toEqual([]);
    });

    it('does not return more than 3 matches', () => {
      expect(googleSearch('.com', dbMock).length).toEqual(3);
    });**
    ```

### describe()

`describe(groupName, func);`

- we can group our tests together based on the unit.

    ```tsx
    describe('googleSearch', () => {
      it('work with undefined and null input', () => {
        expect(googleSearch(undefined, dbMock)).toEqual([]);
        expect(googleSearch(null, dbMock)).toEqual([]);
      });

      it('does not return more than 3 matches', () => {
        expect(googleSearch('.com', dbMock).length).toEqual(3);
      });
    });
    ```

- the output would be:

    ![Testing%20ef58435e9db34d1284d92b86b0b46576/Untitled%201.png](Testing%20ef58435e9db34d1284d92b86b0b46576/Untitled%201.png)

## Testing with Asynchronous Function

- Due to testing is not running in the browser, so fetch() can't be directly used. - Have to install node version of fetch.
- If `done()` or `return` are not used in tests, the tests will be finished **before** the calls actually get called. And it will always shows z**ero assertion calls.**

### Install node-fetch

`yarn add node-fetch`

### assertions()

- This specifies how many assertion calls will be expected.

    `expect.assertions(1);`

### done()

- Passing from the it() callback.
- Using at the end of the async calls.

### return

- Return the whole promise.
- So test will wait until the promise finished and returned.

```jsx
const fetch = require('node-fetch');
const swapi = require('./script-async-case');

it('calls swapi to get people by using done', **done** => {
  // specify that we have one assertion call.
  **expect.assertions(1);**
  swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87);
    // if we have a done in code, test will not
    // be passed until done gets called.
    **done();**
  });
});

it('calls swapi to get people by using return', () => {
  // specify that we have one assertion call.
  **expect.assertions(1);**
  // using return can let test waits until this promise returns.
  **return** swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87);
  });
});

it('calls swapi to get people with a promise', () => {
  **expect.assertions(2);**
  **return** swapi.getPeoplePromise(fetch).then(data => {
    expect(data.count).toEqual(87);
    expect(data.results.length).toBeGreaterThan(5);
  });
});
```

[sapegin/jest-cheat-sheet](https://github.com/sapegin/jest-cheat-sheet)

## Mock

- By testing asynchronous events, if there are many async events, it will take lots of time to wait until these events get called. - Waste Resources.
- Mock can fake a function like calling an API. Mock can pretend other codes are running.
- Because the library like node-fetch has been tested, so we don't need to actually test it. We can mock this fetch by mocking fetch.