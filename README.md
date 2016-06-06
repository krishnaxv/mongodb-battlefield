# JSON

> What is JSON?

# MongoDB

### Introduction

> MongoDB is an open-source document database that provides high performance, high availability, and automatic scaling. MongoDB is written in C++.

MongoDB works on concept of collection and document.

**Database**

Database is a physical container for collections. A single MongoDB server typically has multiple databases.

**Collection**

Collection is a group of MongoDB documents. It is the equivalent of an RDBMS table.

- Collections do not enforce a schema.
- Documents within a collection can have different fields. Typically, all documents in a collection are of similar or related purpose.

**Document**

A document is a set of key-value pairs.

Documents have dynamic schema. Dynamic schema means that documents in the same collection do not need to have the same set of fields or structure, and common fields in a collection's documents may hold different types of data.

> Sample blog document

```javascript
{
   _id: ObjectId(7df78ad8902c)
   title: "MongoDB Traininf",
   description: "MongoDB is NoSQL database.",
   by: "krishnaxv",
   url: "https://github.com/krishnaxv/mongodb-battlefield",
   tags: ["MongoDB", "Database", "NoSQL"],
   likes: 7,
   comments: [
      {
         user: "John",
         message: "MongoDB is great.",
         dateCreated: new Date(2016, 5, 20, 2, 15),
         like: 0
      },
      {
         user: "Michael",
         message: "Relational vs NoSQL.",
         dateCreated: new Date(2016, 5, 25, 7, 45),
         like: 5
      }
   ]
}
```

### RDBMS terminology relationship with MongoDB

RDBMS | MongoDB
----- | -------
Database | Database
Table | Collection
Tuple/Row | Document
column | Field
Primary Key | Primary Key (Default key `_id` provided by MongoDB itself)

### Advantages of MongoDB over RDBMS

1. Schema less: MongoDB is document database in which one collection holds different different documents. Number of fields, content and size of the document can differ from one document to another.
2. Structure of a single object is clear
3. No complex joins
4. Deep query-ability. MongoDB supports dynamic queries on documents using a document-based query language that's nearly as powerful as SQL
5. Tuning
6. Ease of scale-out: MongoDB is easy to scale
7. Uses internal memory for storing the (windowed) working set, enabling faster access of data

**Why you should use MongoDB?**

1. Document Oriented Storage: Data is stored in the form of JSON style documents
2. Index on any attribute
3. Replication & High Availability
4. Auto-Sharding
5. Rich Queries
6. Fast In-Place Updates

### Data Modeling

### Getting Started

**MongoDB Help**

To get list of commands type `db.help()` in MongoDB client.

**MongoDB Statistics**

To get statistics about database, type `db.stats()`

**`use` Command**

MongoDB `use DATABASE_NAME` is used to create database. The command will create a new database, if it doesn't exist otherwise it will return the existing database.

Syntax:

```javascript
use Movies
```

To check your currently selected database, use the command `db`

```javascript
db
```

To check database list, use the command `show dbs`

```javascript
show dbs
```

`Movies` database is still not available in the list. To display database you need to insert at least one document into it.

```javascript
db.MoviesInfo.insert({
  "name": "Avatar"
})
```

*In MongoDB, default database is test. If you didn't create any database, then collections will be stored in `test` database.*

**dropDatabase() Method**

MongoDB db.dropDatabase() command is used to drop a existing database.

Syntax:

```javascript
db.dropDatabase()
```

This will delete the selected database.

*If you have not selected any database, then it will delete default `test` database.*

**createCollection() Method**

MongoDB `db.createCollection(name, options)` is used to create collection.

`name` is name of collection to be created. `options` is a document and used to specify configuration of collection.

Options parameter is optional, so you need to specify only name of the collection.

List of `options`

Field |	Type | Description
----- | ---- | -----------
capped | Boolean	| (Optional) If true, enables a capped collection. Capped collection is a collection fixed size collection that automatically overwrites its oldest entries when it reaches its maximum size. **If you specify true, you need to specify size parameter also.**
autoIndexID	| Boolean |	(Optional) If true, automatically create index on `_id` field. Default value is false.
size | number |	(Optional) Specifies a maximum size in bytes for a capped collection. **If capped is true, then you need to specify this field also.**
max	| number | (Optional) Specifies the maximum number of documents allowed in the capped collection.

While inserting the document, MongoDB first checks size field of capped collection, then it checks max field.

Syntax:

```javascript
db.createCollection('MoviesInfo')
```

You can check the created collection by using the command `show collections`.

In MongoDB you don't need to create collection. MongoDB creates collection automatically, when you insert some document.

```javascript
db.MoviesInfo.insert({
  "name": "Avatar"
})
```

```javascript
show collections
```

**drop() Method**

`db.collection.drop()` is used to drop a collection from the database.

Syntax:

```javascript
db.MoviesInfo.drop()
```

`drop()` method will return true, if the selected collection is dropped successfully otherwise it will return false.

**insert() Method**

To insert data into MongoDB collection, you need to use MongoDB's insert() or save() method.

```javascript
db.MoviesInfo.insert({
  "name": "Movie A"
})
```

To insert multiple documents in single query, you can pass an array of documents in insert() command.

```javascript
db.MoviesInfo.insert([
  {
    "name": "Movie A"
  },
  {
    "name": "Movie B"
  }
])
```

**find() Method**

To query data from MongoDB collection, you need to use MongoDB's find() method.

Syntax:

```javascript
db.MoviesInfo.find()
```

find() method will display all the documents in a non-structured way.

**pretty() Method**

To display the results in a formatted way, you can use pretty() method.

Syntax:

```javascript
db.MoviesInfo.find().pretty()
```

Apart from find() method there is findOne() method, that returns only one document.

**RDBMS Where Clause Equivalents in MongoDB**

Operation | Syntax | Example | RDBMS Equivalent
--------- | ------ | ------- | ----------------
Equality | { key: value } | db.MoviesInfo.find({ "name": "Movie A" }).pretty() | WHERE name = 'Movie A'
Less Than | { key: { $lt: value }} | db.MoviesInfo.find({ "likes": { $lt: 50 }}).pretty() | WHERE likes < 50
Less Than Equals | { key: { $lte: value }} | db.MoviesInfo.find({ "likes": { $lte: 50 }}).pretty() | WHERE likes <= 50
Greater Than | { key: { $gt: value }} | db.MoviesInfo.find({ "likes": { $gt: 50 }}).pretty() | WHERE likes > 50
Greater Than Equals | { key: { $gte: value }} | db.MoviesInfo.find({ "likes": { $gte: 50 }}).pretty() | WHERE likes >= 50
Not Equals | { key: { $ne: value }} | db.MoviesInfo.find({ "likes": { $ne: 50 }}).pretty() | WHERE likes != 50

**AND in MongoDB**

In the find() method if you pass multiple keys by separating them by ',' then MongoDB treats it AND condition.

```javascript
db.MoviesInfo.find({ key1: value1, key2: value2 }).pretty()
```

For the above given example, equivalent WHERE clause will be `WHERE key1='value1' AND key2='value2'`. You can pass any number of key, value pairs in find clause.

**OR in MongoDB**

To query documents based on the OR condition, you need to use $or keyword.

```javascript
db.MoviesInfo.find(
  {
    $or: [
      { key1: value1 }, { key2: value2 }
    ]
  }
).pretty()
```

**Using AND and OR Together**

```javascript
db.MoviesInfo.find({
  "likes": {
    $gt: 10
  },
  $or: [
    {
      "name": "Movie A"
    },
    {
      "releaseYear": 2010
    }
  ]
}).pretty()
```

### Update

MongoDB's update() method is used to update document into a collection.

**update() Method**

The update() method updates values in the existing document.

```javascript
db.COLLECTION_NAME.update(SELECTION_CRITERIA, UPDATED_DATA)

db.MoviesInfo.update({ "name": "Movie A" }, {
  $set: {
    "name": "New Movie A"
  }
})
```

By default MongoDB will update only single document, to update multiple you need to set a parameter 'multi' to true.

```javascript
db.MoviesInfo.update({ "name": "Movie A" }, {
  $set: {
    "name": "New Movie A"
  }
}, { multi: true })
```

### Remove

**remove() Method**

MongoDB's remove() method is used to remove document from the collection. remove() method accepts two parameters. One is deletion criteria and second is justOne flag.

```javascript
db.COLLECTION_NAME.remove(DELETION_CRITERIA)

db.MoviesInfo.remove({ "name": "Movie A" })
```

**Remove only one**

If there are multiple records and you want to delete only first record, then set justOne parameter in remove() method.

```javascript
db.COLLECTION_NAME.remove(DELETION_CRITERIA, 1)
```

**Remove All documents**

If you don't specify deletion criteria, then MongoDB will delete all documents from the collection. **This is equivalent of SQL's truncate command**.

```javascript
db.COLLECTION_NAME.remove({})
db.COLLECTION_NAME.find()
```

### Projection

In MongoDB, projection meaning is selecting only necessary data rather than selecting whole of the data of a document. If a document has 5 fields and you need to show only 3, then select only 3 fields from them.

**find() Method**

MongoDB's find() method, explained in MongoDB Query Document accepts second optional parameter that is list of fields that you want to retrieve.

In MongoDB when you execute find() method, then it displays all fields of a document. To limit this you need to set list of fields with value 1 or 0. 1 is used to show the field while 0 is used to hide the field.

Syntax:

```javascript
db.COLLECTION_NAME.find({}, { KEY: 1 })
```

Following example will display the name of the movie.

```javascript
db.MoviesInfo.find({},{ "name": 1, _id: 0 })
```

*Note _id field is always displayed while executing find() method, if you don't want this field, then you need to set it as 0.*

**limit() Method**

To limit the records in MongoDB, you need to use limit() method. limit() method accepts one number type argument, which is number of documents that you want to displayed.

Syntax:

```javascript
db.COLLECTION_NAME.find().limit(NUMBER)
```

Following example will display only 2 documents.

```javascript
db.MoviesInfo.find({}, { "name": 1, _id: 0 }).limit(2)
```

If you don't specify number argument in limit() method then it will display all documents from the collection.

**skip() Method**

Apart from limit() method there is one more method skip() which also accepts number type argument and used to skip number of documents.

```javascript
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

Following example will display only 2 documents.

```javascript
db.MoviesInfo.find({}, { "name": 1, _id: 0 }).limit(2).skip(2)
```

Please note default value in skip() method is 0.

**sort() Method**

To sort documents in MongoDB, you need to use sort() method. sort() method accepts a document containing list of fields along with their sorting order. To specify sorting order 1 and -1 are used. 1 is used for ascending order while -1 is used for descending order

Syntax:

```javascript
db.COLLECTION_NAME.find().sort({ KEY: 1 })

db.MoviesInfo.find({}, { "name": 1, _id: 0 }).sort({ "releaseYear": -1 })
```

Note if you don't specify the sorting preference, then sort() method will display documents in ascending order.

### Indexing

**createIndex() Method**

Indexes support the efficient resolution of queries. To create an index you need to use createIndex() method of MongoDB.

Syntax:

```javascript
db.COLLECTION_NAME.createIndex({ KEY: 1 })

db.MoviesInfo.createIndex({ "name": 1 })
```

In createIndex() method you can pass multiple fields, to create index on multiple fields.

```javascript
db.MoviesInfo.createIndex({ "name": 1, "releaseYear": 1 })
```

### Aggregation

Aggregations operations process data records and return computed results. Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result.

$sum, $avg, $min, $max, $first, $last, $project, $match, $group, $sort, $skip, $limit, $unwind

### Usage with PyMongo
