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

Note:  
- Only primaries in a cluster can accept write operations.  
- All hostnames are included in the connection string in case the primary goes down.  








