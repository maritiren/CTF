# Tasks
This file contains tasks related to Android mobile reversing.

## Insecure data storage

### Exploiting local data storage

#### Task 1: Find and read the Android Shared Preferences
1. Find the package name
```bash
Android-InsecureBankv2> adb shell
generic_x86:/ $ su
generic_x86:/ # pm list packages | grep <packagename>
```

2. Copy all preferences to you machine.


3. Does the file contain anything juicy?

#### Task 2: 
