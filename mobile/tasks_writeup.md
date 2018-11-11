# Tasks
This file contains tasks related to Android mobile reversing.

## Insecure data storage

### Exploiting local data storage

#### Task 1: Find and read the Android Shared Preferences
_1. Find the package name_

This requires root. With images that use Google APIs,
you may easily get root by entering the following command:
```
$ adb root
```

Without Google API, you may 

Another way, and how I chose to solve this, was using _su_
as in the following snippet:
```bash
Android-InsecureBankv2> adb shell
generic_x86:/ $ su
generic_x86:/ # pm list packages | grep <packagename>
```

_2. Copy all preferences to your machine._

Often you would like to have files locally on your machine,
and not on the device you are reversing. To do this, you
need root on the device. However, using _su_ will not do the
magic for us now as that is just within the shell. With images 
that use Google APIs, you may use:
```bash
$ adb root
```

And then, you can use another _adb_ command to pull the file
to you local machine:
```bash
$ adb pull /data/data/<package_name>/shared_prefs/<preference_file>.xml
```

_3. Does the file contain anything juicy?_

Hellz yeah, it does!
```bash
Android-InsecureBankv2> cat .\mySharedPreferences.xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="superSecurePassword">DTrW2VXjSoFdg0e61fHxJg==&#10;    </string>
    <string name="EncryptedUsername">ZGluZXNo&#13;&#10;    </string>
</map>
```

#### Task 2: 
1. Where are the database(s) for this application located?
`/data/data/com.android.insecurebankv2/databases`

2. Copy the database out of the device
`adb pull /data/data/com.android.insecurebankv2/databases .`

3. What type of database is this?
SQLite 3:
```bash
$ cd databases
$ file mydb
mydb: SQLite 3.x database, user version 1, last written using SQLite version 3019004
```

4. Dump the schema
```bash
$ sqlite3 mydb
SQLite version 3.22.0 2018-01-22 18:45:57
Enter ".help" for usage hints.
sqlite> .schema
CREATE TABLE android_metadata (locale TEXT);
CREATE TABLE names (id INTEGER PRIMARY KEY AUTOINCREMENT,  name TEXT NOT NULL);
CREATE TABLE sqlite_sequence(name,seq);
```

5. List all tables
```bash
sqlite> .tables
android_metadata  names
```

6. Dump all the tables
```bash
sqlite> .dump names
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE names (id INTEGER PRIMARY KEY AUTOINCREMENT,  name TEXT NOT NULL);
INSERT INTO names VALUES(1,'dinesh');
COMMIT;
sqlite> .dump android_metadata
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE android_metadata (locale TEXT);
INSERT INTO android_metadata VALUES('en_US');
COMMIT;
```
