# GraphQL

# What is GraphQL

GraphQL is a server language that wraps around an existing database or server that you can make requests against in a different way.

**The traditional way - REST**

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled.png)

- In order to get comments for a specific post, we may need to make three different requests to the back-end (fetching post id, user id, users table) - may be an expensive solution.
- We might get lots of data from comments or posts which we don't need - over fetching data.

**GraphQL is to solve these drawbacks.**

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%201.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%201.png)

- With GraphQL, the server just exposes one single endpoint - usually called /graphql
    - From this end point, the application makes all of its requests for all of its data - doing CRUD tasks through this endpoint.
    - Make a request to this GraphQL end point - pass a query or a mutation.
        - query: ask for a data
        - mutation: modify data
    - Using JSON-like object to make the request.
        - We can determine what data that we want the server to give back by simply passing it into the object. (`posts {id title content}` - If we don't want the title, then remove it)

# The Property of GraphQL

```graphql
type Query {
  collections: [Collection!]!
  collection(id: ID!): Collection
  getCollectionsByTitle(title: String): Collection
}

type Collection {
  id: ID!
  title: String!
  items: [Item!]!
}

type Item {
  id: ID!
  name: String!
  price: Float!
  imageUrl: String!
  collection: Collection
}
```

`type` - the type of objects that are available

`ID` - An identifier (**MUST** be unique) and it gives back a JSON response as a String

`!` - specify the property is mandatory

`id, name, price, title` - the name of property

`String, Float` - Data type

`[Item!]` - Points to an array of Item type, and there must be items inside of this array.

`collection: Collection` - The owner of this object

`Query` - Defines the type of things we can ask for

`collections` - asking for all collections

`collection(id: ID!): Collection` - asking for one specific Collection by using its id. No ! for this property is becuase we may ask for an id which does not exist in the collections. So in this case, it won't give back any collection since the collection is not existing.

`getCollectionsByTitle(title: String): Collection` - By passing a String (title) to get back a Collection.

- **The object that returned to us will be the name of the method that we called (getCollectionsByTitle)**

**It  won't be able to query any Item directly, because it hasn't be set at the backend code.**

# Asking for Data

### the Query key word

```graphql
query {
  collections { # asking for the whole collection
    id # asking for id
    title # asking for title
  }
}

# Results:
{
  "data": {
    "collections": [
      {
        "id": "cjwuuj5bz000i0719rrtw5gqk",
        "title": "Hats"
      },
      {
        "id": "cjwuun2fa001907195roo7iyk",
        "title": "Jackets"
      },
      {
        "id": "cjwuuprqs00240719lb9kvlqe",
        "title": "Mens"
      },
      {
        "id": "cjwuva2zz003f07193pv1xavh",
        "title": "Sneakers"
      },
      {
        "id": "cjwuvbix2003y071935qfqr7a",
        "title": "Womens"
      }
    ]
  }
}

query {
  collections {
    id
    title
    items { # asking for the sub-array
      id
      name
      price
    }
  }
}

# Results:
{
  "data": {
    "collections": [
      {
        "id": "cjwuuj5bz000i0719rrtw5gqk",
        "title": "Hats",
        "items": [
          {
            "id": "cjwuuj5ip000j0719taw0mjdz",
            "name": "Brown Brim",
            "price": 25
          },
					... # other items
				]
			},
			... # other collection
		]
	}
}

query {
  collection(id: "safd") { # querying something doesn't exist
    title # Must give the subfileds
  }
}
# Result:
{
  "data": {
    "collection": null
  }
}
```

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%202.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%202.png)

# Apollo API - Connecting to GraphQL in ReactJS

**Apollo is that when we query for things from the GraphQL server, this client will make sure to cache the data that we fetch.**

For example, when we fetched the collections data and we navigate back to our page and the request makes a new query against the GraphQL for those collections again, if none of the data has changed, then GraphQL is not going to make the request. It's simply just going to give us back the cached version of the code that it has.

## GraphQL vs Redux

**Using Apollo and its Local Cache**

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%203.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%203.png)

**Local Cache - local state management**

The way that the application makes the query to request GraphQL for fetching data is the exact same query request for any data on the **Local Cache**. It is similar to having the firebase as the back-end database and Redux as the local state. In order to get data from the Local Cache, a query or a mutation is needed.

**Resolver**

**Resolvers** are the functions like the Reducers in Redux. The only difference is that resolver function will either modify some data or get some data once it's called. Resolver is the place where we have to write the code that **defines what types of queries or mutations we have access to** inside of the application. In the back-end server, these resolvers are also needed to connect with the Database - **Those resolver functions are what allow GraphQL Server to know what exactly users are allowed to query for or allowed to mutate.**

If those External queries or mutations are successful, it will send back the data to the application and we can choose to either store on a Local Cache or simply use it in the components. Now **with Apollo, it will always cache that data for us so that we don't end up querying the same data.**

**Using any backend database such as Firebase and Redux**

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%204.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%204.png)

## Install Apollo into the project

`yarn add apollo-boost react-apollo graphql`

## Import Apollo into the project

In the `index.js` file:

```jsx
import { ApolloProvider } from 'react-apollo';
// connect the client to the specific endpoint(/graphql)
import { createHttpLink } from 'apollo-link-http';
// used to cache the data that it fetched already
import { InMemoryCache } from 'apollo-cache-inmemory';
// gql is the function that can understand the syntax of GraphQL (the JSON-like object)
import { ApolloClient, gql } from 'apollo-boost';

// establishing the connection to the backend
// createHttpLink() takes object as the argument
// uri is the endpoint that we will use to fetch data from.
const httpLink = createHttpLink({
  uri: 'https://crwn-clothing.com'
});

// creating the cache object
const cache = new InMemoryCache();

// making the client by passing an object as the argument
// link points to where to request
// cache points to where to cache
const client = new ApolloClient({
  link: httpLink,
  cache: cache
});

// TESTING OUR CLIENT
// client has .query and .mutation to help to fetch data or update data
// both of them take an object that contains the syntax that we use in GraphQL.
// gql helps to understand these JSON-like syntax.
// the .query returns back a promise and the resolve will have the fetched data if successful
client
  .query({
    query: gql`
      {
        getCollectionsByTitle(title: "hats") {
          id
          title
          items {
            id
            name
          }
        }
      }
    `
  })
  .then(res => console.log(res))
  .catch(err => console.error(err));

// wrapped the application inside the ApolloProvider
ReactDOM.render(
  **<ApolloProvider client={client}>**
    <Provider store={store}>
      <BrowserRouter>
        <PersistGate persistor={persistor}>
          <App />
        </PersistGate>
      </BrowserRouter>
    </Provider>
  **</ApolloProvider>**,
  document.getElementById('root')
);
```

## Using GraphQL and Apollo inside of an Component

## Fetching data from the back-end - Query

### **Query without passing any parameter**

```jsx
import React from 'react';
import { Query } from 'react-apollo';
import { gql } from 'apollo-boost';

import CollectionsOverview from './collections-overview.component';
import Spinner from '../spinner/spinner.component';

// making the query to fetch data from GraphQL
**// If we are making a query without passing any parameter
// We will write without using query key word**
const GET_COLLECTIONS = gql`
  {
    collections {
      id
      title
      items {
        id
        name
        price
        imageUrl
      }
    }
  }
`;

// wrapping between Query component will allow to make the request by using query key
// and this Query component will return back a function with an object containing
// multiple fields, such as loading, error and data.
const CollectionsOverviewContainer = () => (
  <**Query** **query**={GET_COLLECTIONS}>
    {(**{ loading, error, data })** => {
      console.log({ loading });
      console.log({ error });
      console.log({ data });
      if (loading) return <Spinner />;
      return <CollectionsOverview collections={data.collections} />;
    }}
  </**Query**>
);

export default CollectionsOverviewContainer;
```

### **Query with pasing a parameter**

We need to use `query` key word in the gql to specify which query we are going to use, and telling which is the parameter with `$` symbol and its type - the blue part

Then, we need to make the GraphQL query following GraphQL syntax - the red part

```jsx
import React from 'react';
import { Query } from 'react-apollo';
import { gql } from 'apollo-boost';

import CollectionPage from './collection.component';
import Spinner from '../../components/spinner/spinner.component';

// defining the dynamic title by passing parameter
// we define the query first - what type that we are expecting for,
// then we define actually what we are going to request
const GET_COLLECTION_BY_TITLE = gql`
  **query getCollectionsByTitle($title: String!) {
    getCollectionsByTitle(title: $title) {**
      id
      title
      items {
        id
        name
        price
        imageUrl
      }
    }
  **}**
`;

const CollectionPageContainer = ({ match }) => (
  <Query
    query={GET_COLLECTION_BY_TITLE}
    **variables={{ title: match.params.collectionId }}**
  >
		{
      // If loading is true, data will be undefined
    }
    {({ loading, data }) => {
      if (loading) return <Spinner />;
      const { getCollectionsByTitle } = data;
      return <CollectionPage collection={getCollectionsByTitle} />;
    }}
  </Query>
);

export default CollectionPageContainer;
```

[7. Fetch data with queries](https://www.apollographql.com/docs/tutorial/queries/)

## Storing and Updating data to local cache - Mutation

### **Introduced local cache to the application**

In `index.js` file

- client.writeData() - setting the local state in the local cache.

```jsx
...
import { resolvers, typeDefs } from './graphql/resolvers';

...

// making the client by passing an object as the argument
// link points to where to request
// cache points to where to cache
const client = new ApolloClient({
  link: httpLink,
  cache: cache,
  **typeDefs,
  resolvers**
});

// write data to the cache immediately after the client instantiated
// these data will be stored as cache
**client.writeData({
  data: {
    cartHidden: true
  }
});**

...
```

### **Setting up resolvers**

resolvers is like reducers in the redux. 

In `resolvers.js` file

```jsx
import { gql } from 'apollo-boost';

// extends in graphql: extend to the existing type of mutation that might exist in the back-end.
// If exists, then modify it
// If doesn't exist, then add it
// it's type definition inside of the Mutation.
export const typeDefs = gql`
    extend type Mutation {
        ToggleCartHidden: Boolean! // name of the property: its type
    }
`;

// A query to read from the cache about the initial value
// @client tells to specify the Apollo to fetch data from the client side - looking for the value on the local cache.
const GET_CART_HIDDEN = gql`
  {
    cartHidden @client
  }
`;

// defining the mutations, queries or additional types that might exist on the client side
// Inside of Mutation, they are the actual mutation definitions.
// Mutation takes four parameter and they are meant to not be modified.
// _root：represents the top level object that holds the actual type. like the parent class.
//        For mutation object, it will be an empty object, because mutation is the highest level object.
// _args: represents all of the arguments that can be accessed to inside of the mutation.
// _context: the cache and the client, normally just destructuring: { cache }
// _info: the information about the query or the mutation
export const resolvers = {
  Mutation: {
    toggleCartHidden: (_root, _args, { cache }) => {
      // passing the actual query to make an request to the cache
      // { cartHidden } = data.cartHidden
      const { cartHidden } = cache.readQuery({
        query: GET_CART_HIDDEN
      });

      // updating the data, query identifies where to update
      cache.writeQuery({
        query: GET_CART_HIDDEN,
        data: { cartHidden: !cartHidden }
      });

      return !cartHidden;
    }
  }
};
```

### **Fetching data from the local cache**

**Similar to fetching data from the back-end database**

In the `header.container.jsx` file

```jsx
import React from 'react';
import { Query } from 'react-apollo';
import { gql } from 'apollo-boost';

import Header from './header.component';

// query for fetching data
// @client - identify fetching data from the local cache
const GET_CART_HIDDEN = gql`
  {
    cartHidden @client
  }
`;

// destructuring data property from the returned object
const HeaderContainer = () => (
  <Query query={GET_CART_HIDDEN}>
    {({ data: { cartHidden } }) => <Header hidden={cartHidden} />}
  </Query>
);

export default HeaderContainer;
```

### **Mutating data to the local cache - inside of a component container**

**Mutating data without passing any parameter**

In the `cart-icon.container.jsx` file

```jsx
import React from 'react';
import { Mutation } from 'react-apollo';
import { gql } from 'apollo-boost';

import CartIcon from './cart-icon.component';

// specifying which mutation type will be called (ToggleCartHidden),
// and for this type of mutation, clarifying what's the actual mutation body (toggleCartHidden)
// and where to find this body - defined in the resolvers on the client side.
// toggleCartHidden is an arrow function in the resolvers
const TOGGLE_CART_HIDDEN = gql`
  mutation ToggleCartHidden {
    toggleCartHidden @client
  }
`;

// There will be a function between Mutation component tag
// toggleCartHidden will be passed by Mutation tag as the parameter, which is an call-back function
// toggleCartHidden will get called inside of CartIcon component, it actually changes the value stored in the cache
const CartIconContainer = () => (
  <Mutation mutation={TOGGLE_CART_HIDDEN}>
    {toggleCartHidden => <CartIcon toggleCartHidden={toggleCartHidden} />}
  </Mutation>
);

export default CartIconContainer;
```

In the `cart-dropdown.container.jsx`

```jsx
import React from 'react';
import { Query, Mutation } from 'react-apollo';
import { gql } from 'apollo-boost';

import CartDropdown from './cart-dropdown.component';

const TOGGLE_CART_HIDDEN = gql`
  mutation ToggleCartHidden {
    toggleCartHidden @client
  }
`;

const GET_CART_ITEMS = gql`
  {
    cartItems @client
  }
`;

const CartDropdownContainer = () => (
  <Mutation mutation={TOGGLE_CART_HIDDEN}>
    {toggleCartHidden => (
      <Query query={GET_CART_ITEMS}>
        {({ data: { cartItems } }) => (
          <CartDropdown
            cartItems={cartItems}
            toggleCartHidden={toggleCartHidden}
          />
        )}
      </Query>
    )}
  </Mutation>
);

export default CartDropdownContainer;
```

**Mutating data with passing parameters**

In the `collection-item.container.jsx` file

```jsx
import React from 'react';
import { Mutation } from 'react-apollo';
import { gql } from 'apollo-boost';

import CollectionItem from './collection-item.component';

// AddItemToCart is taking an argument item. so $item is a variable, and its type is Item!
// addItemToCart will be passed an argument which the name is item and its value is $item.
const ADD_ITEM_TO_CART = gql`
  mutation AddItemToCart($item: Item!) {
    addItemToCart(item: $item) @client
  }
`;

const CollectionsItemContainer = props => (
  <Mutation mutation={ADD_ITEM_TO_CART}>
    {addItemToCart => (
      <CollectionItem
        {...props}
        addItem={item => addItemToCart({ variables: { item } })}
      />
    )}
  </Mutation>
);

export default CollectionsItemContainer;
```

## Compose - Using HOC into Apollo

We can compose the Query and Mutation components as a Higher-order component by using `lodash` library (`compose` was removed from `react-apollo` lib). And the component will take the values as `props`.

### Installation

You can install lodash in your project by adding it as a dependency as follows:

If you're using yarn:

`yarn add lodash`

If you're using npm:

`npm install lodash`

You can then import flowRight into your file like so:

`import { flowRight } from 'lodash';`

and just replace any place in the lesson where we use `compose` with `flowRight`:

`export default compose( //...code )(CollectionItemContainer);`

becomes

`export default flowRight( // ...code)(CollectionItemContainer);`

**graphql**

It takes mutations and queries and it binds them to some components that we can pass as the outcome of that function.

Import `graphql` from `react-apollo`

`import { graphql } from 'react-apollo';`

![GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%205.png](GraphQL%20fe190f00fcdd4568a4eb9310659ad095/Untitled%205.png)

In the `cart-icon.container.jsx` file

```jsx
import React from 'react';
import { graphql } from 'react-apollo';
import { flowRight } from 'lodash';
import { gql } from 'apollo-boost';

import CartIcon from './cart-icon.component';

// specifying which mutation type will be called (ToggleCartHidden),
// and for this type of mutation, clarifying what's the actual mutation body (toggleCartHidden)
// and where to find this body - defined in the resolvers on the client side.
// toggleCartHidden is an arrow function in the resolvers
const TOGGLE_CART_HIDDEN = gql`
  mutation ToggleCartHidden {
    toggleCartHidden @client
  }
`;

const GET_ITEM_COUNT = gql`
  {
    itemCount @client
  }
`;

// There will be a function between Mutation component tag
// toggleCartHidden will be passed by Mutation tag as the parameter, which is an call-back function
// toggleCartHidden will get called inside of CartIcon component, it actually changes the value stored in the cache.
const CartIconContainer = ({ data: { itemCount }, toggleCartHidden }) => (
  <CartIcon toggleCartHidden={toggleCartHidden} itemCount={itemCount} />
);

export default flowRight(
  // having access to the data object by using graphql, and it will be passed as props
  graphql(GET_ITEM_COUNT),
  // By default, it will give back the mutation as the prop, named mutate,
  // so passing an config object as the second argument to change the name.
  graphql(TOGGLE_CART_HIDDEN, { name: 'toggleCartHidden' })
)(CartIconContainer);
```