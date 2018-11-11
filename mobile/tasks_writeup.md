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
