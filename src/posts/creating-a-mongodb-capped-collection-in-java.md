---
title: Creating a MongoDB Capped Collection In Java
date: 2021-07-13
tags: [java, mongodb, morphia]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628543688/0_mEG350RCW6H7trnP_zohlos.jpg)

In MongoDB, it’s possible to preserve the insertion order of documents into a collection in a circular fashion. These types of collections are called Capped Collections in MongoDB. The MongoDB documentation describes Capped Collections:

> “Capped collections are fixed-size collections that support high-throughput operations that insert, retrieve, and delete documents based on insertion order. Capped collections work in a way similar to circular buffers: once a collection fills its allocated space, it makes room for new documents by overwriting the oldest documents in the collection.”

OK, so we know what a capped collection is, but how to we create one?

From the MongoDB shell, we would create a capped collection using the `db.createCollection` command:

```java
db.createCollection("logs", {capped: true,
                             size: 4096,
                             max:5})
```

This command tells MongoDB to create a collection called “logs” with a maximum size of 4096 bytes which can hold a maximum of 5 documents. When the 6th document is added, the first document is removed form the collection ensuring that there is only ever a maximum of 5 documents in the collection. The “size” parameter is mandatory, however the “max” parameter is optional.

In Java, there are two common ways to communicate with MongoDB; the MongoDB [Java Driver](https://github.com/mongodb/mongo-java-driver/wiki) and with [Morphia](https://github.com/mongodb/morphia/wiki) (the lightweight type-safe mapping library for mapping Java objects to/from MongoDB).

First, let’s first look at using the Java Driver.

## Java Driver

With the Java Driver, again, we use the `db.createCollection` command, this time passing a `BasicDBObject` as the parameter. This parameter has the fields “capped”, “size” and “max” which specify that the collection is capped, the maximum size of the collection in bytes and the maximum number of entries in the collection. The following code snippet shows how to connect to a local instance of MongoDB and create a capped collection.

```java
MongoClient mongoClient = new MongoClient(new MongoClientURI("mongodb://localhost"));
DB db = mongoClient.getDB("test");

DBCollection collection;
if (!db.collectionExists("cappedLogsJavaDriver")) {
    BasicDBObject options = new BasicDBObject("capped", true);
    options.append("size", 4096);
    options.append("max", 5);
    collection = db.createCollection("cappedLogsJavaDriver", options);
} else {
    collection = db.getCollection("cappedLogsJavaDriver");
}
```

Once we’ve created a collection, we can insert documents into it to ensure that it is working as expected. The following code snippet shows how to insert 8 documents into the collection (remember, only the last 5 of these will be stored as it’s a capped collection).

```java
for (int i = 0; i < 8; i++) {
    BasicDBObject logEntry = new BasicDBObject("logId", i);
    collection.insert(logEntry);
}
```

Using the MongoDB interactive shell, we can verify the documents that are now stored in the collection are as expected.

```json
> db.cappedLogsJavaDriver.find()
{ "_id" : ObjectId("54a1ca44a82617da4f72e025"), "logId" : 3 }
{ "_id" : ObjectId("54a1ca44a82617da4f72e026"), "logId" : 4 }
{ "_id" : ObjectId("54a1ca44a82617da4f72e027"), "logId" : 5 }
{ "_id" : ObjectId("54a1ca44a82617da4f72e028"), "logId" : 6 }
{ "_id" : ObjectId("54a1ca44a82617da4f72e029"), "logId" : 7 }
```

## Morphia

Now that we’ve seen how to create a collection via the Java Driver, let’s see how we can accomplish the same thing by using Morphia.

The essence of Morphia is to map Java classes to/from MongoDB. Classes that we wish to persist in MongoDB are annotated with the `@Entity` annotation, which are then stored within a collection usually named after the class being annotated. To create a capped collection, we must add additional values onto the `@Entity` annotation to specify the maximum number of entries in the collection and the size of the collection. Modelling the same type of object as used in the example for the Java Driver, we would create a LogEntry class as follows:

```java
@Entity(value="cappedLogsMorphia", cap=@CappedAt(count=5, value=4096))
public class LogEntry {

    private int logId;

    @Id
    private ObjectId id;

    public LogEntry(int logId) {
        this.logId = logId;
    }

    public int getLogId() {
        return logId;
    }

    public void setLogId(int logId) {
        this.logId = logId;
    }
}
```

We can see that this class is annotated with `@Entity` specifying that the collection should be capped with a maximum of 5 documents and a size of 4096 bytes.

With Morphia, the capped collection is created at start-up time by invoking the `.ensureCaps()` method on the Morphia `Datastore` as shown below.

```java
MongoClient mongoClient = new MongoClient(new MongoClientURI("mongodb://localhost"));
DB db = mongoClient.getDB("test");

Morphia morphia = new Morphia();
morphia.map(LogEntry.class);

Datastore datastore = morphia.createDatastore(mongoClient, "test");
datastore.ensureCaps();
```

Again, as before, we can insert 8 documents into the collection to verify that only the last 5 are stored.

```java
for (int i = 0; i < 8; i++) {
    LogEntry logEntry = new LogEntry(i);
    datastore.save(logEntry);
}
```

```json
> db.cappedLogsMorphia.find()
{ "_id" : ObjectId("54a1ce9da82629642c64f5d9"), "className" : "com.cloudblogaas.cappedcollection.LogEntry", "logId" : 3 }
{ "_id" : ObjectId("54a1ce9da82629642c64f5da"), "className" : "com.cloudblogaas.cappedcollection.LogEntry", "logId" : 4 }
{ "_id" : ObjectId("54a1ce9da82629642c64f5db"), "className" : "com.cloudblogaas.cappedcollection.LogEntry", "logId" : 5 }
{ "_id" : ObjectId("54a1ce9da82629642c64f5dc"), "className" : "com.cloudblogaas.cappedcollection.LogEntry", "logId" : 6 }
{ "_id" : ObjectId("54a1ce9da82629642c64f5dd"), "className" : "com.cloudblogaas.cappedcollection.LogEntry", "logId" : 7 }
```

## Checking a collection status

Once we’ve created a capped collection in MongoDB, we can check its status by executing the `.stats()` method on the collection from within the Mongo DB interactive shell.

```javascript
> db.cappedLogsJavaDriver.stats()
{
	"ns" : "test.cappedLogsJavaDriver",
	"count" : 5,
	"size" : 180,
	"avgObjSize" : 36,
	"storageSize" : 4096,
	"numExtents" : 1,
	"nindexes" : 1,
	"lastExtentSize" : 4096,
	"paddingFactor" : 1,
	"systemFlags" : 1,
	"userFlags" : 0,
	"totalIndexSize" : 8176,
	"indexSizes" : {
		"_id_" : 8176
	},
	"capped" : true,
	"max" : 5,
	"ok" : 1
}
```

Here we can see that the collection is indeed capped (“capped”=true) and that the maximum number of entries in the collection is 5 (“max”=5).

## Credits

Photo by [Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
