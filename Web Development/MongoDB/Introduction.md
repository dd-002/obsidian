MongoDB is a  non relational database. It contains each object known as a document, a **collection** is a collection of documents.

## Connecting -
Intially when we connect to mongodb using mongosh , we dont have a database(just after spinning up a cluster). When we create a collection it will automatically generate a database for us.

### Checking databases --
```
show dbs
```


Initially we have 2 databases :
1. admin
2. local


### Basic Commands --
```
db.createCollection("userCollection")
show collections        //shows all the collections
use admin               //switches to admin database
db                      //shows the current database in use
db.<collectionName>.drop()  //deletes a collection
db.<collectionNane>.insertOne({"name":"Dipayan","age":30})
db.<collectionName>.find() //shows all the documents
db.<collectionName>.find({"name":"Dipayan"}) //shows filtered document
db.<collectionName>.find({age:30}) //shows all the filtered documents
```

### Querying --

find function: find(query,projection)

projection -- means the number of fields to return in the matched output. Also, we can only mention either all include projections, or all exclude projections, but not both. The only exception to this is \_id .


Also note, the return type is **cursor** . 


{field:{$operator:value}}

```
//operators in mongodb start with $
db.<collectionName>.find({age:{$lte:30}}) //age less than or equal 30

//using projection
db.userCollection.find({age:{$lte:30}},{age:0}) //doesnt return age field

//querying in nested documents, but we need to use " in this case
db.userCollection.find({ age: { $gt: 30 },"address.city":"Kolkata" })
db.userCollection.find({ $or : [{age:30},{name:"Dipayan"}] })

```

### Useful methods --
```
db.<collectionName>.find().count() // counts all documents
db.<collectionName>.find().sort("age":1) //sorts the output in ascending, -1 for descending
db.<collectionName>.find().skip(2).limit(3) //skips the first 2 documents and sends the next three

```