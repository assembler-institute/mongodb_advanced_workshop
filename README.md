`mongodb` `#assembler-school` `#master-in-software-engineering`

# Assembler School: MongoDB Advanced <!-- omit in toc -->

In this workshop you will learn how to perform advanced queries in order to optimize your internal and external processes.

## MongoDB Queries

This file contains a list of MongoDB queries for filtering and manipulating data stored in collections.

### Find Documents

The db.sales.find() function is used to retrieve documents from the sales collection. The following are examples of different ways to filter the data:

1. Find documents where the name is equal to "pens" or where the items tag is equal to "writing".

```bash
> db.sales.find({
  $or: [{ 'name': 'pens' }, { 'items.tag': 'writing' }]
})
```

2. Find documents where the _id is equal to a specified ObjectId.

```bash
> db.sales.find({ _id: ObjectId("5bd761dcae323e45a93ccff4") });
```

3. Find documents where the storeLocation is either "London" or "New York".

```bash
> db.sales.find({ storeLocation: { $in: ["London", "New York"] } });
```

4. Find documents where the price of an item is greater than 200.

```bash
> db.sales.find({ "items.price": { $gt: 200 } });
```

5. Find documents where the price of an item is less than 25.

```bash
> db.sales.find({ "items.price": { $lt: 25 } });
```

6. Find documents where the quantity of an item is greater than or equal to 10.

```bash
> db.sales.find({ "items.quantity": { $gte: 10 } });
```

7. Find documents where the age of the customer is less than or equal to 45.

```bash
> db.sales.find({ "items.price": { $lt: 25 } });
```

8. Find all documents in the accounts collection where the products array contains the value "CurrencyService".

```bash
> db.accounts.find({ products: "CurrencyService" })¡

```
### Replace a document

In MongoDB, the replaceOne() method is used to replace an existing document with a new one. The method takes two arguments:

- A filter that specifies the document to be replaced
- The new document to replace the existing one

The method replaces the entire document, rather than just updating specific fields.


```bash
> db.birds.replaceOne({
  _id: ObjectId('6286809e2f3fa87b7d86dccd')
},
  {
    "common_name": "Blue Jay",
    "scientific_name": "Cyanocitta cristata",
    "wingspan_cm": 43.18,
    "habitat": ["urban areas", "farms", "grassland"],
    "diet": ["seeds", "insects", "fruit"]
  }
);

```

The replaceOne() method in MongoDB is recommended to use when you want to replace an entire document with a new one. This method is useful in cases where the current document has become outdated or incorrect and needs to be entirely replaced with a new and updated version.

For example, consider a collection of customer records. If a customer's contact information changes, you can use the replaceOne() method to replace the entire document containing the outdated information with a new document containing the updated information.

In general, the replaceOne() method is recommended to use when you need to replace an entire document, rather than updating specific fields in a document. It's also a good option when you need to replace a document with a completely different document that has a different structure or different fields.

### Update a document

The updateOne() method, is used to update specific fields of a document rather than replacing the entire document. The method takes three arguments:

- A filter that specifies the document to be updated
- An update statement that specifies the changes to be made to the document
- Options that control how the update is performed

The updateOne() method allows you to modify specific fields in a document, rather than replacing the entire document with a new one.

```bash
> db.birds.updateOne({
  _id: ObjectId('6268413c613e55b82d7065d2')
},
  {
    $set: {
      'tags': ['geese', 'herbivore', 'migration']
    }
  },
  {
    upsert: true
  }
);

> db.birds.updateOne({
  _id: ObjectId('6268471e613e55b82d7065d7')
},
  {
    $push: {
      'diet': ["newts", "opossum", "skunks", "squirrels"]
    }
  },
  {
    upsert: true
  }
);

> db.birds.updateOne({
  _id: ObjectId('6268471e613e55b82d7065d7')
},
  {
    $push: {
      'diet': {
        $each: ["newts", "opossum", "skunks", "squirrels"]
      }
    }
  },
  {
    upsert: true
  }
);
```

The upsert: true option in MongoDB is used with the updateOne() or updateMany() methods to create a new document if no documents are found that match the specified filter.

When you perform an update operation on a MongoDB collection, the operation updates the existing documents that match the specified filter. If no documents match the filter, the operation has no effect.

By setting the upsert: true option, you tell MongoDB to create a new document if no documents match the specified filter. The new document is created using the update statement specified in the method.

For example, consider a collection of customer records. If you want to add a new customer to the collection, but you're not sure if the customer already exists, you can use the updateOne() method with the upsert: true option. If the customer already exists, the method updates the existing document. If the customer does not exist, the method creates a new document with the specified update statement.

In summary, the upsert: true option in MongoDB is used to ensure that an update operation creates a new document if no documents match the specified filter. This option is useful when you want to ensure that a document exists in a collection, regardless of whether or not it already exists.

### MongoDB Collection Modification

MongoDB provides several methods for modifying a collection, including:

findAndModify(): finds and updates a single document, and returns the document as it was before the update.
updateMany(): updates all documents that match the filter, and returns the document as it was before the update.
deleteOne(): deletes a single document that matches the filter, and returns the document as it was before the update.
deleteMany(): deletes all documents that match the filter, and returns the document as it was before the update.

### Find and modify

The findAndModify() method allows you to find and update a single document within a collection. The method requires a query and update argument, which specify the conditions for finding and updating the document, respectively. The optional new argument is a boolean <b>that specifies whether the method should return the updated or original document</b>. The following code finds and updates the document in the birds collection that has the common_name of "Blue Jay", and increments the sightings_count by 1.

```bash
> db.birds.findAndModify({
  query: {
    'common_name': 'Blue Jay'
  },
  update: {
    $inc: {
      sightings_count: 1
    }
  },
  new: true
})
```
### updateMany() Method

The updateMany() method allows you to update all documents that match a certain filter. The method requires a filter and update argument, which specify the conditions for finding and updating the documents, respectively. The following code updates all documents in the birds collection that have a common_name of "Blue Jay" or "Grackle", and sets the last_seen field to a specific date.

```bash
> db.birds.updateMany({
  'common_name': {
    $in: ['Blue Jay', 'Grackle']
  }
},
  {
    $set: {
      last_seen: ISODate('2023-01-30')
    }
  }
);
```
### deleteOne() Method

The deleteOne() method allows you to delete a single document that matches a certain filter. The method requires a filter argument, which specifies the conditions for finding the document to delete. The following code deletes a single document in the birds collection with a specific _id.

```bash
> db.birds.deleteOne({
  _id: ObjectId('62cddf53c1d62bc45439bebf')
})
```
### deleteMany() Method

The deleteMany() method allows you to delete all documents that match a certain filter. The method requires a filter argument, which specifies the conditions for finding the documents to delete. The following code deletes all documents in the birds collection with a sightings_count less than or equal to 10.

```bash
> db.birds.deleteMany({
  sightings_count: {
    $lte: 10
  }
})
```

### MongoDB projection, sort and count operations

This document provides examples of using MongoDB's find() method to retrieve documents from a sales collection, and using the sort() method to sort the results.

## Sort

You can sort the documents in the result set using the sort() method. The sort() method takes an object with field names as keys and the sort direction as values (1 for ascending and -1 for descending). For example, to sort the sales by saleDate in descending order:

```bash
> db.sales.find({}).sort({ saleDate: -1 })
```

To sort the sales where couponUsed is true by couponUsed in ascending order:
```bash
> db.sales.find({ couponUsed: true }).sort({})
```

Or: 
```bash
> db.sales.find({}).sort({ couponUsed: 1 })
```

## Projection

The find() method also supports limiting the fields to return for all matching documents using a projection. Inclusions and exclusions are both supported (1 for inclusion and 0 for exclusion), but not both in the same projection. The _id field is the only field that can be excluded from the projection.

For example, to return all data except the purchaseMethod, customer information, and whether a coupon was used:

```bash
> db.sales.find({
  storeLocation: { $in: ['Seattle', 'New York'] }
}, {
  customer: 0, couponUsed: 0, purchaseMethod: 0
})

```

## Count Documents

To count the number of documents in the sales collection:

```bash
> db.sales.countDocuments()
```

To count the number of documents where couponUsed is true:

```bash
> db.sales.countDocuments({ couponUsed: true })
```

You can also limit the number of documents returned using the limit option:

```bash
> db.sales.countDocuments({ couponUsed: true }, { limit: 10 })
```

Note that countDocuments() is an efficient alternative to the count() method (deprecated but still in use), which returns the same result, but countDocuments() is faster for large collections.

#### MongoDB Aggregation

MongoDB provides a powerful aggregation framework for performing complex data transformations and analysis. In MongoDB, aggregation operations process data records and return <b>computed results. The data can be filtered, sorted, grouped, reshaped and analyzed using aggregation.</b>

### Stages of Aggregation

* $match: Filters the documents to pass only the documents that match the specified condition(s) to the next stage in the pipeline.
* $group: Groups the documents by some specified expression and computes the count of documents in each distinct group.
* $sort: Sorts the documents in ascending or descending order based on the specified field.
* $limit: Limits the number of documents to be passed to the next stage in the pipeline.
* $project: Reshapes each document in the stream by adding new fields or removing existing fields and returns the documents with the requested fields to the next stage in the pipeline.
* $set: Adds a new field to the documents.
* $count: Counts the number of documents that pass through the stage.
* $out: Writes the documents that are returned by the aggregation pipeline to a specified collection.

## Examples of usage

Here are some examples of how to use these stages in the MongoDB aggregation framework.

```bash
> db.sightings.aggregate([
  {
    $match: {
      species_common: 'Eastern Bluebird'
    }
  },
  {
    $group: {
      _id: '$location.coordinates',
      number_of_sightings: { $count: {} }
    }
  },
  {
    $sort: {
      number_of_sightings: -1
    }
  },
  {
    $limit: 3
  }
])

```

The above code performs a series of operations on the sightings collection. It first filters the documents to only those where species_common is "Eastern Bluebird". Then it groups the documents by the location.coordinates field and computes the count of each group. Finally, it sorts the groups based on the number_of_sightings field in descending order and limits the number of documents to 3.

## Reshaping Data

```bash
> db.sightings.aggregate({
  $project: {
    _id: 0,
    species_common: 1,
    'location.coordinates': 1
  }
})
```
The above code reshapes the documents in the sightings collection by selecting only the species_common, location.coordinates fields and excluding the _id field.

## Adding a New Field

```bash
> db.birds.aggregate({
  $set: {
    class: 'Bird'
  }
})
```

## Filtering with Multiple Conditions

```bash
> db.sightings.aggregate([
  {
    $match: {
      date: {
        $gte: ISODate('2022-01-01T00:00:00.0Z'),
        $lt: ISODate('2023-01-01T00:00:00.0Z')
      }
    }
  },
  {
    $out: 'sightings_2022'
  }
])
```

$match: The first stage of the pipeline filters the documents in the sightings collection based on the date field. It only passes documents to the next stage of the pipeline if the date field is greater than or equal to '2022-01-01T00:00:00.0Z' and less than '2023-01-01T00:00:00.0Z'.

$out: The second stage of the pipeline writes the filtered documents from the previous stage to a new collection named sightings_2022. If this collection does not exist, it will be created. If it already exists, it will be overwritten. The $out stage must be the last stage in the pipeline.

#### Indexes in MongoDB

Indexes play a crucial role in improving the performance of queries in MongoDB. They are special data structures that store a small portion of the data in an easy-to-traverse form. <b>Indexes reduce I/O (input/output) operations and enable faster and more efficient searching.</b>

## Key Take Aways

* Indexes are used to improve and speed up the performance of queries.
* Reduces I/O (input/output) operations.
* They are special data structures that store a small portion of the data set in an easy to traverse form.
* Ordered and easy to search in an efficient way.
* They point to the location of the data in the collection.
* INDEXES IMPROVE QUERY PERFORMANCE AT THE COST OF WRITE PERFORMANCE.

Without Indexes | With Indexes
| :--- | ---:
MongoDB will scan every document in the collection to find the matching documents.  | Sorts the documents in memory.
Mongo only fetches the documents identified by the index.  | Faster computed results -> Less memory loss.

### How Indexes Work

<b>Without indexes, MongoDB will scan every document in the collection to find the matching documents and sort the results in memory. </b> With indexes, MongoDB only fetches the documents identified by the index and sorts the documents in memory, resulting in faster query performance.

Every collection in MongoDB has one default index, called _id, but we can also create our own indexes. However, it's important to note that adding indexes can improve query performance but can slow down write performance, so it's crucial to delete any indexes that are no longer needed.

### Common Index Types

The most common index types in MongoDB are:

* Single field index
* Multikey index (for arrays)
* Compound index
* Text index
* Geospatial index

### Creating an Index

Here's an example of creating an index:

```bash
> db.transfers.createIndex({ from_account: 1 }, { unique: true })
```

In this example, a single field index is created on the from_account field, and <b>the unique option is used to enforce uniqueness.</b>

### Explaining a Query

We can use the explain() method to determine the indexes being used for a query. For example:

```bash
> db.accounts.explain().find({ account_id: "MDB829000996" })
```

### Compound Indexes

Compound indexes are multikey indexes that are created on multiple fields. They allow you to index more than one field in a collection, enabling you to search and sort your data based on multiple fields at once. By using a compound index, you can perform complex queries that filter your data based on the values of multiple fields, which can greatly improve the performance of your queries. In MongoDB, you can create a compound index by passing an array of fields to the createIndex() method. The order of the fields in the array matters, as it will determine the order in which the data will be sorted in the index.

For example, if you have a collection of bank transactions with fields account_id, date, and amount, you can create a compound index on both account_id and date fields to support queries that search for transactions by both the account and a date range.

```bash
> db.accounts.createIndex({ transfers_complete: 1 })
or
> db.transactions.createIndex({account_id: 1, date: 1})
```

In this example, a compound index is created on the transfers_complete field. The results of a query that uses this index are much faster compared to a query without the index.

```bash
> db.accounts.explain().find({ transfers_complete: { $in: ["TR617907396"] } })
```

## Logic and order

The recommended order of indexed fields in a compound index is Equality, Sort, and Range. Equality fields are used to determine which documents match the query, the Sort field is used to determine the order of the documents, and the Range field is used to determine which documents to include in the result set.

Example:

```bash
> db.accounts.createIndex({ account_holder: 1, balance: 1, account_type: 1 })
> db.accounts.explain().find({ account_holder: "Andrea", balance: { $gt: 5 } }, { account_holder: 1, balance: 1, account_type: 1, _id: 0 }).sort({ balance: 1 })
```

It is important to note that deleting an index that is the only index on a field will affect the performance of the query. To delete an index, use the dropIndex() method. However, it is not possible to delete the default index on the _id field. To avoid the performance impact of deleting an index, you can hide the index before deleting it by using the hideIndex() method.

Example:

```bash
> db.accounts.hideIndex("account_holder_1")
> db.accounts.dropIndex("account_holder_1")
```

#### MongoDB Atlas Search

MongoDB Atlas Search is a fully managed search service that provides fast, highly available, and scalable search experience for MongoDB data. It is based on the Lucene search engine, and allows you to perform complex search operations on your MongoDB data with ease.

### Search Indexes

Search indexes specify how records are referenced for relevance-based search. They are used to tokenize and index all fields in a search index except for the _id field, boolean fields, and timestamps. The following is an example of a search index definition in MongoDB Atlas Search:

```bash
> {
  "name": "sample_supplies-sales-dynamic",
  "searchAnalyzer": "lucene.standard",
  "analyzer": "lucene.standard",
  "collectionName": "sales",
  "database": "sample_supplies",
  "mappings": {
      "dynamic": true
  }
}
```

The easiest way to create a search index is directly on mongodb atlas. Let's see it! Another way is to insert the following command through your terminal:

```bash
> atlas clusters search indexes create --clusterName myAtlasClusterEDU -f /app/search_index.json
```

And you can list the search indexes in a collection by running the following command:

```bash
> atlas clusters search indexes list --clusterName myAtlasClusterEDU --db sample_supplies --collection sales
```

### Querying with MongoDB Atlas Search

MongoDB Atlas Search provides a $search operator to perform search operations on the indexed data. The following is an example of a query that performs a search operation:

```bash
> db.sales.aggregate([
  {
    $search: {
      index: 'sample_supplies-sales-dynamic',
      text: {
        query: 'notepad', path: { 'wildcard': '*' }
      }
    }
  },
  {
    $set: {
      score: { $meta: "searchScore" }
    }
  }
])
```

### MongoDB Relationship Types and Schema Design

In MongoDB, there are several types of relationships between documents:

* One-to-one: One document in one collection references one document in another collection.
* One-to-many: One document in one collection references many documents in another collection.
* Many-to-many: Many documents in one collection reference many documents in another collection.

You can implement these relationships in MongoDB using either embedded documents or references.

## One-to-One Relationships

One-to-one relationships occur when one document in a collection references one document in another collection. In this case, the referencing document is embedded within the referenced document. For example, let's say we have two collections, users and profiles. Each user has a profile, and each profile belongs to only one user. The users collection would look like this:

```bash
> {
    "_id": ObjectId("5f9f11f1b2c8ea71d80c3214"),
    "email": "john.doe@example.com",
    "password": "hashedPassword",
    "profileId": ObjectId("5f9f11f1b2c8ea71d80c3215")
}
```

And the profiles collection would look like this:

```bash
> {
    "_id": ObjectId("5f9f11f1b2c8ea71d80c3215"),
    "firstName": "John",
    "lastName": "Doe",
    "birthday": ISODate("1990-01-01T00:00:00Z"),
    "address": "123 Main St, Anytown USA 12345"
}
```
In this example, the users collection has a field profileId which is a reference to the _id field of the corresponding document in the profiles collection. This creates a one-to-one relationship between the users and profiles collections.


## One-to-Many Relationships

One-to-many relationships occur when one document in a collection references multiple documents in another collection. This can be achieved using references, where the _id of the referenced document is saved in the referencing document. For example:

```bash
> const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
});

const postSchema = new mongoose.Schema({
  title: { type: String, required: true },
  content: { type: String, required: true },
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
});

const User = mongoose.model('User', userSchema);
const Post = mongoose.model('Post', postSchema);

```

In this example, one User document can have many Post documents, but each Post document only belongs to one User document. This is achieved by storing the _id of the User document as the value of the author field in the Post document. The ref option specifies the name of the model that the ObjectId refers to.

This type of relationship is useful in scenarios where one item, such as a user, is associated with multiple related items, such as posts. By using a one-to-many relationship, you can avoid duplicating data and keep related data organized in separate collections.

## Many-to-Many Relationships

Many-to-many relationships occur when many documents in one collection reference many documents in another collection. This can be achieved using references and embedding, depending on the specific use case.

```bash
> // Students collection
{
  "_id": ObjectId("5f2a1b5c06cfc867ed63ccef"),
  "name": "John Doe",
  "enrollments": [
    ObjectId("5f2a1b5c06cfc867ed63ccf0"),
    ObjectId("5f2a1b5c06cfc867ed63ccf1"),
    ObjectId("5f2a1b5c06cfc867ed63ccf2"),
  ]
}

// Courses collection
{
  "_id": ObjectId("5f2a1b5c06cfc867ed63ccf0"),
  "name": "Math 101",
  "students": [
    ObjectId("5f2a1b5c06cfc867ed63ccef"),
    ObjectId("5f2a1b5c06cfc867ed63cceg"),
    ObjectId("5f2a1b5c06cfc867ed63cceh"),
  ]
}
```

In this example, one student can enroll in multiple courses and one course can have multiple students. The enrollments array in the students collection stores the _id of the courses that the student has enrolled in. Similarly, the students array in the courses collection stores the _id of the students enrolled in the course.

## Embedding

Embedding is used when you have one-to-one or one-to-many relationships between documents. Advantages of embedding include single query to retrieve the data and single operation to update or delete the data. However, disadvantages include data duplication and large document sizes.

## Referencing

Referencing is used when you have many-to-many relationships between documents. Advantages of referencing include no duplication of data and smaller document sizes. However, disadvantages include multiple queries to retrieve the data.

## Common Schema Anti-Patterns

1. Massive arrays
2. Massive number of collections
3. Bloated documents
4. Unnecessary indexes
5. Queries without indexes
6. Data that is accessed together but is stored

#### MongoDB Transactions

MongoDB is a document-oriented database that provides support for ACID-compliant transactions. ACID (Atomicity, Consistency, Isolation, and Durability) is a set of properties that guarantee the integrity and reliability of a database.

MongoDB transactions are supported on replica sets and sharded clusters and are available on MongoDB 4.0 and later, as well as on MongoDB Atlas and MongoDB Compass.

### Atomicity

Atomicity in MongoDB transactions means that all operations in a transaction are either completed or none are completed. For example, in an update operation, either the update happens or it doesn't. It's all or nothing.

```bash
> db.products.updateOne({
  _id: 1
}, {
  $set: {
    quantity: 100,
    details: {
      model: "14Q3",
      make: "Ford"
    },
    tags: [ "cars", "sedans" ],
  }
})
```

### Consistency

Consistency in MongoDB transactions ensures that the database is always in a valid state. This means that all transactions maintain the constraints and rules of the database, such as unique keys, referential integrity, and data type restrictions.

### Isolation

Isolation in MongoDB transactions ensures that transactions are isolated from each other. This means that the execution of one transaction does not affect the execution of another transaction.

### Multi-Document Transactions

MongoDB transactions support multi-document transactions, allowing updates to multiple documents in a single operation. For example, you can use the updateMany method to update multiple documents in a single operation:

```bash
> db.products.updateMany({
  quantity: { $lt: 50 }
}, {
  $set: {
    "instock": true
  }
})
```

Additionally, you can use the bulkWrite method to update multiple documents in a single operation and ensure that all updates are successful:

```bash
> db.products.bulkWrite([
  {
    updateOne: {
      filter: { _id: 1 },
      update: {
        $set: {
          quantity: 100,
          details: {
            model: "14Q3",
            make: "Ford"
          },
          tags: [ "cars", "sedans" ],
        }
      }
    }
  },
  {
    updateMany: {
      filter: { quantity: { $lt: 50 } },
      update: {
        $set: {
          "instock": true
        }
      }
    }
  }
])

```

## Transaction Runtime

It's important to note that transactions in MongoDB have a maximum runtime of less than one minute after the first write operation. If a transaction exceeds the maximum runtime, it will be automatically aborted.

#### Starting and Ending a Transaction

This code demonstrates the use of MongoDB transactions. It includes three examples of how to use transactions to update collections within a database.

## Example 1

```bash
> const session = db.getMongo().startSession();
session.startTransaction();
const account = session.getDatabase('bank').getCollection('accounts');
account.updateOne({ account_id: 'MDB653115886' }, { $inc: { balance: -10 } });
account.updateOne({ account_id: 'MDB000000007' }, { $inc: { balance: 10 } });
session.commitTransaction();
session.endSession();
```

In this example, a transaction is started with startTransaction(). The database bank and its collection accounts are accessed and updated using the updateOne() method. The first update decreases the balance field of account_id 'MDB653115886' by 10 and the second update increases the balance field of account_id 'MDB000000007' by 10. The transaction is committed with commitTransaction() and ended with endSession().

## Example 2

```bash
> const sessionTwo = db.getMongo().startSession();
sessionTwo.startTransaction();
const accountTwo = session.getDatabase('bank').getCollection('accounts');
accountTwo.updateOne({ account_id: 'MDB653115886' }, { $inc: { balance: -10 } });
sessionTwo.abortTransaction();
accountTwo.updateOne({ account_id: 'MDB000000007' }, { $inc: { balance: 10 } });
sessionTwo.endSession();
```

This example is similar to the first example, but the transaction is aborted with abortTransaction(). The updateOne() method for the account_id 'MDB000000007' is never executed, and its balance field remains unchanged. The transaction is ended with endSession().

## Example 3

```bash
> const mySession = db.getMongo().startSession()

mySession.startTransaction()

const myAccount = mySession.getDatabase('bank').getCollection('accounts')

myAccount.insertOne({
  account_id: "MDB454252264",
  account_holder: "Florence Taylor",
  account_type: "savings",
  balance: 100.0,
  transfers_complete: [],
  last_updated: new Date()
})
myAccount.updateOne( { account_id: "MDB963134500" }, {$inc: { balance: -100.00 }})

mySession.commitTransaction()

```

In this example, a new account is inserted into the accounts collection with insertOne(). The balance field of an existing account_id 'MDB963134500' is also decreased by 100 with updateOne(). The transaction is committed with commitTransaction() and does not require an endSession() call, as the session is automatically ended when the transaction is committed or aborted.

## Author <!-- omit in toc -->

[Jose Valenzuela](https://github.com/Jose1i1o)

## License <!-- omit in toc -->

[MIT](https://choosealicense.com/licenses/mit/)
