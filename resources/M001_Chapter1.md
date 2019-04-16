## Databases, Collections and Documents  

- A Cluster contains Databases  
- Database Name serves as a NameSpace for Collections  
- Collections store individual records called Documents  
- Security policies can be applied at Database and Collection level  
- Authorisation at Document level not yet supported  
- Each database and collection combination define a namespace  
- Collections referenced by databaseName.collectionName  

![To Be Named](/images/DBsCollsDocs.jpg)  

### Documents: Scalar Value Types  
- Use schema view to see field types.  
- Field Name and Type shown plus graph with % of docs of that type.  

### Documents: Fields with Documents as Values  
- Fields can have Document types and contain nested documents.  

### Documents: Fields with Arrays as Values  
- Fields can have array types.  

### Geospatial Data  
- documents, arrays and geospatial are types of data recognized.  

### Geospatial Queries  
- Can select a centerpoint on map widget and drag to limit query size.  
- Geospatial Operators at https://docs.mongodb.com/manual/reference/operator/query-geospatial/  

### Filtering Collections with Queries  
- Filtering uses json key:value pairs {"key":"value"}  
- Key Values can be mongodb Documents.  

Example of Equality Filter: ``{"Street Name":"Old Road"}``  

Example of Range Filter: ``{"birth year":{"$gte":1985,"$lt":1990}}``  

### Understanding JSON  
- Mongodb is a Documents database using json modelling.  
- JSON Document and JSON Object are interchangable and mean the same thing.  
- ALL keys must be contained in double quotes.  
- Key and Value must be seperated by a colon.  
- JSON documentation at http://www.json.org/  
