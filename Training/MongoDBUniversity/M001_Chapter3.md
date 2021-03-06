## Mongodb Query Operators  

- All CRUD operations require filters and can all use query operators.  
- Complete list of operators can be found in the Mongodb documentation.  

https://docs.mongodb.com/manual/reference/operator/query/  

### Comparison Operators  

Examples:  

Find movies with a runtime of greater than 90 minutes.  
``db.movieDetails.find({runtime: {$gt: 90}})``  

Find movies with a runtime between 90 and 120 minutes and return only title and runtime.  
``db.movieDetails.find({runtime: {$gt: 90, $lt: 120}}, {_id: 0, title: 1, runtime: 1})``  

Add an '**e**' to the operator to change them to greater than or equal to, or less than or equal to e.g. **$gte** **$lte**  

The **$in** operator can only be used with arrays:  
``db.movieDetails.find({rated: {$in: ["G", "PG", "PG-13"]}}``  

Opposite to the above is **$nin** which matches non of the values in an array.  

### Element Operators  

**$exists** Matches documents that have the specified field.  
``db.movies.find({mpaaRating: {$exists: true}}).pretty()``  

**$type** Selects documents if a field is of the specified type.  
``db.movies.find({viewerRating: {$type: "int"}}).pretty()``  


