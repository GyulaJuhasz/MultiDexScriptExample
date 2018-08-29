# UDATE

The bug was fixed with the release of *26.0.0* of build tools by [this commit](https://android.googlesource.com/platform/dalvik/+/33f274bde1e1aa248c5cf78635d571770dd07179).

# MultiDexScriptExample

This example project demonstrates the bug in mainDexClasses.bat in the build tools folder.

## Prerequisites

To build this test project and examine the bug, the following are needed:

* Windows OS
* Maven (tested with version 3.3.9)
* Internet access (to download the dependencies)

## How to test the bug

Run the following command in the folder of the cloned Git repository:

```
mvn -X clean install
```

In the build log, the following can be seen:

```
[INFO] Invalid option 
[INFO] Usage:
[INFO] 
[INFO] Short version: Don't use this.
[INFO] 
[INFO] Slightly longer version: This tool is used by mainDexClasses script to build
[INFO] the main dex list.
```

The executed command can be seen in the build log too. On my machine, it was the following:

```
C:\android-sdk\build-tools\25.0.2\mainDexClasses.bat --output C:\IdeaProjects\MultiDexScriptExample\target\mainDexClasses.txt "C:\IdeaProjects\MultiDexScriptExample\target\classes"
```

It can be seen that mainDexClasses.bat script is called with valid arguments, but the script run the MainDexListBuilder Java class with
wrong arguments.

Because of this bug, the generated classes.dex will be empty - very little size with no classes in it -, and this will cause an
**INSTALL_FAILED_DEXOPT** error when trying to install the APK on an older API level device with Dalvik (eg. API 19).
