#firebase 02

Based on 'Structuring your Firebase Data correctly for a Complex App' at https://www.airpair.com/firebase/posts/structuring-your-firebase-data


#What is Firebase ?

Firebase is a trending Cloud based ([BaaS](http://en.wikipedia.org/wiki/Mobile_Backend_as_a_service)) backend as a service which provides Real Time syncing with all the clients subscribed to the server at any given instance. Firebase supports multiple platforms/frameworks like AngularJS, EmberJS, Backbone.js, iOS7, OSX, Android and programming languages like Ruby, Node.js, Python, Java, Clojure, PHP, Perl & Javascript. Firebase relies on web sockets to update all the clients about any data changes instantly.

#Data In Firebase

##Storage Format

Data is stored in firebase as a large JSON document. It is the same case as it is done in most of the NoSQL database systems like MongoDB, Cassandra, CouchDB etc. The data is stored as a large objects which can hold key value pairs where value can be a string, number or another object. 

##Managing Data in Firebase

Firebase provides an excellent GUI to manage your data. The awesome part of it is that it is real real-time. You do not have to refresh your browser to see updated data as opposed to other DBMS GUIs. It shows the newly added, modified or deleted data objects using colors like green, yellow & red.

#Problem in Structuring your Data for Firebase

We all have been storing our data in a SQL based database and accustomed to relational data hierarchy from a long time. NoSQL world is quite different and finalizing the schema for your Firebase data will be a problem for you initially. I have seen a lot of queries and questions from people all around the world but no definitive guide or answer to this problem. In this article we will try to understand the problem in depth and find an elegant solution to deal with it.

##Example Data Schema

In order to understand the problem, we will define the data schema we want to implement in Firebase for an imaginary application. I will take a common example of an app where we have three entities Users, Groups & Updates. The app is intended to send updates to a specific user or a Group consisting of multiple users.

###Properties or fields for Entities

| User       | Group             | Update      |
|:----------:|:-----------------:|:-----------:|
| username   | group_name        | update_text |
| full_name  | group_description | created_at  |
| created_at | no_of_users       | sent_by     |


| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |



##Relations or Entity Relationship Diagram

##Maintaining Relations in the SQL DBMS

#Maintaining Relations in Firebase

##Solving the Problem

###1. Flatten (Denormalize) Your Data

###2. Storing One to Many Relations or Static Lists

###3. Maintaining Many to Many Relations

#Conclusion
