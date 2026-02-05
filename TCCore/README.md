![alt tag](../res/ca_logo.png)


Core Guide
==========

Last update : *05/02/2026*
Release version : *5.4.9*

## Table of Contents

- [Core Guide](#core-guide)
- [Introduction](#introduction)
- [Dependencies](#dependencies)
  - [Using ProGuard](#using-proguard)
- [Support and contacts](#support-and-contacts)

Introduction
============

As we expended Commanders Act's mobile possibilities, it rapidly became apparent that we could not fit everything at the same time for everybody in only one big library. In fact, it's contradictory with Tag Management itself, so we decided to cut capabilities into smaller modules so people wouldn't need to add everything in their project, saving space in your application.

But even with that said, a part of our code is pretty useful in several of our modules, so we created a Core module to prevent code repetition and thus also bigger applications if you need several modules.

Dependencies
============

The Core module is mandatory if you are using Commanders Act's mobile solution, so we simply put the dependencies needed for the Core module directly in the documentations of the other modules.

Core is building with the following dependencies :

	implementation 'androidx.appcompat:appcompat:1.4.1'

Using ProGuard
--------------

If your release build uses ProGuard to strip and obfuscate your code, you will need to add some configuration for the modules to keep working.

Just add the following line in your proguard-rules.pro.

```
-keep class com.tagcommander.lib.** { *; }
```

You may also need to add the following rule depending on your other libraries dependencies

```
    -keep class org.json.** { *; }
```

Support and contacts
====================
![alt tag](../res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com
***

This documentation was generated on 05/02/2026 14:40:02
