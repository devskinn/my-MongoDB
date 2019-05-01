Using Compass to analyse blog post data

Resources used from course:
MongoDB-Practical-Slides-Section6.pdf
P71-Section6-Dataset.zip

Procedures:

Quantitative Fields: likes, dislikes, views, date_created
1. Project - Set the fields required and leave these set
2. Get min (1) and max (500) values for required fields
3. Get Midpoint 1+500/2 = 251
4. Get all posts with likes >=251 Number found at 'Displaying documents ...' (509)
5. Get all posts with likes < 251 (491)
6. Clear 'Filter' box and repeat 2-5 for other fields (dislikes and Views)
7. Clear filter and sort by date_created to get oldest and newest posts and the midpoint value
8. Get posts above and below the midpoint value (1st July 2018)
9. Create summary chart of findings Min - Max - Midpoint - $lt midpoint+% - $gte midpoint+%

Categorical Fields: topic
1. 


Expand Options:

FILTER
{likes: {$gte: 251}}
{likes: {$lt: 251}}
Midpoint syntax for Date object: {date_created: {$gte: new Date("2018-07-01")}}

PROJECT
Used to identify only the fields required for the analysis.
{topic: 1, likes: 1, dislikes: 1, views: 1, date_created: 1}

SORT
Used to sort the data
Get Min No of Likes: {likes: 1}  will return in Ascending order (min to max)
Get Max No of Likes: {likes: -1} will return in Descending order (max to min)
Get most recent posts: {date_created: -1}

COLLATION

Example Document:
_id: 5c9fee8966d8c1392df11009
post_id: 349
user_id: 72
body: "Lorem ipsum dolor sit amet"
topic: "music"
likes: 500
dislikes: 300
views: 808
date_created: 2018-07-12T12:08:12.000+00:00
