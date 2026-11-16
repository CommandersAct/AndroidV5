![alt tag](../res/ca_logo.png)


Core Guide
==========

Last update : *12/11/2026*
Release version : *5.6.0*

## Table of Contents

- [Core Guide](#core-guide)
- [Introduction](#introduction)
- [Dependencies](#dependencies)
  - [Using ProGuard](#using-proguard)
- [Support and contacts](#support-and-contacts)

Introduction
============

As we expanded Commanders Act's mobile possibilities, it rapidly became apparent that fitting everything into one big library wasn't the right approach — it's also contradictory with Tag Management itself. We decided to split capabilities into smaller modules, so you only add what you need and keep your application lightweight.

That said, a part of our code is shared across several modules. The Core module exists to avoid repetition and keep things smaller when you need more than one module.

Dependencies
============

The Core module is mandatory when using Commanders Act's mobile solution. Its dependencies are listed directly in each module's documentation rather than here.

Core is building with the following dependencies :

	implementation 'androidx.appcompat:appcompat:1.6.1'

Using ProGuard
--------------

If your release build uses ProGuard/R8 to strip and obfuscate your code, Core ships a consumer rule automatically — `-keep class com.tagcommander.lib.** { *; }` — so you don't need to add it yourself. This matters because some modules (ServerSide, Consent) detect each other and TCIAB/FirebaseDestination at runtime via reflection rather than a compile dependency; without this rule, obfuscation could silently break consent-gating, Firebase-forwarding, or IAB support.

> [!NOTE]
> This consumer rule is not a substitute for your own care around obfuscation — your app's own ProGuard/R8 configuration can still override or strip it.

You may also need to add the following rule yourself, depending on your other library dependencies:

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

This documentation was generated on 12/11/2026 16:22:52