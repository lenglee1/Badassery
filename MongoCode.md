# Mongo Snippets

## Simple queries

db.collection.find().skip(10).limit(5) 
db.collection.findOne()


## Querying across collections

```js

//Declare an object called x. Assign to it a document from the collection lessons.
var x = db.lessons.findOne()
//In courses collection, find a document that has the course_id that is the same as the x object's course_id
db.courses.findOne(x.course_id)
//This is how we can do the 2 lines of above code in one line.
db.courses.findOne(db.lessons.findOne().course_id)
```






//Lists all the values associated with the name key in the courses collection
db.courses.find({},{name:1})

//Lists all the values associated with the name key in the courses collection, but only for published courses
db.courses.find({published:true},{name:1})

//Create object x. Assign one document in courses collection which has this name/key pair.
var x = db.courses.findOne({name:"Functions in JavaScript"})
//Now create 

//Get all the documents in the collection lessons that have a course_id that matches the id of the object x.
//I guess that this will be an array of objects.
var y = db.lessons.find({course_id:x._id})

//So have a lot of documents from lessons that all have the same course_id. Then for each lesson, we can get the total completed and the total started. 

//QUESTION: HOW EXACTLY DOES THE FOREACH COMMAND WORK AND WHAT IS THE ROLE OF Z?

y.forEach(function(z) {
print(z.t_c)
print(z.t_s)
})


//This is just more efficient code so we get the percentage straight away
y.forEach(function(z) {
percent = z.t_c/z.t_s
print(percent)
})


//Use the course_progresses and lesson_progresses collections
var x = db.courses.findOne({name:"Functions in JavaScript"})
//Get the number of courses that are completed for Functions in JS - numerator
db.course_progresses.count({c:true, course_id:x._id})
//Get the number of courses NOT completed for Functions in JS - add to numerator to get denominator
db.course_progresses.count({c:false, course_id:x._id})

//Alternative denominator for courses started
db.course_progresses.count({t_e:{$gt:0}})

//Print out names of each published course

var x = db.courses.find({"published":true})
x.forEach(function(y){
print(y.name)
})

//Run the mongoexport script

//Get total starated and total completed for published courses
./script/mongodb/exportMongodata courses name,t_c,t_s '{"published":true}' ./collection.csv

//Get the date created and number of exercises for every course
./script/mongodb/exportMongodata courses name,created_at,updated_at,language,num_exercises,published '{}' ./allcourses.csv


//Changed the value of a key in the user collection
db.users.update({email:"jefftchan@gmail.com"},{"$set":{is_admin:true}})




//Another example of forEach

//Start off with a user. Can use their email address to identify, or the hash from their profile page.

var user = db.users.findOne({_id:ObjectId("4e5b707ed9437f000100208c") });

//Create a variable course based on the course that has the slug that we are looking for (ie. functions in JS)


var course = db.courses.findOne({slug:"functions-in-javascript-2-0"});



//Here, got all the sections that match up to a particular course id. Each section is an object. We put all 5 section objects into the variable y.

 var y = db.sections.find({course_id:course._id})

// Want to count the number of exercises in each object. 


db.sections.find({course_id:course._id}).forEach(function (z) {print(z.exercises.length)})

//Debugging trick

 db.sections.find({course_id:course._id}).forEach(function (z) {print(1)})

//Time since the epic.
new Date(0)
new Date("Sat Apr 07 2012 14:11:56 GMT-0400 (EDT)")
var x = new Date("Feb 07 2012 14:11:56 GMT-0400 (EDT)")
var y = new Date("Feb 11 2012 14:11:56 GMT-0400 (EDT)")



var x = db.courses.find({published:true})
x.forEach(function(y){
	print(y.name)
})

1. x is a variable with elements (ie. an array, a cursor etc. a list of stuff separated by comma)
2. we want to do something to each element in that variable. so we use forEach. 
3. what do we want to do to each element? that is defined by the function (the code underneath this first line)
4. this function normally takes an argument
5. in this case, the argument is y
6. what does y represent?
7. y represents each individual element in the collection forEach is operating on (ie. x). 
8. so here, y is each of the objects in x (ie. a published course)
9. and we use dot notation to get the name of each published course



//Map gives me back an array. The array it gives me is going to be whatever I return in every iteration. 

var x = db.courses.find({published:true})
var publishedCourseIds = x.map(function(y){
	return y._id;
	});



//number of courses started
publishedCourseIds.forEach(function(y){ 
print (db.course_progresses.find({c_id:y}).count());
});


//Completed published courses
publishedCourseIds.forEach(function(y){ 
print (db.course_progresses.find({c_id:y, c:true}).count());
});


//Alternative approach
x.forEach(function(y){
	print (db.course_progresses.find({c_id:y._id}).count());
});


//Steps taken to change the author of a course

1. var x = db.users.findOne({email:"me@davidhu.me"})
This gets the document (ie. object) for David Hu.
2. var david_ID = x._id
This creates the variable david_ID which is equal to the value associated with the object's _id key.
3. db.courses.findOne({slug:"week-14-more-html"})
Doing this so can get the course - then will get the _id for the course (ie. the course id)
4. db.courses.update({_id:ObjectId("4f70dc4045c98a000300345f")},{"$set":{user_id:david_ID}})
This updates the user_id (ie. the author) of this course to the variable david_ID








