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

 **Mongo Shell**  

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

**Insert 1**  

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

**Insert Many**  

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

**Reading Documents: Scalar Fields**  

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

**Reading Documents: Array Fields**  

Array matches can be based on the whole array, elements of the array or a specific element of the array.  

Find all movies in which Jeff Bridges is the first named actor in the array:  
```
db.movies.find({"cast.0": "Jeff Bridges"})  
```  













