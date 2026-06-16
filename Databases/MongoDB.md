# 🍃 MongoDB Cheatsheet

![MongoDB Logo](https://webassets.mongodb.com/_com_assets/cms/mongodb_logo1-76twgcu2dm.png)

---

# Table of Contents

1. [What is MongoDB?](#1-what-is-mongodb)
2. [Why MongoDB?](#2-why-mongodb)
3. [Relational vs NoSQL](#3-relational-vs-nosql)
4. [MongoDB Architecture](#4-mongodb-architecture)
5. [Core Concepts](#5-core-concepts)
6. [BSON](#6-bson)
7. [Documents](#7-documents)
8. [Collections](#8-collections)
9. [Databases](#9-databases)
10. [MongoDB Data Model](#10-mongodb-data-model)
11. [CRUD Overview](#11-crud-overview)
12. [Insert Operations](#12-insert-operations)
13. [Query Operations](#13-query-operations)
14. [Update Operations](#14-update-operations)
15. [Delete Operations](#15-delete-operations)
16. [Data Types](#16-data-types)
17. [Embedded Documents](#17-embedded-documents)
18. [References](#18-references)
19. [MongoDB Shell](#19-mongodb-shell)
20. [Installation](#20-installation)

---

# 1. What is MongoDB?

MongoDB is a popular open-source NoSQL document database designed for modern applications.

Instead of storing data in tables and rows, MongoDB stores data as JSON-like documents.

---

## Key Characteristics

```text id="x7m4wp"
Document Database
+
Schema Flexible
+
Horizontal Scaling
+
High Performance
+
Developer Friendly
```

---

## MongoDB Stores Data As

```text id="m5v8xt"
Database
    |
    v
Collection
    |
    v
Document
```

---

## Example Document

```json id="r8v2qt"
{
  "name": "John",
  "age": 30,
  "city": "London"
}
```

---

## Common Use Cases

```text id="p4m7wr"
Web Applications
+
Mobile Applications
+
IoT Platforms
+
E-Commerce
+
Analytics Systems
```

---

# 2. Why MongoDB?

Traditional databases can become difficult when application data changes frequently.

MongoDB provides flexibility and scalability.

---

## Traditional Database Problem

```text id="x6v3qt"
Schema Change
      |
      v
ALTER TABLE
      |
      v
Downtime Risk
```

---

## MongoDB Approach

```text id="m8v4xp"
Document
      |
      v
Flexible Structure
```

---

## Benefits

```text id="r7m2wt"
Flexible Schema
+
Fast Development
+
Easy Scaling
+
JSON Style Documents
```

---

## Example

Document 1:

```json id="n5v9wr"
{
  "name": "Alice"
}
```

---

Document 2:

```json id="q4m8xt"
{
  "name": "Bob",
  "phone": "123456789"
}
```

Both can exist in the same collection.

---

# 3. Relational vs NoSQL

MongoDB belongs to the NoSQL database category.

---

## Relational Database

```text id="x8m2wp"
Database
   |
   v
Tables
   |
   v
Rows
```

Examples:

```text id="r5v7qt"
MySQL
+
PostgreSQL
+
Oracle
```

---

## MongoDB

```text id="m4v8wr"
Database
   |
   v
Collections
   |
   v
Documents
```

---

## Comparison

| Feature | Relational DB | MongoDB    |
| ------- | ------------- | ---------- |
| Storage | Tables        | Documents  |
| Schema  | Fixed         | Flexible   |
| Scaling | Vertical      | Horizontal |
| Joins   | Common        | Limited    |
| Format  | Rows          | BSON       |

---

## Mapping

```text id="p7m3xt"
Table
   =
Collection

Row
   =
Document

Column
   =
Field
```

---

# 4. MongoDB Architecture

MongoDB follows a document-oriented architecture.

---

## Basic Architecture

```text id="n3v8wp"
Application
      |
      v
MongoDB Driver
      |
      v
MongoDB Server
      |
      v
Storage Engine
```

---

## Components

```text id="x5m9qt"
Client
+
Driver
+
mongod
+
Storage Engine
```

---

## MongoDB Server

```text id="r8v4wr"
mongod
```

Main database process.

---

## Client Access

```text id="m2v7xt"
Application
     |
     v
MongoDB Driver
     |
     v
MongoDB
```

---

# 5. Core Concepts

MongoDB is built on a few fundamental concepts.

---

## Hierarchy

```text id="q6m8wp"
Database
    |
    v
Collection
    |
    v
Document
    |
    v
Field
```

---

## Database

Container for collections.

---

## Collection

Container for documents.

---

## Document

Stores actual data.

---

## Field

Individual data element.

---

## Example

```json id="n7m4qt"
{
  "name": "Alice",
  "age": 25
}
```

---

# 6. BSON

MongoDB stores data internally as BSON.

BSON stands for Binary JSON.

---

## Why BSON?

```text id="x4v8wr"
Efficient Storage
+
Fast Processing
+
Rich Data Types
```

---

## JSON Example

```json id="m8v3xt"
{
  "name": "John"
}
```

---

## BSON Supports

```text id="q5m7wp"
Date
+
ObjectId
+
Binary Data
+
Decimal128
```

---

## Workflow

```text id="r9v2qt"
JSON
   |
   v
BSON
   |
   v
Storage
```

---

# 7. Documents

Documents are the basic unit of data in MongoDB.

---

## Example

```json id="p4m8wr"
{
  "_id": 1,
  "name": "John",
  "email": "john@example.com"
}
```

---

## Characteristics

```text id="x7m4qt"
Self Contained
+
Flexible
+
JSON Like
```

---

## Document Structure

```text id="n5v8xp"
Document
   |
   +-- Field
   +-- Field
   +-- Field
```

---

## Advantages

```text id="r3m9wp"
Easy To Read
+
Flexible Design
```

---

# 8. Collections

Collections store related documents.

---

## Similar To

```text id="m4v8qt"
Table

In Relational Databases
```

---

## Example

Collection:

```text id="q8m2wr"
users
```

Contains:

```json id="x6v4wp"
{
 "name":"John"
}
```

```json id="r5m9xt"
{
 "name":"Alice"
}
```

---

## Benefits

```text id="n7v3wr"
Flexible Schema
+
Easy Scaling
```

---

## Collection Structure

```text id="x4m8wr"
Collection
    |
    +-- Document
    +-- Document
    +-- Document
```

---

# 9. Databases

Databases contain collections.

---

## Example

```text id="m8v4qt"
ecommerce
```

Database.

---

Collections:

```text id="q4m8wr"
users
orders
products
payments
```

---

## Hierarchy

```text id="r8v2qt"
Database
     |
     +--- Collection
     |
     +--- Collection
```

---

## Benefits

```text id="x6m4wp"
Logical Separation
+
Organization
```

---

# 10. MongoDB Data Model

MongoDB uses a document data model.

---

## Example

```json id="n5v9xt"
{
  "name": "John",
  "email": "john@example.com",
  "address": {
    "city": "London"
  }
}
```

---

## Advantages

```text id="p7m3wr"
Natural Data Structure
+
Flexible Design
```

---

## Model

```text id="m4v8qt"
Application Object
        |
        v
MongoDB Document
```

---

# 11. CRUD Overview

CRUD operations are the foundation of MongoDB.

---

## CRUD

```text id="q5v7xt"
Create
Read
Update
Delete
```

---

## Mapping

```text id="r4m9wp"
Create → Insert

Read → Find

Update → Update

Delete → Delete
```

---

## Workflow

```text id="n7m4wr"
Application
      |
      v
CRUD Operation
      |
      v
MongoDB
```

---

# 12. Insert Operations

Used to create documents.

---

## Insert One

```javascript id="x4m8wr"
db.users.insertOne({
  name: "John",
  age: 30
})
```

---

## Insert Many

```javascript id="q6v3wp"
db.users.insertMany([
  { name: "John" },
  { name: "Alice" }
])
```

---

## Result

```text id="r5v8qt"
Documents Added
```

---

## Best Practice

```text id="n8m4xp"
Validate Input
Before Insert
```

---

# 13. Query Operations

Queries retrieve data.

---

## Find All

```javascript id="m3v7wr"
db.users.find()
```

---

## Find One

```javascript id="x7m2qt"
db.users.findOne()
```

---

## Query Example

```javascript id="p8m4wr"
db.users.find({
  age: 30
})
```

---

## Workflow

```text id="r7v3xt"
Query
   |
   v
MongoDB
   |
   v
Results
```

---

# 14. Update Operations

Updates modify existing documents.

---

## Update One

```javascript id="n4m8wp"
db.users.updateOne(
  { name: "John" },
  { $set: { age: 31 } }
)
```

---

## Update Many

```javascript id="x5v7qt"
db.users.updateMany(
 {},
 { $set: { active: true } }
)
```

---

## Common Operator

```text id="m8v2wr"
$set
```

---

## Purpose

```text id="r5v8wp"
Modify Existing Data
```

---

# 15. Delete Operations

Deletes documents.

---

## Delete One

```javascript id="n7m4xt"
db.users.deleteOne({
  name: "John"
})
```

---

## Delete Many

```javascript id="p4m9wr"
db.users.deleteMany({
  active: false
})
```

---

## Result

```text id="x8m3qt"
Documents Removed
```

---

# 16. Data Types

MongoDB supports many data types.

---

## Common Types

```text id="m5v8wr"
String
+
Number
+
Boolean
+
Date
+
Array
+
Object
```

---

## Example

```json id="r4v9xp"
{
  "name": "John",
  "age": 30,
  "active": true
}
```

---

## Advanced Types

```text id="n6m4qt"
ObjectId
+
Decimal128
+
Binary
```

---

# 17. Embedded Documents

Documents can contain other documents.

---

## Example

```json id="p7v3wr"
{
  "name": "John",
  "address": {
    "city": "London",
    "country": "UK"
  }
}
```

---

## Benefits

```text id="x5m8qt"
Fewer Queries
+
Faster Reads
```

---

## Structure

```text id="r8m2wp"
User
  |
  +--- Address
```

---

# 18. References

References link documents together.

---

## Example

```json id="n4v8xt"
{
  "userId": "123"
}
```

---

## Similar To

```text id="m7v3qt"
Foreign Key
```

in relational databases.

---

## Benefits

```text id="q8m4wr"
Reduced Duplication
```

---

## Tradeoff

```text id="r5v9xt"
More Queries
```

---

# 19. MongoDB Shell

MongoDB Shell (mongosh) is the command-line interface.

---

## Start Shell

```bash id="n7v2wp"
mongosh
```

---

## Show Databases

```javascript id="x4m8qt"
show dbs
```

---

## Use Database

```javascript id="m5v7wr"
use ecommerce
```

---

## Show Collections

```javascript id="p8m4xt"
show collections
```

---

## Benefits

```text id="r6m9wp"
Administration
+
Testing
+
Debugging
```

---

# 20. Installation

MongoDB can run locally, on VMs, containers, or cloud platforms.

---

## Installation Options

```text id="x7m2qt"
Local Server
+
Docker
+
Kubernetes
+
MongoDB Atlas
```

---

## Docker Example

```bash id="n5v8wr"
docker run -d \
  --name mongodb \
  -p 27017:27017 \
  mongo
```

---

## Verify

```bash id="m4v7xt"
mongosh
```

---

## Architecture

```text id="q6m8wp"
Application
      |
      v
MongoDB
      |
      v
Data Storage
```

---

# 21. MongoDB Query Language (MQL)

MongoDB uses MongoDB Query Language (MQL) to retrieve and manipulate data.

MQL is JSON-based and easy to understand.

---

## Basic Query

```javascript id="x7m4wp"
db.users.find()
```

Returns all documents.

---

## Query Structure

```javascript id="m5v8xt"
db.collection.find(
  query,
  projection
)
```

---

## Example

```javascript id="r8v2qt"
db.users.find({
  age: 25
})
```

---

## MQL Capabilities

```text id="p4m7wr"
Filtering
+
Sorting
+
Aggregation
+
Updates
+
Deletes
```

---

# 22. Query Operators

Query operators allow advanced filtering.

---

## Categories

```text id="x6v3qt"
Comparison
+
Logical
+
Element
+
Array
+
Evaluation
```

---

## Example

```javascript id="m8v4xp"
db.users.find({
  age: { $gt: 25 }
})
```

---

## Common Operators

```text id="r7m2wt"
$eq
$gt
$gte
$lt
$lte
$ne
$in
```

---

## Purpose

```text id="n5v9wr"
Precise Data Retrieval
```

---

# 23. Comparison Operators

Used to compare field values.

---

## Equal

```javascript id="q4m8xt"
db.users.find({
  age: { $eq: 25 }
})
```

---

## Greater Than

```javascript id="x8m2wp"
db.users.find({
  age: { $gt: 25 }
})
```

---

## Greater Than Or Equal

```javascript id="r5v7qt"
db.users.find({
  age: { $gte: 25 }
})
```

---

## Less Than

```javascript id="m4v8wr"
db.users.find({
  age: { $lt: 25 }
})
```

---

## Not Equal

```javascript id="p7m3xt"
db.users.find({
  age: { $ne: 25 }
})
```

---

## In Operator

```javascript id="n3v8wp"
db.users.find({
  age: { $in: [20,25,30] }
})
```

---

# 24. Logical Operators

Combine multiple conditions.

---

## AND

```javascript id="x5m9qt"
db.users.find({
  $and: [
    { age: 25 },
    { city: "London" }
  ]
})
```

---

## OR

```javascript id="r8v4wr"
db.users.find({
  $or: [
    { city: "London" },
    { city: "Paris" }
  ]
})
```

---

## NOT

```javascript id="m2v7xt"
db.users.find({
  age: {
    $not: { $gt: 30 }
  }
})
```

---

## NOR

```javascript id="q6m8wp"
db.users.find({
  $nor: [
    { city: "London" },
    { city: "Paris" }
  ]
})
```

---

## Usage

```text id="n7m4qt"
Complex Filtering
```

---

# 25. Element Operators

Used to check document structure.

---

## Exists

```javascript id="x4v8wr"
db.users.find({
  email: { $exists: true }
})
```

---

## Type

```javascript id="m8v3xt"
db.users.find({
  age: { $type: "int" }
})
```

---

## Example

```text id="q5m7wp"
Find Documents

Containing Email Field
```

---

## Benefits

```text id="r9v2qt"
Schema Validation
+
Data Quality Checks
```

---

# 26. Array Operators

MongoDB has powerful array support.

---

## Example Document

```json id="p4m8wr"
{
  "name": "John",
  "skills": ["Java", "Docker", "AWS"]
}
```

---

## Match Array Value

```javascript id="x7m4qt"
db.users.find({
  skills: "Docker"
})
```

---

## All Operator

```javascript id="n5v8xp"
db.users.find({
  skills: {
    $all: ["Java","Docker"]
  }
})
```

---

## Size Operator

```javascript id="r3m9wp"
db.users.find({
  skills: {
    $size: 3
  }
})
```

---

## Benefits

```text id="m4v8qt"
Flexible Data Modeling
```

---

# 27. Projection

Projection controls returned fields.

---

## Return All Fields

```javascript id="q8m2wr"
db.users.find()
```

---

## Return Specific Fields

```javascript id="x6v4wp"
db.users.find(
 {},
 { name: 1, age: 1 }
)
```

---

## Exclude Field

```javascript id="r5m9xt"
db.users.find(
 {},
 { password: 0 }
)
```

---

## Benefits

```text id="n7v3wr"
Less Network Traffic
+
Faster Queries
```

---

# 28. Sorting

Sorting organizes query results.

---

## Ascending

```javascript id="x4m8wr"
db.users.find().sort({
  age: 1
})
```

---

## Descending

```javascript id="m8v4qt"
db.users.find().sort({
  age: -1
})
```

---

## Multiple Fields

```javascript id="q4m8wr"
db.users.find().sort({
  city: 1,
  age: -1
})
```

---

## Values

```text id="r8v2qt"
1  = Ascending

-1 = Descending
```

---

# 29. Limiting Results

Limit reduces returned documents.

---

## Example

```javascript id="x6m4wp"
db.users.find().limit(5)
```

---

## Skip

```javascript id="n5v9xt"
db.users.find()
  .skip(10)
  .limit(5)
```

---

## Pagination

```text id="p7m3wr"
Skip
+
Limit
```

---

## Benefits

```text id="m4v8qt"
Improved Performance
```

---

# 30. Indexes

Indexes are the most important performance feature in MongoDB.

Without indexes MongoDB performs collection scans.

---

## Without Index

```text id="q5v7xt"
Collection
      |
      v
Scan Every Document
```

---

## With Index

```text id="r4m9wp"
Index
   |
   v
Locate Documents Quickly
```

---

## Benefits

```text id="n7m4wr"
Faster Queries
+
Lower Latency
```

---

## Tradeoff

```text id="x4m8wr"
Faster Reads

Slower Writes
```

---

# 31. Single Field Index

Index on one field.

---

## Create Index

```javascript id="q6v3wp"
db.users.createIndex({
  email: 1
})
```

---

## Example

```text id="r5v8qt"
email
```

indexed.

---

## Best For

```text id="n8m4xp"
Frequently Queried Fields
```

---

## Benefits

```text id="m3v7wr"
Fast Lookups
```

---

# 32. Compound Index

Index multiple fields together.

---

## Example

```javascript id="x7m2qt"
db.users.createIndex({
  city: 1,
  age: 1
})
```

---

## Useful For

```text id="p8m4wr"
Multi-Field Queries
```

---

## Example Query

```javascript id="r7v3xt"
db.users.find({
  city: "London",
  age: 30
})
```

---

## Benefits

```text id="n4m8wp"
Improved Performance
```

---

# 33. Multikey Index

Used for array fields.

---

## Example Document

```json id="x5v7qt"
{
  "skills": [
    "Java",
    "Docker",
    "AWS"
  ]
}
```

---

## Create Index

```javascript id="m8v2wr"
db.users.createIndex({
  skills: 1
})
```

---

## MongoDB Automatically Creates

```text id="r5v8wp"
Multikey Index
```

---

## Benefits

```text id="n7m4xt"
Fast Array Searches
```

---

# 34. Text Index

Supports text search.

---

## Create

```javascript id="p4m9wr"
db.posts.createIndex({
  title: "text",
  content: "text"
})
```

---

## Search

```javascript id="x8m3qt"
db.posts.find({
  $text: {
    $search: "mongodb"
  }
})
```

---

## Use Cases

```text id="m5v8wr"
Blogs
+
Articles
+
Knowledge Bases
```

---

# 35. TTL Index

Automatically removes expired documents.

TTL = Time To Live.

---

## Create TTL Index

```javascript id="r4v9xp"
db.logs.createIndex(
 {
   createdAt: 1
 },
 {
   expireAfterSeconds: 3600
 }
)
```

---

## Example

```text id="n6m4qt"
Delete Logs

After

1 Hour
```

---

## Use Cases

```text id="p7v3wr"
Logs
+
Sessions
+
Temporary Data
```

---

# 36. Unique Index

Prevents duplicate values.

---

## Create

```javascript id="x5m8qt"
db.users.createIndex(
 {
   email: 1
 },
 {
   unique: true
 }
)
```

---

## Result

```text id="r8m2wp"
Duplicate Emails

Not Allowed
```

---

## Benefits

```text id="n4v8xt"
Data Integrity
```

---

# 37. Query Optimization

Optimization improves query performance.

---

## Optimization Areas

```text id="m7v3qt"
Indexes
+
Projection
+
Efficient Queries
```

---

## Example

Bad:

```javascript id="q8m4wr"
db.users.find()
```

Large collection scan.

---

Good:

```javascript id="r5v9xt"
db.users.find({
  email: "john@test.com"
})
```

with index.

---

## Goal

```text id="n7v2wp"
Minimize Scanning
```

---

# 38. Explain Plans

Explain shows query execution details.

---

## Example

```javascript id="x4m8qt"
db.users.find({
  age: 30
}).explain("executionStats")
```

---

## Shows

```text id="m5v7wr"
Indexes Used
+
Documents Examined
+
Execution Time
```

---

## Purpose

```text id="p8m4xt"
Performance Analysis
```

---

# 39. Performance Tuning

MongoDB performance depends heavily on query design.

---

## Important Factors

```text id="r6m9wp"
Indexes
+
Query Design
+
Hardware
+
Storage
```

---

## Best Practices

```text id="x7m2qt"
Index Frequently Queried Fields
+
Use Projection
+
Avoid Full Collection Scans
```

---

## Monitor

```text id="n5v8wr"
Query Latency
+
CPU
+
Memory
```

---

# 40. Best Practices

---

## Query Best Practices

```text id="m4v7xt"
Use Indexes
+
Use Projection
+
Limit Results
+
Avoid Unnecessary Queries
```

---

## Index Best Practices

```text id="q6m8wp"
Create Required Indexes
+
Remove Unused Indexes
```

---

## Performance Best Practices

```text id="r8v4xt"
Monitor Queries
+
Use Explain Plans
+
Optimize Aggregations
```

---

## Production Recommendations

```text id="x5m9wr"
Index Properly
+
Monitor Performance
+
Review Query Patterns
+
Optimize Continuously
```

---

# 41. Aggregation Framework

The Aggregation Framework is MongoDB's powerful data processing engine.

It transforms and analyzes data through a sequence of stages called a pipeline.

---

## Purpose

```text id="x7m4wp"
Filter
+
Transform
+
Group
+
Analyze
```

---

## Architecture

```text id="m5v8xt"
Collection
      |
      v
Aggregation Pipeline
      |
      v
Results
```

---

## Example

```javascript id="r8v2qt"
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  }
])
```

---

## Common Use Cases

```text id="p4m7wr"
Reporting
+
Analytics
+
Dashboards
+
Business Intelligence
```

---

# 42. Aggregation Pipeline

Aggregation works through stages.

Each stage processes data and passes results to the next stage.

---

## Pipeline Flow

```text id="x6v3qt"
Documents
      |
      v
Stage 1
      |
      v
Stage 2
      |
      v
Stage 3
      |
      v
Result
```

---

## Example

```javascript id="m8v4xp"
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$country" } }
])
```

---

## Common Stages

```text id="r7m2wt"
$match
+
$project
+
$group
+
$sort
+
$limit
+
$lookup
```

---

## Benefits

```text id="n5v9wr"
High Performance
+
Powerful Analytics
```

---

# 43. Match Stage

$match filters documents.

Similar to SQL WHERE.

---

## Example

```javascript id="q4m8xt"
db.orders.aggregate([
 {
   $match: {
     status: "completed"
   }
 }
])
```

---

## SQL Equivalent

```sql id="x8m2wp"
SELECT *

FROM orders

WHERE status='completed'
```

---

## Best Practice

Place `$match` early.

---

## Benefits

```text id="r5v7qt"
Reduce Processing
+
Improve Performance
```

---

# 44. Project Stage

$project selects or transforms fields.

Similar to SQL SELECT.

---

## Example

```javascript id="m4v8wr"
db.users.aggregate([
 {
   $project: {
     name: 1,
     email: 1
   }
 }
])
```

---

## Rename Fields

```javascript id="p7m3xt"
{
  $project: {
    username: "$name"
  }
}
```

---

## Benefits

```text id="n3v8wp"
Reduce Output Size
+
Transform Data
```

---

# 45. Group Stage

$group groups documents.

Similar to SQL GROUP BY.

---

## Example

```javascript id="x5m9qt"
db.orders.aggregate([
 {
   $group: {
     _id: "$country",
     totalOrders: {
       $sum: 1
     }
   }
 }
])
```

---

## SQL Equivalent

```sql id="r8v4wr"
SELECT country,
COUNT(*)

FROM orders

GROUP BY country
```

---

## Common Operators

```text id="m2v7xt"
$sum
+
$avg
+
$min
+
$max
+
$count
```

---

## Benefits

```text id="q6m8wp"
Business Analytics
```

---

# 46. Sort Stage

$sort orders documents.

---

## Ascending

```javascript id="n7m4qt"
{
  $sort: {
    age: 1
  }
}
```

---

## Descending

```javascript id="x4v8wr"
{
  $sort: {
    age: -1
  }
}
```

---

## Example

```javascript id="m8v3xt"
db.users.aggregate([
 {
   $sort: {
     age: -1
   }
 }
])
```

---

## Benefits

```text id="q5m7wp"
Ordered Results
```

---

# 47. Limit Stage

$limit restricts the number of returned documents.

---

## Example

```javascript id="r9v2qt"
db.users.aggregate([
 {
   $limit: 10
 }
])
```

---

## Use Cases

```text id="p4m8wr"
Top 10 Records
+
Pagination
+
Reporting
```

---

## Benefits

```text id="x7m4qt"
Reduced Memory Usage
```

---

# 48. Lookup Stage

$lookup performs joins between collections.

---

## Example Collections

```text id="n5v8xp"
users

orders
```

---

## Example

```javascript id="r3m9wp"
db.orders.aggregate([
 {
   $lookup: {
     from: "users",
     localField: "userId",
     foreignField: "_id",
     as: "user"
   }
 }
])
```

---

## Similar To

```text id="m4v8qt"
SQL JOIN
```

---

## Benefits

```text id="q8m2wr"
Combine Related Data
```

---

# 49. Replica Sets

Replica Sets provide high availability and fault tolerance.

---

## Purpose

```text id="x6v4wp"
Data Redundancy
+
Automatic Failover
```

---

## Architecture

```text id="r5m9xt"
Primary
   |
   +---- Secondary

   |
   +---- Secondary
```

---

## Benefits

```text id="n7v3wr"
High Availability
+
Disaster Recovery
```

---

## Production Recommendation

```text id="x4m8wr"
Minimum

3 Nodes
```

---

# 50. Primary Node

The Primary node receives write operations.

---

## Responsibilities

```text id="m8v4qt"
Writes
+
Updates
+
Deletes
```

---

## Workflow

```text id="q4m8wr"
Application
      |
      v
Primary
      |
      v
Replication
```

---

## Characteristics

```text id="r8v2qt"
Single Writable Node
```

---

## Example

```text id="x6m4wp"
Primary

Node A
```

---

# 51. Secondary Node

Secondary nodes replicate data from Primary.

---

## Purpose

```text id="n5v9xt"
Redundancy
+
Read Scaling
```

---

## Workflow

```text id="p7m3wr"
Primary
    |
    v
Secondary
    |
    v
Secondary
```

---

## Benefits

```text id="m4v8qt"
Failover
+
Backup Reads
```

---

## Characteristics

```text id="q5v7xt"
Read Only
```

---

# 52. Election Process

If the Primary fails, MongoDB automatically elects a new Primary.

---

## Failure

```text id="r4m9wp"
Primary Down
```

---

## Election

```text id="n7m4wr"
Replica Set Members
      |
      v
Voting
```

---

## Result

```text id="x4m8wr"
New Primary Selected
```

---

## Benefits

```text id="q6v3wp"
Automatic Recovery
```

---

# 53. Read Preferences

Read Preference controls where reads occur.

---

## Modes

```text id="r5v8qt"
primary

primaryPreferred

secondary

secondaryPreferred

nearest
```

---

## Example

```javascript id="n8m4xp"
db.collection.find()
.readPref("secondary")
```

---

## Benefits

```text id="m3v7wr"
Read Scaling
+
Load Distribution
```

---

## Common Usage

```text id="x7m2qt"
Analytics Reads

On Secondary Nodes
```

---

# 54. Write Concerns

Write Concern determines write acknowledgment.

---

## Example

```javascript id="p8m4wr"
db.users.insertOne(
 {
   name: "John"
 },
 {
   writeConcern: {
     w: "majority"
   }
 }
)
```

---

## Levels

```text id="r7v3xt"
w:1

w:majority

w:0
```

---

## Meaning

```text id="n4m8wp"
How Many Nodes

Must Confirm Write
```

---

## Tradeoff

```text id="x5v7qt"
More Durability

=

More Latency
```

---

# 55. Read Concerns

Read Concern controls consistency of reads.

---

## Levels

```text id="m8v2wr"
local

available

majority

snapshot
```

---

## Example

```javascript id="r5v8wp"
db.collection.find(
 {},
 {
   readConcern: {
     level: "majority"
   }
 }
)
```

---

## Benefits

```text id="n7m4xt"
Data Consistency
+
Predictable Reads
```

---

## Tradeoff

```text id="p4m9wr"
Higher Consistency

=

Potentially Higher Latency
```

---

# Read Concern vs Write Concern

| Feature | Read Concern     | Write Concern    |
| ------- | ---------------- | ---------------- |
| Purpose | Read Consistency | Write Durability |
| Affects | Reads            | Writes           |
| Example | majority         | majority         |
| Goal    | Consistent Data  | Durable Data     |

---

# Replica Set Summary

```text id="x8m3qt"
Primary
+
Secondary
+
Replication
+
Automatic Elections
+
High Availability
```

---

# Aggregation Summary

```text id="m5v8wr"
Match
+
Project
+
Group
+
Sort
+
Limit
+
Lookup
```

---

# 56. Horizontal Scaling

Horizontal Scaling means adding more servers instead of increasing the size of a single server.

MongoDB is designed for horizontal scaling through sharding.

---

## Vertical Scaling

```text id="x7m4wp"
1 Server

4 CPU
16 GB RAM

      |
      v

Bigger Server

16 CPU
64 GB RAM
```

---

## Horizontal Scaling

```text id="m5v8xt"
Server 1
+
Server 2
+
Server 3
+
Server 4
```

---

## Benefits

```text id="r8v2qt"
Higher Capacity
+
Better Availability
+
Unlimited Growth Potential
```

---

## MongoDB Recommendation

```text id="p4m7wr"
Horizontal Scaling

For Large Datasets
```

---

# 57. Sharding Overview

Sharding is MongoDB's horizontal scaling solution.

Data is distributed across multiple servers called shards.

---

## Purpose

```text id="x6v3qt"
Scale Storage
+
Scale Reads
+
Scale Writes
```

---

## Without Sharding

```text id="m8v4xp"
All Data

Stored On

One Server
```

---

## With Sharding

```text id="r7m2wt"
Data

Distributed

Across Multiple Shards
```

---

## Architecture

```text id="n5v9wr"
Application
      |
      v
mongos
      |
      +----- Shard 1

      +----- Shard 2

      +----- Shard 3
```

---

## Benefits

```text id="q4m8xt"
Scalability
+
Performance
+
High Availability
```

---

# 58. Shard Keys

Shard Key determines how data is distributed.

It is the most important sharding decision.

---

## Purpose

```text id="x8m2wp"
Determine

Data Placement
```

---

## Example

```json id="r5v7qt"
{
  "userId": 123
}
```

---

## Shard Key Example

```text id="m4v8wr"
userId
```

---

## Good Shard Key

```text id="p7m3xt"
High Cardinality
+
Even Distribution
+
Frequently Queried
```

---

## Bad Shard Key

```text id="n3v8wp"
Few Unique Values
```

May create hot spots.

---

# 59. Config Servers

Config Servers store cluster metadata.

---

## Responsibilities

```text id="x5m9qt"
Shard Information
+
Chunk Information
+
Cluster Metadata
```

---

## Architecture

```text id="r8v4wr"
Config Server
      |
      v
Cluster Metadata
```

---

## Production Setup

```text id="m2v7xt"
3 Config Servers
```

---

## Importance

```text id="q6m8wp"
Required

For Sharding
```

---

# 60. Query Routers (mongos)

mongos acts as the query router.

Applications connect to mongos instead of directly to shards.

---

## Architecture

```text id="n7m4qt"
Application
      |
      v
mongos
      |
      +----- Shard 1

      +----- Shard 2

      +----- Shard 3
```

---

## Responsibilities

```text id="x4v8wr"
Route Queries
+
Merge Results
+
Hide Cluster Complexity
```

---

## Benefits

```text id="m8v3xt"
Transparent Scaling
```

---

## Example

```text id="q5m7wp"
Client

Does Not Need To Know

Where Data Lives
```

---

# 61. Backup & Restore

Backups protect against data loss.

Every production deployment requires backup strategies.

---

## Backup Goals

```text id="r9v2qt"
Disaster Recovery
+
Data Protection
+
Compliance
```

---

## Common Methods

```text id="p4m8wr"
mongodump
+
Snapshots
+
Atlas Backups
```

---

## Workflow

```text id="x7m4qt"
Backup
   |
   v
Storage
   |
   v
Restore
```

---

## Best Practice

```text id="n5v8xp"
Automated Backups
```

---

# 62. mongodump

mongodump creates logical backups.

---

## Backup Database

```bash id="r3m9wp"
mongodump \
  --db ecommerce
```

---

## Backup Entire Server

```bash id="m4v8qt"
mongodump
```

---

## Output

```text id="q8m2wr"
BSON Files
```

---

## Use Cases

```text id="x6v4wp"
Migration
+
Backup
+
Testing
```

---

# 63. mongorestore

mongorestore restores backups created by mongodump.

---

## Restore Database

```bash id="r5m9xt"
mongorestore dump/
```

---

## Restore Specific Database

```bash id="n7v3wr"
mongorestore \
  --db ecommerce \
  dump/ecommerce
```

---

## Workflow

```text id="x4m8wr"
Backup Files
      |
      v
mongorestore
      |
      v
Database
```

---

## Benefits

```text id="m8v4qt"
Fast Recovery
```

---

# 64. Monitoring

Monitoring is critical for production MongoDB deployments.

---

## Monitor

```text id="q4m8wr"
CPU
+
Memory
+
Disk
+
Queries
+
Replication
```

---

## Architecture

```text id="r8v2qt"
MongoDB
      |
      v
Metrics
      |
      v
Monitoring Tool
```

---

## Popular Tools

```text id="x6m4wp"
MongoDB Atlas
+
Prometheus
+
Grafana
+
Datadog
```

---

## Goal

```text id="n5v9xt"
Detect Problems Early
```

---

# 65. Logging

Logs help diagnose issues and monitor activity.

---

## Log Categories

```text id="p7m3wr"
Queries
+
Replication
+
Authentication
+
Errors
```

---

## Example Log Entry

```text id="m4v8qt"
Connection Accepted
```

---

## Log Location

```text id="q5v7xt"
mongod.log
```

---

## Benefits

```text id="r4m9wp"
Troubleshooting
+
Auditing
```

---

# 66. Troubleshooting

Production issues require systematic investigation.

---

## Common Problems

```text id="n7m4wr"
Slow Queries
+
High CPU
+
Replication Lag
+
Disk Usage
```

---

## Investigation Workflow

```text id="x4m8wr"
Problem
   |
   v
Logs
   |
   v
Metrics
   |
   v
Root Cause
```

---

## Useful Commands

```javascript id="q6v3wp"
db.serverStatus()
```

---

```javascript id="r5v8qt"
db.currentOp()
```

---

## Goal

```text id="n8m4xp"
Identify Root Cause
```

---

# 67. Performance Optimization

Performance optimization improves throughput and reduces latency.

---

## Key Areas

```text id="m3v7wr"
Indexes
+
Queries
+
Hardware
+
Storage
```

---

## Best Practices

```text id="x7m2qt"
Create Proper Indexes
+
Use Explain Plans
+
Avoid Collection Scans
```

---

## Example

```text id="p8m4wr"
Indexed Query

Milliseconds

----------------

Collection Scan

Seconds
```

---

## Goal

```text id="r7v3xt"
Fast Queries
+
Efficient Resources
```

---

# 68. Capacity Planning

Capacity Planning ensures MongoDB can support future growth.

---

## Consider

```text id="n4m8wp"
Storage Growth
+
User Growth
+
Traffic Growth
+
Replication
```

---

## Workflow

```text id="x5v7qt"
Current Usage
      |
      v
Forecast Growth
      |
      v
Scale Infrastructure
```

---

## Metrics

```text id="m8v2wr"
Disk Usage
+
Memory Usage
+
OPS
+
Connections
```

---

## Benefits

```text id="r5v8wp"
Avoid Outages
```

---

# 69. Storage Optimization

Storage efficiency directly affects cost and performance.

---

## Optimization Areas

```text id="n7m4xt"
Indexes
+
Compression
+
Data Retention
+
Archiving
```

---

## Common Waste

```text id="p4m9wr"
Unused Indexes
+
Old Data
+
Duplicate Data
```

---

## Strategies

```text id="x8m3qt"
TTL Indexes
+
Archival Storage
+
Data Cleanup
```

---

## Benefits

```text id="m5v8wr"
Lower Storage Cost
+
Better Performance
```

---

# Sharding Architecture Summary

```text id="r4v9xp"
Application
      |
      v
mongos
      |
      v
Config Servers
      |
      v
Shards
```

---

# Production Operations Summary

```text id="n6m4qt"
Sharding
+
Backups
+
Monitoring
+
Logging
+
Capacity Planning
+
Performance Tuning
```

---

# 70. MongoDB Security

Security is a critical aspect of production MongoDB deployments.

A database often contains the most sensitive organizational data.

---

## Security Goals

```text id="x7m4wp"
Confidentiality
+
Integrity
+
Availability
```

---

## Security Layers

```text id="m5v8xt"
Authentication
+
Authorization
+
Encryption
+
Auditing
```

---

## Security Architecture

```text id="r8v2qt"
Users
   |
   v
Authentication
   |
   v
Authorization
   |
   v
MongoDB
```

---

## Best Practice

```text id="p4m7wr"
Never Expose MongoDB

Directly To Internet
```

---

# 71. Authentication

Authentication verifies user identity.

---

## Purpose

```text id="x6v3qt"
Who Are You?
```

---

## Authentication Methods

```text id="m8v4xp"
SCRAM
+
X.509
+
LDAP
+
Kerberos
```

---

## Create User

```javascript id="r7m2wt"
db.createUser({
  user: "admin",
  pwd: "StrongPassword",
  roles: ["root"]
})
```

---

## Enable Authentication

```yaml id="n5v9wr"
security:
  authorization: enabled
```

---

## Benefits

```text id="q4m8xt"
Controlled Access
```

---

# 72. Authorization

Authorization determines what authenticated users can do.

---

## Purpose

```text id="x8m2wp"
What Can You Access?
```

---

## Example

```text id="r5v7qt"
User A

Read Only
```

---

```text id="m4v8wr"
User B

Read + Write
```

---

## Workflow

```text id="p7m3xt"
Authentication
      |
      v
Authorization
      |
      v
Access Granted
```

---

## Benefits

```text id="n3v8wp"
Least Privilege
```

---

# 73. RBAC (Role-Based Access Control)

MongoDB uses RBAC to manage permissions.

---

## Concept

```text id="x5m9qt"
User
  |
  v
Role
  |
  v
Permissions
```

---

## Built-In Roles

```text id="r8v4wr"
read
+
readWrite
+
dbAdmin
+
clusterAdmin
+
root
```

---

## Example

```javascript id="m2v7xt"
db.createUser({
 user: "developer",
 pwd: "password",
 roles: [
   {
     role: "readWrite",
     db: "appdb"
   }
 ]
})
```

---

## Best Practice

```text id="q6m8wp"
Grant Minimum Required Access
```

---

# 74. TLS Encryption

TLS encrypts network communication.

---

## Purpose

```text id="n7m4qt"
Protect Data

In Transit
```

---

## Without TLS

```text id="x4v8wr"
Client
  |
  v
Plain Text
  |
  v
MongoDB
```

---

## With TLS

```text id="m8v3xt"
Client
  |
  v
Encrypted Traffic
  |
  v
MongoDB
```

---

## Benefits

```text id="q5m7wp"
Confidentiality
+
Integrity
```

---

## Recommendation

```text id="r9v2qt"
Always Use TLS
In Production
```

---

# 75. Auditing

Auditing tracks database activity.

---

## Purpose

```text id="p4m8wr"
Who Did What?
```

---

## Events Captured

```text id="x7m4qt"
Logins
+
Queries
+
Updates
+
Administrative Actions
```

---

## Workflow

```text id="n5v8xp"
User Action
      |
      v
Audit Log
```

---

## Benefits

```text id="r3m9wp"
Compliance
+
Security Investigation
```

---

# 76. MongoDB Atlas

MongoDB Atlas is MongoDB's fully managed cloud database platform.

---

## Purpose

```text id="m4v8qt"
Managed MongoDB
```

---

## Atlas Handles

```text id="q8m2wr"
Provisioning
+
Backups
+
Monitoring
+
Scaling
+
Security
```

---

## Architecture

```text id="x6v4wp"
Application
      |
      v
Atlas Cluster
      |
      v
Storage
```

---

## Benefits

```text id="r5m9xt"
Reduced Operational Overhead
```

---

# 77. Atlas Architecture

Atlas abstracts infrastructure management.

---

## Components

```text id="n7v3wr"
Cluster
+
Nodes
+
Storage
+
Networking
```

---

## Deployment Models

```text id="x4m8wr"
AWS
+
Azure
+
Google Cloud
```

---

## Cluster Architecture

```text id="m8v4qt"
Application
      |
      v
Atlas
      |
      v
Replica Set
```

---

## Benefits

```text id="q4m8wr"
High Availability
+
Managed Operations
```

---

# 78. Atlas Backups

Atlas provides automated backups.

---

## Features

```text id="r8v2qt"
Continuous Backups
+
Point-In-Time Recovery
+
Snapshots
```

---

## Workflow

```text id="x6m4wp"
Atlas Cluster
      |
      v
Backup Storage
      |
      v
Restore
```

---

## Benefits

```text id="n5v9xt"
Disaster Recovery
```

---

## Best Practice

```text id="p7m3wr"
Enable Continuous Backups
```

---

# 79. Atlas Monitoring

Atlas provides built-in monitoring.

---

## Metrics

```text id="m4v8qt"
CPU
+
Memory
+
Disk
+
Connections
+
Queries
```

---

## Dashboard

```text id="q5v7xt"
Metrics
+
Alerts
+
Performance Insights
```

---

## Benefits

```text id="r4m9wp"
Visibility
+
Troubleshooting
```

---

## Alerting

```text id="n7m4wr"
CPU Alerts
+
Storage Alerts
+
Connection Alerts
```

---

# 80. Kubernetes Deployment

MongoDB can run inside Kubernetes.

---

## Deployment Model

```text id="x4m8wr"
Kubernetes
      |
      v
StatefulSet
      |
      v
MongoDB Pods
```

---

## Benefits

```text id="q6v3wp"
Automation
+
Self-Healing
+
Scalability
```

---

## Requirements

```text id="r5v8qt"
Persistent Storage
+
StatefulSets
```

---

# 81. StatefulSets

MongoDB is a stateful application.

StatefulSets provide stable identities and storage.

---

## Why StatefulSets?

```text id="n8m4xp"
Stable Hostnames
+
Persistent Volumes
```

---

## Architecture

```text id="m3v7wr"
mongo-0

mongo-1

mongo-2
```

---

## Benefits

```text id="x7m2qt"
Predictable Networking
+
Persistent Storage
```

---

## Example

```yaml id="p8m4wr"
kind: StatefulSet
```

---

# 82. Operators

Operators automate MongoDB management in Kubernetes.

---

## Responsibilities

```text id="r7v3xt"
Deployment
+
Scaling
+
Backup
+
Recovery
```

---

## Architecture

```text id="n4m8wp"
Operator
      |
      v
MongoDB Cluster
```

---

## Popular Operator

```text id="x5v7qt"
MongoDB Community Operator
```

---

## Benefits

```text id="m8v2wr"
Automation
+
Reduced Manual Work
```

---

# 83. DevOps Best Practices

MongoDB should be treated as critical infrastructure.

---

## Best Practices

```text id="r5v8wp"
Enable Authentication
+
Use TLS
+
Monitor Performance
+
Automate Backups
```

---

## Operations Best Practices

```text id="n7m4xt"
Replica Sets
+
Monitoring
+
Disaster Recovery
```

---

## Deployment Best Practices

```text id="p4m9wr"
Infrastructure As Code
+
Automation
+
Secrets Management
```

---

# 84. Common Mistakes

Many MongoDB issues originate from poor operational practices.

---

## Common Mistakes

```text id="x8m3qt"
No Authentication
+
No Backups
+
Poor Indexing
+
No Monitoring
+
Overusing Sharding
```

---

## Impact

```text id="m5v8wr"
Performance Problems
+
Security Risks
+
Data Loss
```

---

## Avoid By

```text id="r4v9xp"
Planning
+
Testing
+
Monitoring
```

---

# 85. Common Interview Questions

## What is MongoDB?

```text id="n6m4qt"
Document-Oriented
NoSQL Database
```

---

## Difference Between Collection And Document?

```text id="p7v3wr"
Collection
=
Container

Document
=
Actual Data
```

---

## What Is BSON?

```text id="x5m8qt"
Binary JSON
```

---

## What Is A Replica Set?

```text id="r8m2wp"
Group Of MongoDB Servers

Providing High Availability
```

---

## What Is Sharding?

```text id="n4v8xt"
Horizontal Data Partitioning
```

---

## What Is A Shard Key?

```text id="m7v3qt"
Field Used To
Distribute Data
```

---

## Difference Between Read Concern And Write Concern?

```text id="q8m4wr"
Read Concern
=
Read Consistency

Write Concern
=
Write Durability
```

---

## What Is Atlas?

```text id="r5v9xt"
MongoDB Managed Cloud Platform
```

---

## Why Use Indexes?

```text id="n7v2wp"
Improve Query Performance
```

---

## What Is Aggregation?

```text id="x4m8qt"
Data Processing Pipeline
```

---

# 86. Additional Resources

| Resource                    | URL                                                                                                              |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| MongoDB Docs                | [https://www.mongodb.com/docs](https://www.mongodb.com/docs)                                                     |
| MongoDB Atlas               | [https://www.mongodb.com/atlas](https://www.mongodb.com/atlas)                                                   |
| MongoDB University          | [https://learn.mongodb.com](https://learn.mongodb.com)                                                           |
| MongoDB Community           | [https://www.mongodb.com/community](https://www.mongodb.com/community)                                           |
| MongoDB Kubernetes Operator | [https://github.com/mongodb/mongodb-kubernetes-operator](https://github.com/mongodb/mongodb-kubernetes-operator) |

---

# 📎 Official Documentation

* [https://www.mongodb.com/docs](https://www.mongodb.com/docs)
* [https://www.mongodb.com/atlas](https://www.mongodb.com/atlas)
* [https://learn.mongodb.com](https://learn.mongodb.com)
* [https://www.mongodb.com/community](https://www.mongodb.com/community)
* [https://github.com/mongodb/mongodb-kubernetes-operator](https://github.com/mongodb/mongodb-kubernetes-operator)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, software engineering, backend development, DevOps engineering, platform engineering, cloud engineering, SRE, database administration, and Kubernetes operations.

Primary references include:

* MongoDB Documentation
* MongoDB Atlas Documentation
* MongoDB University
* MongoDB Community Resources
* MongoDB Kubernetes Operator Documentation

MongoDB is a document-oriented NoSQL database developed by [MongoDB, Inc.](https://www.mongodb.com?utm_source=chatgpt.com).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

```text id="x8m4qt"
Documents
+
Collections
+
CRUD Operations
+
Indexes
+
Aggregation
+
Replica Sets
+
Sharding
+
Atlas
+
Security
+
High Availability
```

MongoDB is one of the most widely used NoSQL databases and provides flexible schema design, horizontal scalability, powerful querying capabilities, high availability, cloud-native integrations, and enterprise-grade security for modern applications.

---

```md id="n7v3wr"
**Last Updated:** 2026
**Platform:** MongoDB
**Version:** MongoDB 8.x
**License:** Server Side Public License (SSPL)

For latest information, visit https://www.mongodb.com/docs/
```