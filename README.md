![CommandersAct logo](./res/ca_logo.png)


Android Developers' Implementation Guide
================================

![Platform](https://img.shields.io/badge/platform-Android-3DDC84?logo=android&logoColor=white)
![Min SDK](https://img.shields.io/badge/minSdk-21-blue)
![Maven Central](https://img.shields.io/badge/Maven%20Central-available-orange?logo=apachemaven&logoColor=white)
![CI](https://img.shields.io/badge/CI-passing-brightgreen?logo=gitlab)

Last update : *18/06/2026*

Release version : *5*

## Table of Contents

- [Android Developers' Implementation Guide](#android-developers-implementation-guide)
- [Introduction](#introduction)
- [Latest available versions](#latest-available-versions)
- [Adding a module to your project](#adding-a-module-to-your-project)
  - [mavenCentral](#mavencentral)
  - [Aar file](#aar-file)
  - [Demo Application](#demo-application)
- [Support and contacts](#support-and-contacts)

Introduction
============

TagCommander for mobile is a collection of small SDKs each designed to serve a dedicated purpose.

| Module | Purpose | Documentation |
|--------|---------|---------------|
| **Core** | Base module required by all others | [Core README](TCCore/README.md) |
| **ServerSide** | Tag management via server-side data collection | [ServerSide README](TCServerSide/README.md) |
| **Consent** | Privacy & consent management | [Consent README](TCConsent/README.md) |
| **IAB** | IAB TCF v2 consent string support | [IAB README](TCConsent/TCIAB/README.md) |
| **FirebaseDestination** | Firebase Analytics destination | [Firebase README](TCFirebaseDestination/README.md) |

> [!NOTE]
> The **Core** module is always required regardless of which other modules you use.

Latest available versions
=========================

Core : *5.5.0*

ServerSide : *5.6.0*

Consent : *5.4.0*

IAB : *5.1.1*

Partners : *5.0.1*

FirebaseDestination : *5.1.4*

Minimum Android SDK version: 21

Adding a module to your project
===============================

Both methods require modifying your `build.gradle`. The Core module must always be included.

mavenCentral
------------

The recommended approach. Keeps modules up to date with minimal effort.

Add `mavenCentral()` to your project's `build.gradle` if not already present:

	allprojects {
    	repositories {
        	mavenCentral()
    	}
	}

Then in your application's `build.gradle`, always add Core first:

	implementation 'com.tagcommander.lib:core:5.5.0'

Add any other modules you need the same way. See each module's documentation for details.

Example:

	implementation 'com.tagcommander.lib:ServerSide:5.6.0'

Aar file
--------

Download from: https://github.com/CommandersAct/AndroidV5

> [!NOTE]
> You will always need to at least add the Core module to your project.

Add `flatDir` to your project's `build.gradle`:

    allprojects {
	    repositories {
	        mavenCentral()
	        flatDir {
	            dirs 'libs'
	        }
	    }
	}

Then reference the modules in your application's `build.gradle`:

    compile (name:'TCCore-release-5.5.0', ext:'aar')
    compile (name:'TCServerSide-release-5.6.0', ext:'aar')
    compile (name:'TCConsent-release-5.4.0', ext:'aar')


Demo Application
----------------

A full working example integrating our libraries:

https://github.com/CommandersAct/tcmobiledemo-v5


Support and contacts
====================
![CommandersAct logo](./res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com

Commanders Act | 25 rue de Tolbiac, 75013 Paris - France
***

This documentation was generated on 18/06/2026 09:00:32