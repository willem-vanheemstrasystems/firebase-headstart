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

###Authorize Firebase with Google

When the user clicks the ***Sign in with Google*** button the ```FriendlyChat.prototype.signIn``` function gets triggered (we already set that up for you!). At this point we want to authorize Firebase using Google as the Identity Provider. We'll sign in using a popup ([Several other methods](https://firebase.google.com/docs/auth/web/google-signin) are available). Change the ```FriendlyChat.prototype.signIn``` function with:

```javascript
// Signs-in Friendly Chat.
FriendlyChat.prototype.signIn = function() {
  // Sign in Firebase using popup auth and Google as the identity provider.
  var provider = new firebase.auth.GoogleAuthProvider();
  this.auth.signInWithPopup(provider);
};
```

The ```FriendlyChat.prototype.signOut``` function is triggered when the user clicks the ```Sign out``` button. Add the following line to make sure we sign out of Firebase:

```javascript
// Signs-out of Friendly Chat.
FriendlyChat.prototype.signOut = function() {
  // Sign out of Firebase.
  this.auth.signOut();
};
```

We want to display the signed-in user's profile pic and name in the top bar. Earlier we've set up the ```FriendlyChat.prototype.onAuthStateChanged``` function to trigger when the auth state changes. This function gets passed a Firebase ```User``` object when triggered. Change the two lines with a ```TODO``` to read the user's profile pic and name:

```javascript
// Triggers when the auth state change for instance when the user signs-in or signs-out.
FriendlyChat.prototype.onAuthStateChanged = function(user) {
  if (user) { // User is signed in!
    // Get profile pic and user's name from the Firebase user object.
    var profilePicUrl = user.photoURL; // Only change these two lines!
    var userName = user.displayName;   // Only change these two lines!

    ...
```

We display an error message if the users tries to send messages when the user is not signed-in. To detect if the user is actually signed-in ***add*** these few lines to the top of the ```FriendlyChat.prototype.checkSignedInWithMessage``` function where the ```TODO``` is located:

```javascript
// Returns true if user is signed-in. Otherwise false and displays a message.
FriendlyChat.prototype.checkSignedInWithMessage = function() {
  // Return true if the user is signed in Firebase
  if (this.auth.currentUser) {
    return true;
  }

  ...
```

###Test Signing-in to the app.

1. Reload your app if it is still being served or run ```firebase serve``` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) and open it in your browser.

2. Sign-In using the Sign In button

3. After Signing in the profile pic and name of the user should be displayed

NOTE: You might get this error message in your web browser's console: The given sign-in provider is disabled for this Firebase project. Enable it in the Firebase console, under the sign-in method tab of the Auth section.

In our case we had to 'enable' Google as the sign-in provider.

##8 Read messages

###Import Messages

In your project in Firebase console visit the ***Database*** section on the left navigation bar. In the overflow menu of the Database select ***Import JSON***. Browse to the ```initial_messages.json``` file in the starter directory, select it then click the ***Import*** button. This will replace any data currently in your database. You could also edit the database directly, using the green + and red x to add and remove items manually.

After importing the JSON file your database should contain the following elements:

```javascript
friendlychat-12345/
    messages/
        -K2ib4H77rj0LYewF7dP/
            text: "Hello"
            name: "anonymous"
        -K2ib5JHRbbL0NrztUfO/
            text: "How are you"
            name: "anonymous"
        -K2ib62mjHh34CAUbide/
            text: "I am fine"
            name: "anonymous"
```

These are a few sample chat messages to get us started with reading from the Database.

###Synchronize Messages

To synchronize messages on the app we'll need to add listeners that triggers when changes are made to the data and then create a UI element that show new messages.

Add code that listens to newly added messages to the app's UI. To do this modify the ```FriendlyChat.prototype.loadMessages``` function. This is where we'll register the listeners that listens to changes made to the data. We'll only display the last 12 messages of the chat to avoid displaying a very long history on load.

```javascript
// Loads chat messages history and listens for upcoming ones.
FriendlyChat.prototype.loadMessages = function() {
  // Reference to the /messages/ database path.
  this.messagesRef = this.database.ref('messages');
  // Make sure we remove all previous listeners.
  this.messagesRef.off();

  // Loads the last 12 messages and listen for new ones.
  var setMessage = function(data) {
    var val = data.val();
    this.displayMessage(data.key, val.name, val.text, val.photoUrl, val.imageUrl);
  }.bind(this);
  this.messagesRef.limitToLast(12).on('child_added', setMessage);
  this.messagesRef.limitToLast(12).on('child_changed', setMessage);
};
```

###Test Message Sync

1. Reload your app if it is still being served or run ```firebase serve``` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000) and open it in your browser.

2. The sample messages we imported earlier into the database should be displayed in the Friendly-Chat UI. You can also manually add new messages directly from the ***Database*** section of the Firebase console. Congratulations, you are reading real-time database entries in your app!

##9 Database Security Rules [Optional]

###Set Database Security Rules

The Firebase Realtime Database uses a specific [rules language](https://firebase.google.com/docs/database/security/) to define access rights, security and data validations.

New Firebase Projects are setup with default rules that only allow authenticated users to use the Realtime Database. You can view and modifies these rules in the ***Database*** section of [Firebase console](https://console.firebase.google.com/) under the ***RULES*** tab. You should be seeing the default rules: 

```javascript
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

The ```auth``` rule variable is a special variable containing information about the user if authenticated. More information can be found in the [documentation](https://firebase.google.com/docs/database/security/).

You can update these rules manually directly in the Firebase console. Alternatively you can write these rules in a file and apply them to your project using the CLI:

- Save the rules shown above in a file named ```database-rules.json```.

- In your ```firebase.json``` file, add a ```database.rules``` attribute pointing to a JSON file containing the rules shown above (the ```hosting``` attribute should already be there):

```javascript
{
  "database": {
    "rules": "database-rules.json"
  },
  "hosting": {
    "public": "./",
    "ignore": [
      "firebase.json",
      "database-rules.json",
      "storage.rules"
    ]
  }
}
```

###Deploy Database Security Rules

You can then deploy these rules with the Firebase CLI using:

```javascript
$web-start> firebase deploy

=== Deploying to 'friendlychat-a29c5'...

i  deploying database, hosting
+  database: rules ready to deploy.
i  hosting: preparing ./ directory for upload...
+  hosting: ./ folder uploaded successfully
+  hosting: 8 files uploaded successfully
i  starting release process (may take several minutes)...

+  Deploy complete!

Hosting Site: https://friendlychat-a29c5.firebaseapp.com
Dashboard: https://console.firebase.google.com/project/friendlychat-a29c5/overview

Visit the URL above or run firebase open
```

##10 Send Messages


##11 Send Images


##12 Storage Security Rules [Optional]


##13 Deploy your app using Firebase static hosting


##14 Congratulations!
