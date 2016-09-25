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
    storageBucket: "friendlychat-a29c5.appspot.com",
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

The Firebase Command Line Interface (CLI) will allow you to serve your web apps locally and deploy your web app to Firebase hosting.

To install the CLI you need to have installed [npm](https://www.npmjs.com/) which typically comes with [NodeJS](https://nodejs.org/en/).

To install the CLI run the following npm command:

```javascript
$~> npm -g install firebase-tools
```
___
Doesn't work? You may need to [change npm permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions).
___

To verify that the CLI has been installed correctly open a console and run:

```javascript
$~> firebase version
3.x.x
```

Make sure the Firebase version is 3.x.x. If this reads 2.x.x you still have an older firebase CLI version installed and you may need to fix your PATH.

Authorize the Firebase CLI by running:

```javascript
$~> firebase login --interactive
```

Make sure you are in the ```web-start``` directory then set up the Firebase CLI to use your Firebase Project:

```javascript
$~> firebase use --add --interactive
```

Then select your Project ID and follow the instructions.

##6 Run the starter app

Now that you have imported and configured your project you are ready to run the app for the first time. Open a console at the ```web-start``` folder and run:

```javascript
$web-start> firebase serve
Listening at http://localhost:5000
```

The web app should now be served from [http://localhost:5000](http://localhost:5000/) open it. You should see your app's not (yet!) functioning UI.

The app cannot do anything right now but with your help it will soon! We only have laid out the UI for you so far. Let's now build a realtime chat!

##7 User Sign-in

###Initialize Firebase Auth

The Firebase SDK should be ready to use since you have imported and initialized it in the ```index.html``` file. In this application we'll be using the [Firebase Realtime Database](https://firebase.google.com/docs/database), [Firebase Storage](https://firebase.google.com/docs/storage/) and [Firebase Authentication](https://firebase.google.com/docs/auth/). 

Modify the ```FriendlyChat.prototype.initFirebase``` function in the ```scripts/main.js``` file so that it sets some shortcuts to the Firebase SDK features and initiates auth:

```javascript
// Sets up shortcuts to Firebase features and initiate firebase auth.
FriendlyChat.prototype.initFirebase = function() {
  // Shortcuts to Firebase SDK features.
  this.auth = firebase.auth();
  this.database = firebase.database();
  this.storage = firebase.storage();
  // Initiates Firebase auth and listen to auth state changes.
  this.auth.onAuthStateChanged(this.onAuthStateChanged.bind(this));
};
```






##8 Read messages


##9 Database Security Rules [Optional]


##10 Send Messages


##11 Send Images


##12 Storage Security Rules [Optional]


##13 Deploy your app using Firebase static hosting


##14 Congratulations!
