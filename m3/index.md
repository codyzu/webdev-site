# Module 3 NoSQL

# 1 NoSQL Introduction

NoSQL or document based databases let us store data without enforcing a schema. Instead of adding rows to tables, we store documents in collections. A document (similar to a row in SQL) stores key value pairs. Think of a document as JSON data.

Imagine a user document:
```json
{
  "name": "Cody",
  "address": "Annecy"
}
```

We can have objects and arrays in our documents. Imagine another user document:
```json
{
  "name": {
    "first": "Brian",
    "last": "Smith"
  },
  "address": "Annecy"
}
```

We would most likely store these documents in a collection named "users". Collections are used to organize documents. We can think of them as directories.

`/users/cody`:
```json
{
  "name": "Cody",
  "address": "Annecy"
}
```

`/users/briansmith`:
```json
{
  "name": {
    "first": "Brian",
    "last": "Smith"
  },
  "address": "Annecy"
}
```

Notice that the two documents DO NOT have the same schema. NoSQL lets us do this because there is no schema enforced. However, documents should have the same schema to allow us to query them.

Documents are identified by an id. Notice that the documents have no explicit "id" field. In SQL you would be required to identify a primary key in a table and every row must have a unique primary key. Similarly, in NoSQL every document must have a unique id, but we are not required to store the id in the document. Instead we we reference the document by it's id.

When storing a document we can either choose the id for the document or let the DB assign a random id.

The database can have many collections.

For this course we will use [Cloud Firestore](https://firebase.google.com/products/firestore/) from Firebase. Firestore is NoSQL document database with several advantage for creating cloud based applications.

# 2 Countries DB

```json
{
 "altSpellings": ["CA"],
 "area": 9984670,
 "borders": ["USA"],
 "bordersObject": {"USA": true},
 "callingCode": ["1"],
 "capital": "Ottawa",
 "cca2": "CA",
 "cca3": "CAN",
 "ccn3": "124",
 "cioc": "CAN",
 "currency": ["CAD"],
 "demonym": "Canadian",
 "geo": {"lat": 60, "lng": -95},
 "landlocked": false,
 "languages": {"eng": "English", "fra": "French"},
 "latlng": [60, -95],
 "name": {
   "common": "Canada",
   "native": {
     "eng": {"common": "Canada", "official": "Canada"},
     "fra": {"common": "Canada", "official": "Canada"}
   },
   "official": "Canada"
 },
 "region": "Americas",
 "subregion": "Northern America",
 "tld": [".ca"],
 "translations": {
   "cym": {"common": "Canada", "official": "Canada"},
   "deu": {"common": "Kanada", "official": "Kanada"},
   "fin": {"common": "Kanada", "official": "Kanada"},
   "fra": {"common": "Canada", "official": "Canada"},
   "hrv": {"common": "Kanada", "official": "Kanada"},
   "ita": {"common": "Canada", "official": "Canada"},
   "jpn": {"common": "カナダ", "official": "カナダ"},
   "nld": {"common": "Canada", "official": "Canada"},
   "por": {"common": "Canadá", "official": "Canadá"},
   "rus": {"common": "Канада", "official": "Канада"},
   "spa": {"common": "Canadá", "official": "Canadá"}
 }
}
```

## 2.1 Node.js Setup

Setup project to do queries:

1. Start with a new directory
   ```cmd
   mkdir countries
   cd countries
   ```
1. Initialize your npm project using yarn
   ```cmd
   yarn init
   ```
   Press enter in all of the prompts.
1. Install the firebase client library
   ```cmd
   yarn add firebase
1. Open your project directory in vscode
1. Add the following file to your project, named `countries.js`:
```javascript
'use strict';

const firebase = require('firebase');

// Initialize Firebase
firebase.initializeApp({
 apiKey: "AIzaSyC4dlGHunlrM_YL6b_5YrTVH6VFIfg-aUQ",
 projectId: "tetras-restaurants",
});

// Access the firestore API
// https://firebase.google.com/docs/reference/js/firebase.firestore.Firestore
const firestore = firebase.firestore();

async function queryRestaurants() {
 try {
   // YOUR CODE HERE


 } catch(error) {
   console.log(error);
 } finally {
   // Delete the application to close the connections to the DB
   await firebase.app().delete();
 }
}

queryRestaurants();
```

The above code segment initializes the Firebase API to run queries in Firestore. The function queryCountries() is a function where you can add queries by accessing the firestore object.

## 2.2 Accessing collections and documents

As discussed above, Firestore documents are organized into collections (similar to a table in SQL). The collection method provides a reference to a collection. The [CollectionReference](https://firebase.google.com/docs/reference/js/firebase.firestore.CollectionReference) contains several methods for accessing documents stored in that collection.

We can access a Firestore document by id with the `doc()` method of the CollectionReference. The `doc()` method returns a [DocumentReference]().

References, as their names imply, are a reference to a document in the DB. In order to read the contents of the document we have to fetch the document with the `get()` method. However, `get()` returns a promise since it is an asynchronous operation. We will use the await keyword access the resolved value of the promise The promise will resolve to a [DocumentSnapshot](https://firebase.google.com/docs/reference/js/firebase.firestore.DocumentSnapshot) which is the state of the document at the moment it was fetched from the DB.

Add the following code into your `queryRestuarants()` function:

```javascript
const countries = firestore.collection('countries');
const franceRef = countries.doc('FRA');
const franceSnapshot = await franceRef.get();
console.log(franceSnapshot.data());
```

Execute your restaurants.js script to see the results. node .\restaurants.js

Of course, we could have fetched the document with a single line of code:

```javascript
const franceSnapshot2 = await firestore
  .collection('countries')
  .doc('FRA')
  .get();
console.log(franceSnapshot2.data());
```

#### Question 2.1 What is the value of "size" of the FRA document?

The DocumentSnapshot has some other interesting properties:
- `id` - Contains the id of the document. Note that the data of the document and the id are not stored in the same place.
- `exists` - Indicates if the document exists. This is important because no error is thrown when we attempt to fetch a document that does not exist. Therefore when working with snapshots that were fetched by id, it is a good idea to check the exists flag.
- `ref` - The DocumentReference. In our example above, we already have a reference, so it is not very useful. However, if we had executed a query that returns multiple documents, the ref can be used in subsequent operations (such as deleting and updating).

## 2.3 Simple Query