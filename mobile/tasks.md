# Tasks
This file contains tasks related to Android mobile reversing.

---

## Getting started in 1-2-3

#### Task 1: Download APK
Go to https://github.com/dineshshetty/Android-InsecureBankv2 
and clone or download to InsecureBankv2 APK.

#### Task 2: Unzip APK file
APK's are zipped files. Rename the APK so that it has the
_.zip_ file extension, and unzip the file. Open 
`AndroidManifest.xml`. Is it readable?

#### Task 3: Decompile APK
Decompile APK to get readable code using **apktool**. Open the 
project folder in **Android Studio**. Now, open the
_AndroidManifest.xml_ file. Can you read anything now? Open 
some code files as well. Do you recognize the language?

#### Task 4: Extract java source code
Extract Java source code from the APK using **jadx-gui**.
Now you are ready to find some juicy information by doing
some mobile app reversing!! Whoop whoop!

#### Task 5: Rebuild APK
In many CTF tasks, you will have to do changes in the app code
and then rebuild the app and run it again on the phone or 
emulator. A common task is for instance to change the code so
that you win thousand rounds of a game. In order to get the flag,
you will have to rebuild the app, sign it and then install it 
again.

Make a change. It sdoes not matter what you do, just make sure
the code is runnable. And then rebuild the app using **apktool**
and **jarsigner**. Remember that you also need a keystore to sign
the app. Make a keystore using **keytool**.

---

## Important note!
In order to run you app properly, it requires a server to run.
You must start the server of the InsecureBankv2 app. Do that by
going to the `AndroLabServer` folder, and entering the following 
commands (requires Python):

```sh
$ pip install -r requirements.txt
$ python app.py
The server is hosted on port: 8888
```

Or follow instructions from the Github repository:
https://github.com/dineshshetty/Android-InsecureBankv2/tree/master/AndroLabServer

## Insecure data storage

### Exploiting local data storage
Shared Preference files are similar to Android property files.
Shared Preferences are used for storing key-value pairs of 
primitive data types. For example, Shared Preferences are used 
to store user’s settings and application data.

#### Task 1: Find and read the Android Shared Preferences
1. Find the package name 
2. Copy all preferences to your machine.
3. Does the file contain anything juicy?

#### Task 2: Exploring databases
1. Where are the database(s) for this application located?
2. Copy the database out of the device
3. What type of database is this?
4. Dump the schema
5. List all tables
6. Dump all the tables

---

## Insecure communication

### Exploiting insecure communication

### Task 1: Insecure Communication
What happens when you log in?
1. What protocol is used?
2. Is it encrypted?
3. Can you see any sensitive data?
4. Can you identify other API endpoints?

Forging requests
1. Can you change the password of `dinesh` without logging in first?
2. Any other requests you can forge?

---

## Extraneous Functionality

### Exploiting Extraneous Functionality

#### Task 1: Extraneous Functionality
1. Can you find any backdoors or other developer tools left in the app?
2. What can these be used for?
3. If they pose a security risk, what would you do to fix it?
4. Enable debugging on an app that doesn't have it enabled by default.
