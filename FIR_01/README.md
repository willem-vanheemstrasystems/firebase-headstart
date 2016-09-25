#firebase 01

Based on 'Firebase: Build a Real Time Web Chat App - Friendly Chat' at https://codelabs.developers.google.com/codelabs/firebase-web/#0

##1 Overview

In this codelab, you'll learn how to use the Firebase platform to easily create Web applications and you will implement and deploy a chat client using Firebase.

***What you'll learn***

- Sync data using the Firebase Realtime Database and Firebase Storage.

- Authenticate your users using Firebase Auth.

- Deploy your web app on Firebase static hosting.

***What you'll need***

- The IDE/text editor of your choice such as [WebStorm](https://www.jetbrains.com/webstorm), [Atom](https://atom.io/) or [Sublime](https://www.sublimetext.com/)

- The sample code.

- A browser such as Chrome.

##2 Get the sample code

Clone the [GitHub repository](https://github.com/firebase/friendlychat) from the command line:

```javascript
$ git clone https://github.com/firebase/friendlychat
```

The firebase-codelabs repository contains sample projects for multiple platforms. This codelab only uses these two repositories:

- ***web-start*** — The starting code that you'll build upon in this codelab.

- ***web*** — The complete code for the finished sample app.

Note: If you would like to run the finished app, you will still have to create a project in the Firebase console, see the ***Create a Firebase project and Setup your app*** section for instructions.

##3 Import the starter app

Open the starter app:

1. Using your IDE open or import the web-start directory from the sample code directory. This directory contains the starting code for the codelab which consists of a non functional UI.

##4 Create a Firebase project and Setup your app

###Create project

In the [Firebase console](https://console.firebase.google.com/) click on ***CREATE NEW PROJECT*** and call it ***FriendlyChat***.

###Initialize your app

In the Firebase Console, in the ***Overview*** click the ***Add Firebase to your web app** button.

You will find an HTML/JavaScript snippet:

```javascript
<script src="https://www.gstatic.com/firebasejs/3.4.0/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "AIzaSyDEdtLnRcCRuTpyKLbkgbCG6_BUSRCyr60",
    authDomain: "friendlychat-a29c5.firebaseapp.com",
    databaseURL: "https://friendlychat-a29c5.firebaseio.com",
    storageBucket: "",
    messagingSenderId: "1008728266001"
  };
  firebase.initializeApp(config);
</script>
```
___

Warning!

If your ```storageBucket``` attribute is empty like this:

```javascript
    databaseURL: "https://friendlychat-a29c5.firebaseio.com",
    storageBucket: "",
```

You've hit a bug. Sorry about that! Close and re-open this dialog and your ```storageBucket``` should now be populated.
___

Copy this snippet and paste it at the bottom of the ```index.html``` file where the ```TODO``` is located. This imports the Firebase JavaScript SDK and initializes it for your Firebase Project.

##Enable Google Auth

To sign users in we'll use Google auth which needs to be enabled.

In the ***Auth*** section > ***SIGN IN METHOD*** tab you need to enable the ***Google*** Sign-in Provider and click ***SAVE***.

##5 Install the Firebase Command Line Interface


##6 Run the starter app


##7 User Sign-in


##8 Read messages


##9 Database Security Rules [Optional]


##10 Send Messages


##11 Send Images


##12 Storage Security Rules [Optional]


##13 Deploy your app using Firebase static hosting


##14 Congratulations!
