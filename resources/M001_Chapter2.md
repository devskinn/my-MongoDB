## Mongodb Query Language and Atlas  

**C**reate  
**R**ead  
**U**pdate  
**D**elete  

- Three of the crud operations are write operations (create, update, delete)  
- Compass does not fully support Mongodb query language and therefore Mongo Shell is normally used.  
- Executable for Mongo Shell is mongo.exe (mongod.exe is to run the server).  
- Install Server product which also installs the shell.  
- Set environment path to point to install/bin folder.  
- Can use ``mongo --nodb`` to check shell is working as expected.  

 ### Mongo Shell  

 Typical connect string - connects to a database called test in the cluster  
 Must be run from command line and not in the mongo shell, otherwise an error will occur  
 ```
 mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username <username> --password <password>
 ```  

Typical commands:  
``use <databasename>`` switches database and connects  
``show collections`` show collections in currently connected database  
``db.<collectionname>.find().pretty()`` lists Documents in collection  
``show dbs`` show databases in cluster  
``load("filename.js")`` load data from script file  

Note:  
- Only primaries in a cluster can accept write operations.  
- All hostnames are included in the connection string in case the primary goes down.  

### Insert 1  

Method used to insert one document in to a database using the mongo shell.  

Example: ``db.moviesScratch.insertOne({title: "Star Trek II: The Wrath of Khan", year: 1982, imdb: "tt0084726"})``  

If entered correctly output should be:  
```
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5cb3432a369ae4789d30e5f0")
}
```  
Note: ObjectId is created automatically but can be created manually if required.  

### Insert Many  

Method used to insert multiple documents in to a database using the mongo shell.  

Example:  
```
db.moviesScratch.insertMany(
    [
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0796366",
	    "title" : "Star Trek",
	    "year" : 2009,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        }
    ],
    {
        "ordered": false 
    }
);
```  

### Reading Documents: Scalar Fields  

Method used to search for documents is 'Find', examples below are using mongo shell.  

Example:  
```
use video  
db.movies.find({mpaaRating: "PG-13"}).pretty()  
```  

Searching sub documents:  
Use 'dot' notation to search sub docs e.g.  
```
use 100YWeatherSmall  
db.data.find({"wind.direction.angle": 100}).pretty()  
```  

```
use video  
db.movieDetails.find({"rated": "PG", "awards.nominations": 10}).count()  
```  

### Reading Documents: Array Fields  

Array matches can be based on the whole array, elements of the array or a specific element of the array.  

Find all movies in which Jeff Bridges is the first named actor in the array:  
```
db.movies.find({"cast.0": "Jeff Bridges"})  
```  

### Cursors  

- Pointer to current location in a result set.  
- Default is set for cursor to return 20 items.  
- Entering "it" will iterate through the result set showing 20 items at a time.  

### Projections  

- Reduces network and resources requirements by limiting fields returned in a result document.  
- Can be defined as the second argument when using the 'find' method.  
- A '1' in the projection explicitly includes a field, using '0' explicitly excludes the field.  

Example: Return only the title  
``db.movies.find({genre: "Action, Adventure"}, {title: 1})``  

Example: Return only the title and exlude the object id  
``db.movies.find({genre: "Action, Adventure"}, {title: 1, _id: 0})``  

### Update Documents: updateOne()  

updateOne() is the method used to update single documents.  

Example: 'The Martian' movie does not include a field with a link to the movie poster, the following will add this:  
```
db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $set: {
    poster: "http://ia.media-imdb.com/images/....."
  }
})
```  
output will be:  
``{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }``  

Example: Update with a field (awards) containing multiple entries:  
```
db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $set: {
    "awards": {
      "wins": 8,
      "nominations": 14,
      "text": "Nominated for 3 Golden Globes. Another 8 wins and 14 nominations."
    }
  }
})
```  

### Update Operators  

- Used to correct errors.  
- Keep data current over time.  

Examples: Above shows **$set** operator, below shows incrementing fields using the **$inc** operator:
```
db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $inc: {
    "tomato.reviews": 3,
    "tomato.userReviews": 25
  }
})
```  

Updating arrays uses several operators such as **$pop**, **$push**, **$pull**, **$pullall** etc, see documentation for details.  

Link to documentation: https://docs.mongodb.com/manual/reference/operator/update/  

### Update Documents: updateMany()  

updateMany() makes same changes to all documents that match the filter used.  

Example: Using the **$unset** operator to remove all the fields specified that have a value of null:  
```
db.movieDetails.updateMany({
  rated: null
}, {
  $unset: {
    rated: ""
  }
})
```  

### Upserts  

Following will update any documents already in the collection and if the document does not exist, the **upsert** command will enter it into the collection as a new document.  
```
db.movieDetails.updateOne({
  "imdb.id": details.imdb.id
}, {
  $set: detail
}, {
  upsert: true
});
```  

### Update Documents: replaceOne()  

Mongo shell is based on javascript and thus following commands require a semi colon at the end of each line.  
```
# set filter variable to value of movie title which requires updating
let filter = {title: "House, M.D., Season Four: New Beginnings"};

# set doc variable to document details specified by the filter
let doc = db.movieDetails.findOne(filter);

# get current value of doc.poster
doc.poster;

# change value of doc.poster
doc.poster = "https://www.imdb.com/title/tt1329164/mediaviewer/rm2619416576";

# get current values of doc.genres
doc.genres;

# add a new value to the doc.genres array
doc.genres.push("TV Series");

# at this time the changes only exist in the mongo shell
# use the following to update the database (using values from the filter and doc variables)
db.movieDetails.replaceOne(filter, doc);
```  

Important aspect to consider about replaceOne:  

- The replacement document cannot contain update operators.  
- replaceOne will apply changes to only one document, the first found in the server that matches the filter expression, using the $natural order of documents in the collection.  

### Deleting Documents  

Methods supported are **deleteOne()** and **deleteMany()** and are similar to the corresponding update methods.  

Use deleteOne() to remove a single document usings its object id:  
``db.reviews.deleteOne({_id: ObjectId("5cae643ea7c101056d6eef8e")})``  

Use deleteMany() to remove all documents belonging to the reviewer id specified:  
``db.reviews.deleteMany({reviewer_id: 759723314})``  
















