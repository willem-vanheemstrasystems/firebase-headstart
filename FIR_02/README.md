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

##Relations or Entity Relationship Diagram

Requirement for Schema Relations:

1. Any user can be a part of a group.

2. Any group can include multiple users.

3. An update can be targeted for a single user or a particular group.

##Maintaining Relations in the SQL DBMS

We know that in order to implement relations in a SQL database we would need to create extra tables like 'user_belong_to_groups' with reference to user's primary key 'userid' and group's primary key 'groupid'. This table is very helpful during data retrieval because of SQL joins but in firebase you do not have joins.

#Maintaining Relations in Firebase

In Firebase we cannot use extra objects like 'user_belong_to_groups' to store the many to many relations, because Firebase does not have any query language or any kind of joins.

The next thought in our mind comes that we can nest data & objects. For example, I can have a Group object which can have a property users consisting an array of users and each user can have an array of updates. The data will look like something given below:

```javascript
// JSON Structure for Data which has nested Objects
"data":{
  "groups":{
     "group1"{
            "group_name":"Administrators",
            "group_description":"Users who can do anything!",
            "no_of_users":2,
            "users":{
                "user1": {
                    "username":"john",
                    "full_name":"John Vincent",
                    "created_at":"9th Feb 2015",
                    "updates": {
                            "update1":{
                                "update_text":"New feature launched!",
                                "created_at":"13th Feb 2015",
                                "sent_by":"user2"
                        }
                    }
                },
                "user2": {
                    "username":"chris",
                    "full_name":"Chris Mathews",
                    "created_at":"11th Feb 2015"
                }
            }
        },
     "group2"{
            "group_name":"Moderators",
            "group_description":"Users who can only moderate!",
            "no_of_users":1,
            "users":{
                "user1": {
                    "username":"john",
                    "full_name":"John Vincent",
                    "created_at":"9th Feb 2015"
                }
            },
            ,
            "updates": {
                    "update2":{
                        "update_text":"Users should expect blackout tomorrow!",
                        "created_at":"19th Feb 2015",
                        "sent_by":"user1"
                }
            }
        }
    }
}
```

The problems with the above structure can be well analysed when you have a close look on the data. The issues with this type of structure for your data are

1. If the same user belongs to 2 groups we have to duplicate the complete user object which creates redundancy of data.

2. Accessing any user object directly using userid will be an issue.

3. It is very cumbersome to find all the groups a user belongs to.

4. We have to create duplicate updates for showing it to multiple recipients.

5. We cannot set proper permissions in this kind of structure.

##Solving the Problem

###1. Flatten (Denormalize) Your Data

###2. Storing One to Many Relations or Static Lists

###3. Maintaining Many to Many Relations

#Conclusion
