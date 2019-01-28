# Module 3 NoSQL

# 1 NoSQL introduction

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

# 2 Countries database

There is an existing Firestore DB with single "countries" collection. Here is an example of the /countries/CAN document that contains information about Canada:

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

## 2.1 Node.js setup

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

## 2.3 Simple query

We can query a collection using the where() method of the CollectionReference. A simple query would look like this:

```javascript
const european = await countries
  .where('subregion', '==', 'Western Europe')
  .get();
```

The first parameter to the `where()` function is the field or path to query on, followed by an operator, followed by the value to compare with.

Executing a query returns a QuerySnapshot. We can access the DocumentSnapshot's via the `docs` property of the QuerySnapshot. The `docs` property contains an array of DocumentSnapshots.

#### Question 2.1 How could you transform the docs property to an array containing the data of each document? Write the line of code below to transform the docs property to an array of document data. (Hint: use map)

There are two other useful properties of a QuerySnapshot:
- `size` - The number of documents in the snapshot.
- `emtpy` - True if there are no documents in the snapshot.

Remember, the `where()` function can use a path to a nested field. For example, to the path to the french language would be 'languages.fra'.

#### Question 2.2 How many countries speak "French"?

#### Question 2.3 How many countries speak "English"?

There are several operators we can use in a where clause:
- `'<'`
- `'<='`
- `'=='`
- `'>'`
- `'>='`
- `'array-contains'`

The `'array-contains'` operator checks if an array contains a value.

The following query lists the names of all of the countries that are larger than 9000000 m2:

```javascript
const bigCountries = await countries.where('area', '>', 9000000).get();
const bigNames = bigCountries.docs.map(d => d.data().name.common);
console.log(bigNames);
```

#### Question 2.4: How many countries are smaller than 10 m2?

## 2.4 Compound queries

Where clauses can be combined to provide more specific queries. For example, to query all french speaking countries in Western Europe:

```javascript
const frenchWesternEurope = await countries
  .where('languages.fra', '==', 'French')
  .where('subregion', '==', 'Western Europe')
  .get();
console.log(frenchWesternEurope.docs.map(d => d.data().name.common));
```

#### Question 2.5: How many countries speak french AND are larger than 500000 m2?

⚠️*Warning!*{: style="color: red"}

The comparison operators '<', '<=', '>', '>=' and 'array_contains' require an index to be configured in Firestore when they are combined with the '==' operator. Indexes are configured by an administrator.

The operators '<', '<=', '>', and '>=' can be repeated with multiple calls to the where function, but only on the same field and 'array_contains' can only be included once in a query.

#### Question 2.6: What is the result of executing the following query?

```javascript
const frenchWesternEurope = await countries
  .where('area', '>', 100000)
  .where('subregion', '==', 'Western Europe')
  .get();
console.log(frenchWesternEurope.docs.size);
```

## Order and limit

When writing queries, we can also order and limit the number of documents queried. For example we can fetch the first 3 smallest countries:

```javascript
const smallest3 = await countries
  .orderBy('area')
  .limit(3)
  .get();
console.log(smallest3.docs.map(d => d.data().name.common));
```

In reverse order:

```javascript
const biggest3 = await countries
  .orderBy('area', 'desc')
  .limit(3)
  .get();
console.log(biggest3.docs.map(d => d.data().name.common));
```

`orderBy()` statements can be repeated for more advanced ordering. Below we can order by subregion, then by area:

```javascript
const largestByRegion = await countries
  .orderBy('subregion')
  .orderBy('area', 'desc')
  .limit(20)
  .get();
console.log(
  largestByRegion.docs.map(d =>
    `${d.data().subregion}, ${d.data().name.common}`
  )
);
```

Ordering can also be combined with where() criteria to further refine our query. For example, to query the largest countries in Western Europe:

```javascript
const largestWesternEurope = await countries
  .where('subregion', '==', 'Western Europe')
  .orderBy('area', 'desc')
  .limit(10)
  .get();
console.log(largestWesternEurope.docs.map(d => d.data().name.common));
```

#### Question 2.7: List the french speaking countries that share a border with france (HINT: the 'borders' array will contain the value 'FRA' if the country shares a border with France).

#### Question 2.8: List all of the countries that speak both French and English.

# Structuring data

In a relational database, like SQL, we would organize the different types of objects into separate tables and define relationships between those tables. We can then query data and include the related data with SQL JOIN statements.

In NoSQL we organize our data into collections. However, we are unable to "join" data between several collections.

## 3.1 Denormalization

**Normalizing** data is the process of minimizing the redundant data. The advantage to Normalized data is that it is easier to maintain integrity and typically reduces the storage cost. This is typically what we do in a SQL database: many tables that are related with primary and foreign keys. This requires developers to write queries with JOIN statements.

**De-normalizing** is adding redundant data in order to simplify data access. In a NoSQL database, it is not possible to query data from multiple collections in the same query. Therefore, it is often necessary to duplicate data between collections to minimize the number of queries that are required to get the data of interest.

In order to de-normalize data, we must first understand the needs of our application.

## 3.2 Application Requirements

Imagine an application that rates restaurants. We have 3 collections:
* restaurants
* users
* ratings

Users can:
* List restaurants based on average review score.
* See the details of a restaurant, including all of the reviews.
* A review shows the rating (0 to 5), data, description, and the username of the reviewer.
* A user has an email address and username.

#### Question 3.1: What data would you store in each type of document (restaurant, user, ratings) to minimize the required queries to show the various pages?

# 4 Integration

#### Exercise 4.1: Add a route into your express server `/regions/:region` that queries the countries DB  by subregion and lists the results ordered by size (area). Use an ordered html list `<ol>` to list the country names.

* Query the DB inside the reqeust route.
* Pass the query results when rendering the page template with pug.
* In the pug template, render the ordered list.

# 5 Problems

Show problems

Show other DB's with solutions

Text search
option 1: split titles into array and use array_contains. note: we can only contain a single array_contains per query... have to manually "OR" things together.
option 2: use 3rd party library
discussion: is this a big deal??? if we really need full text search, use aglolia or elastic search. For AirBnB, we will search by postal code. Or use a dedicated form (allowing us to create indexes for all possible combinations.

Counters
Aggregation Queries

# 6 Further reading