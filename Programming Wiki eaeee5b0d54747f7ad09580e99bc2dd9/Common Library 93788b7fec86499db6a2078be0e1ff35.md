# Common Library

# nodemon

nodemon is a tool that helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected.

nodemon does not require any additional changes to your code or method of development. nodemon is a replacement wrapper for node. To use nodemon, replace the word node on the command line when executing your script.

# Firebase

## Authentication

### Get the currently signed-in user

We can use an observer on the Auth object to know whether the current user is changed or not.

- `onAuthStateChanged()`
    - The connection is always open as long as our application component is mounted.
    - We also need to handle the closing of this open connection to avoid memory leaks
        - `firebase.auth().onAuthStateChanged()` returns `firebase.Unsubscirbe` method
        - So we can call this method to close the connection

            ```jsx
            this.unsubscribeFromAuth = firebase.auth().onAuthStateChanged(function(user) {
              if (user) {
                // User is signed in.
              }
            });

            this.unsubscribeFromAuth();
            ```

    - Whenever we call the `onAuthStateChanged()` or `onSnapshot()` methods from our `auth` library or r`eferenceObject,` we get back a function that lets us unsubscribe from the listener we just instantiated.

## Firestore

### Add Data

- `db.collection(collectionName).doc(**id**).set(data)`
    - When you use `set()` to create a document, you must specify an ID for the document to create
        - can use add() instead of set() to create an auto-generated id document.
    - If the document does **not exist**, it will be **created**. If the document **does exist**, its contents will be **overwritten** with the newly provided data, unless you specify that the data should be merged into the existing document.
    - It is an asynchronous function.

    [Add data to Cloud Firestore | Firebase](https://firebase.google.com/docs/firestore/manage-data/add-data?authuser=0)

### Read Data

![Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled.png](Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled.png)

![Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%201.png](Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%201.png)

### Get Real-time Data

- **documentSnapshot** object

    ![Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%202.png](Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%202.png)

    - `documentRef.onSnapshot()`
        - returns a **documentSnapshot** object
            - `exists` and `id` are the common useful fields.

        ![Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%203.png](Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%203.png)

    - We are not going to get any data from the documentSnapshot unless we use `documentSnapshot.data()`

        ![Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%204.png](Common%20Library%2093788b7fec86499db6a2078be0e1ff35/Untitled%204.png)