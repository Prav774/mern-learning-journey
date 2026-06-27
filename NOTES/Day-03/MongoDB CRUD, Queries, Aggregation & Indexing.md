# DAY 3 — MongoDB CRUD, Queries, Aggregation & Indexing
# CRUD Operations

CRUD stands for:

* Create
* Read
* Update
* Delete

---

## Create

### insertOne()

```javascript
db.products.insertOne({
  name: "Laptop",
  price: 50000,
  category: "Electronics",
  stock: 10
});
```

### insertMany()

```javascript
db.products.insertMany([
  {
    name: "Laptop",
    price: 50000,
    category: "Electronics",
    stock: 10
  },
  {
    name: "Phone",
    price: 25000,
    category: "Electronics",
    stock: 20
  }
]);
```

---

## Read

### find()

Returns multiple documents.

```javascript
db.products.find();
```

### findOne()

Returns the first matching document.

```javascript
db.products.findOne({
  name: "Laptop"
});
```

---

## Update

### updateOne()

```javascript
db.products.updateOne(
  { name: "Laptop" },
  {
    $set: {
      price: 55000
    }
  }
);
```

### updateMany()

```javascript
db.products.updateMany(
  { category: "Electronics" },
  {
    $inc: {
      stock: 5
    }
  }
);
```

---

### $push

Adds element into an array.

```javascript
db.products.updateOne(
  { name: "Laptop" },
  {
    $push: {
      tags: "New"
    }
  }
);
```

---

### $pull

Removes element from an array.

```javascript
db.products.updateOne(
  { name: "Laptop" },
  {
    $pull: {
      tags: "New"
    }
  }
);
```

---

## Delete

### deleteOne()

```javascript
db.products.deleteOne({
  name: "Laptop"
});
```

### deleteMany()

```javascript
db.products.deleteMany({
  category: "Accessories"
});
```

---

# Query Operators

## $eq

```javascript
db.products.find({
  price: {
    $eq: 50000
  }
});
```

---

## $gt

```javascript
db.products.find({
  price: {
    $gt: 10000
  }
});
```

---

## $lt

```javascript
db.products.find({
  price: {
    $lt: 10000
  }
});
```

---

## $in

```javascript
db.products.find({
  category: {
    $in: ["Electronics", "Accessories"]
  }
});
```

---

## $or

```javascript
db.products.find({
  $or: [
    { price: { $lt: 1000 } },
    { stock: { $gt: 15 } }
  ]
});
```

---

## $and

```javascript
db.products.find({
  $and: [
    { category: "Electronics" },
    { stock: { $gt: 5 } }
  ]
});
```

---

## Query Nested Fields

```javascript
db.users.find({
  "address.city": "Coimbatore"
});
```

---

# Projection

Select only required fields.

```javascript
db.products.find(
  {},
  {
    name: 1,
    price: 1,
    _id: 0
  }
);
```

Output:

```json
{
  "name": "Laptop",
  "price": 50000
}
```

---

# Sorting

## Ascending

```javascript
db.products.find().sort({
  price: 1
});
```

---

## Descending

```javascript
db.products.find().sort({
  price: -1
});
```

---

# Limit

```javascript
db.products.find().limit(5);
```

---

# Skip

```javascript
db.products.find().skip(5);
```

---

# Pagination

Formula:

```javascript
skip = (page - 1) * limit;
```

Example:

```javascript
const page = 2;
const limit = 10;

db.products.find()
  .skip((page - 1) * limit)
  .limit(limit);
```

Returns records 11–20.

---

# Aggregation Pipeline

Aggregation is used for:

* Analytics
* Reports
* Data Transformation
* Grouping

Pipeline Concept:

```text
Input Data
    ↓
$match
    ↓
$group
    ↓
$sort
    ↓
Output
```

---

## Syntax

```javascript
db.collection.aggregate([
  { stage1 },
  { stage2 },
  { stage3 }
]);
```

---

## $match

Filters documents.

```javascript
db.products.aggregate([
  {
    $match: {
      category: "Electronics"
    }
  }
]);
```

---

## $group

Groups documents.

```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      count: {
        $sum: 1
      }
    }
  }
]);
```

---

## $project

```javascript
db.products.aggregate([
  {
    $project: {
      name: 1,
      price: 1,
      _id: 0
    }
  }
]);
```

---

## $sort

```javascript
db.products.aggregate([
  {
    $sort: {
      price: -1
    }
  }
]);
```

---

## $limit

```javascript
db.products.aggregate([
  {
    $limit: 5
    }
]);
```

---

## Complete Aggregation Example

```javascript
db.products.aggregate([
  {
    $match: {
      price: { $gt: 1000 }
    }
  },
  {
    $group: {
      _id: "$category",
      totalProducts: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      totalProducts: -1
    }
  }
]);
```

---

# $lookup (MongoDB Join)

Orders Collection:

```javascript
{
  _id: 1,
  customerId: 101,
  amount: 5000
}
```

Customers Collection:

```javascript
{
  customerId: 101,
  name: "Praveen"
}
```

---

## Join Query

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "customerId",
      as: "customerDetails"
    }
  }
]);
```

---

## SQL Equivalent

```sql
SELECT *
FROM Orders o
JOIN Customers c
ON o.customerId = c.customerId;
```

---

# Indexing

## What is an Index?

An index is a special data structure that allows MongoDB to locate data faster.

Without Index:

```text
Full Collection Scan
```

With Index:

```text
Direct Document Lookup
```

---

## Create Single Field Index

```javascript
db.products.createIndex({
  name: 1
});
```

---

## Compound Index

```javascript
db.products.createIndex({
  category: 1,
  price: -1
});
```

---

## Why Use Indexes?

* Faster Queries
* Faster Sorting
* Better Read Performance

---

## Tradeoffs

Advantages:

* Fast Reads
* Fast Searches

Disadvantages:

* More Storage
* Slower Inserts
* Slower Updates
* Slower Deletes

Reason:

Every index must also be updated whenever data changes.

---

## View Indexes

```javascript
db.products.getIndexes();
```

---

## Delete Index

```javascript
db.products.dropIndex("name_1");
```

---

# explain()

Used to analyze query execution.

```javascript
db.products.find({
  category: "Electronics"
}).explain("executionStats");
```

Useful to check:

* Index Usage
* Query Cost
* Collection Scan

---

# Transactions

Transactions ensure:

```text
All Operations Succeed
OR
All Operations Fail
```

---

## Example

Bank Transfer

```text
Account A -1000

Account B +1000
```

If any operation fails:

```text
Rollback Entire Transaction
```

---

## Transaction Flow

```text
Start Transaction
      ↓
Perform Operations
      ↓
Commit Transaction

OR

Abort Transaction
```

---

# Practical Task

Create a Products collection and perform:

1. Insert documents
2. Read documents
3. Update documents
4. Delete documents
5. Pagination using skip and limit
6. Create index on category
7. Run aggregation

Aggregation Task:

```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      totalProducts: {
        $sum: 1
      }
    }
  }
]);
```

Expected Output:

```json
[
  {
    "_id": "Electronics",
    "totalProducts": 5
  },
  {
    "_id": "Accessories",
    "totalProducts": 3
  }
]
```

---

# Interview Questions

## 1. Difference between find() and findOne()?

find()

* Returns Cursor
* Can return multiple documents

findOne()

* Returns Single Document
* Returns first matching document

---

## 2. How do you update a nested field?

```javascript
db.users.updateOne(
  {},
  {
    $set: {
      "address.city": "Chennai"
    }
  }
);
```

---

## 3. What is Aggregation Pipeline?

A framework that processes documents through multiple stages.

Common stages:

* $match
* $group
* $project

---

## 4. Difference between $match and $project?

$match

* Filters documents

$project

* Selects/transforms fields

---

## 5. What is an Index?

A data structure that speeds up data retrieval.

---

## 6. What is the tradeoff of too many indexes?

* Increased storage
* Slower writes
* Slower updates
* Slower deletes

---

## 7. How would you implement pagination?

```javascript
db.products.find()
.skip((page - 1) * limit)
.limit(limit);
```

---

## 8. What does $lookup do?

Performs join-like operations between collections.

Equivalent to SQL JOIN.

---

## 9. What is a Compound Index?

An index created on multiple fields.

Example:

```javascript
{
  category: 1,
  price: -1
}
```

---

## 10. How do you delete multiple documents?

```javascript
db.products.deleteMany({
  category: "Accessories"
});
```

---

# Quick Revision

```text
insertOne()     → Create
insertMany()    → Create

find()          → Read
findOne()       → Read

updateOne()     → Update
updateMany()    → Update

deleteOne()     → Delete
deleteMany()    → Delete

$match          → Filter
$group          → Group
$project        → Select Fields
$lookup         → Join Collections

Index           → Faster Reads
Too Many Indexes→ Slower Writes

Pagination      → skip() + limit()

Aggregation     → Analytics & Reports
Transactions    → Data Consistency
```
