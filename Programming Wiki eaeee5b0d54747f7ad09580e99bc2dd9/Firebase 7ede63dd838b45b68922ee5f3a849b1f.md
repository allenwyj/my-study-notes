# Firebase

# QueryReference and QuerySnapshot

A query is a request we make to firestore to give us something from the database

Firestore returns us two types of objects: **references** and **snapshots**, they can be in either document version or collection version.

Firestore will **always** return us these objects, even if nothing exists at from that query.

## QueryReferences

A **queryReference object** is an object that represents **the 'current' place** in the database that we are querying.

We get them by calling either:

**documentRef**: `firestore.doc('/users/:userId');` or `firestore.collection('/users').doc('userId);`

**collectionRef**: `firestore.collection('/users');`

The **queryReference** object does not have the actual data of the collection or document. It instead has properties that tell us details about it, or the method to get the Snapshot object which gives us the data we are looking for.

## DocumentReference vs CollectionReference

### CRUD

documentRef objects ⇒ perform CRUD methods (`.set() .get() .update() .delete()`)

### Add Document

**adding documents** to collections ⇒ `collectionRef.add({//value: prop})`

### Get snapshotObject from referenceObject

We **get the snapshotObject from the referenceObject** using the `.get()` method. ie.  `documentRef.get()` or `collectionRef.get()`

- documentRef returns a **documentSnapshot** object
- collectionRef returns a **querySnapshot** object

## Snapshots

### DocumentSnapshot

documentReference object ⇒ documentSnapshot object

**Checking existing** 

The **documentSnapshot** object allows us to check if a document exists at this query using the `.exists` property which returns a boolean.

**Getting data from document snapshot**

Getting the actual properties on the object by calling the `.data()` method, which returns us a JSON object of the document.

### QuerySnapshot

collectionReference object ⇒ querySnapshot object

To **check** if there are **any documents** in the collection, we can call `.empty` property which returns a boolean.

**Getting documents from query snapshot**

To **get all the documents** in the collection, we can call `.docs` property. It returns an **array** of our documents as **documentSnapshot objects.**

To **get the size of the collection**, we can call `.size` property. It returns a number of the number of documents in the collection.

## Batch

batch object allows us to batch all the requests that will make calls to the firebase, and put them into one big call when we fire the batch.

```jsx
export const addCollectionAndDocuments = async (collectionKey, objectsToAdd) => {
  const collectionRef = firestore.collection(collectionKey);

  // setting the batch  object and allow us to process all calls in one big calls.
  const batch = firestore.batch();

  objectsToAdd.forEach(obj => {
    // creating a new document ref with empty string and generating an random document id
    const newDocRef = collectionRef.doc();
    batch.set(newDocRef, obj);
  })

  // batch.commit() is a promise function, and we want to reuse its return
  return await batch.commit();
};
```

# Security Rules