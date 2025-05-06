#           MongoDB Full Course Notes       #

## Exit & Clear Commands ##

Exit MongoDB shell:
> exit

Clear screen:
> cls


###   DATABASES   ###

Show databases:
> show dbs

Use database (switch to or create):
> use admin  
> use school   # (creates new DB only if collections added)

Create a collection (equivalent to a table):
> db.createCollection("students")  # adds to `school` database

(if empty, will not show dbs)

Drop current database:
> db.dropDatabase()


###    INSERT     ###

Insert a single document:
> db.students.insertOne({name: "SpongeBob", age: 30, gpa: 3.2})

Insert multiple documents:
> db.students.insertMany([
    {name: "Patrick", age: 32, gpa: 2.1},
    {name: "Sandy", age: 29, gpa: 3.8}
])

View all documents:
> db.students.find()


###   DATA TYPES     ###

- String:           "text" or 'text'
- Integer:          10
- Double:           2.5
- Boolean:          true / false
- Date:             new Date()
- Null:             null
- Array:            courses: ["bio", "chem", "phy"]
- Nested Document:  
  db.students.insertOne({
    name: "Larry",
    address: {
        street: "123 Fake St",
        zip: 1234
    }
  })


###  SORTING & LIMITING RESULTS ###

Sort by name (ascending/descending):
> db.students.find().sort({name: 1})     # A to Z  
> db.students.find().sort({name: -1})    # Z to A

Sort by GPA:
> db.students.find().sort({gpa: 1})      # Low to high  
> db.students.find().sort({gpa: -1})     # High to low

Limit results:
> db.students.find().limit(1)  
> db.students.find().limit(3)

Top student (highest GPA):
> db.students.find().sort({gpa: -1}).limit(1)

Lowest GPA student:
> db.students.find().sort({gpa: 1}).limit(1)


###     FIND()       ###

Find all:
> db.students.find()

Note:  
Parameter: .find({query},{projection})  
# {query} is like sql

Find with query:
> db.students.find({name: "SpongeBob"})  
> db.students.find({gpa: 4.0})  
> db.students.find({fullTime: false})  
> db.students.find({gpa: 4.0, fullTime: false})

Projection (only return specific fields):
> db.students.find({}, {name: true})  
> db.students.find({}, {_id: false, name: true})  
> db.students.find({}, {_id: false, name: true, gpa: true})


###    UPDATE()       ###

Parameter: .updateOne(filter, update)

Update one:
> db.students.updateOne({name: "SpongeBob"}, {$set: {fullTime: true}})

Remove a field:
> db.students.updateOne({name: "SpongeBob"}, {$unset: {fullTime: ""}})

Update many:
> db.students.updateMany({}, {$set: {fullTime: false}})

Add missing field to all:
> db.students.updateMany(
    {fullTime: {$exists: false}},
    {$set: {fullTime: true}}
)


###    DELETE()      ###

Delete one:
> db.students.deleteOne({name: "Larry"})

Delete many:
> db.students.deleteMany({fullTime: false})  
> db.students.deleteMany({registerDate: {$exists: false}})


###  COMPARISON QUERY OPERATORS  ###

> db.students.find({name: {$ne: "SpongeBob"}})   # Not equal  
> db.students.find({age: {$lt: 20}})             # Less than  
> db.students.find({age: {$lte: 22}})            # Less than or equal  
> db.students.find({gpa: {$gt: 3.0}})            # Greater than  
> db.students.find({name: {$in: ["SpongeBob", "Patrick", "Sandy"]}})  
> db.students.find({name: {$nin: ["SpongeBob", "Patrick", "Sandy"]}})


### LOGICAL QUERY OPERATORS    ###

$and, $or, $not, $nor

> db.students.find({$and: [{fullTime: true}, {age: {$lte: 22}}]})

> db.students.find({age: {$not: {$gte: 30}}})

###   INDEXES     ###

Check performance stats:
> db.students.find({name: "Larry"}).explain("executionStats")

Create index (B *Tree):
> db.students.createIndex({name: 1})

/*
now following command will show one document examine:

db.students.find({name:"Larry"}).explain("executionStats")
*/

List indexes:
> db.students.getIndexes()

Drop index:
> db.students.dropIndex("name_1")

# linear search video search yt for more details

###  COLLECTIONS     ###

Show collections:
> show collections

Create a capped collection (fixed size):
> db.createCollection("teachers", {
    capped: true,
    size: 1024000,     # 1MB = 1000 * 1024 = 1,024,000
    max: 100
})

// autoIndexId: false is deprecated, no longer recommended

// More advanced options are available in official MongoDB docs.

#        END OF NOTES - MONGO CHEATSHEET     #
