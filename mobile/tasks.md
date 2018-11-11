# Tasks
This file contains tasks related to Android mobile reversing.

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

## Extraneous Functionality

### Exploiting Extraneous Functionality

#### Task 1: Extraneous Functionality
1. Can you find any backdoors or other developer tools left in the app?
2. What can these be used for?
3. If they pose a security risk, what would you do to fix it?
4. Enable debugging on an app that doesn't have it enabled by default.
