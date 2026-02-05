![CommandersAct logo](./res/ca_logo.png)


Android Developers' Implementation Guide
================================

Last update : *05/02/2026

Release version : *5*

## Table of Contents

- [Android Developers' Implementation Guide](#android-developers-implementation-guide)
- [Introduction](#introduction)
- [Latest available versions](#latest-available-versions)
- [Adding a module to your project](#adding-a-module-to-your-project)
  - [mavenCentral](#mavencentral)
  - [Jar file](#jar-file)
  - [Aar file](#aar-file)
  - [Demo Application](#demo-application)
- [Support and contacts](#support-and-contacts)

Introduction
============

TagCommander for mobile is a collection of small SDKs each designed to serve a dedicated purpose.
The modules are the following :

[Core : Used as a base by the other modules.](TCCore/README.md)

[ServerSide : Tag management system collecting data through a server-side approach.](TCServerSide/README.md)

[Consent : Pass the Consent settings to our tag system](TCConsent/README.md)


For each of those modules, please check their respective documentation for more information.


Latest available versions
=========================


Core : *5.4.9*

ServerSide : *5.5.8*

Consent : *5.3.10*

IAB : *5.1.1*

Partners : *5.0.1*

FirebaseDestination : *5.1.4*

Adding a module to your project
===============================

If you want to add a module to your android project, you have several possibilities.

	- Using mavenCentral to manage the dependency.
	- Using the aar file in your project.


Both of them will require you to modify a bit your build.gradle.

mavenCentral
------------

The easiest way is to go with mavenCentral. It will help you get updates on the module on a regular basis without doing much work.

/!\ The IAB module is not on mavenCentral yet, so it will need to be added manually with the aar or jar.

If it's not present in your project's build.gradle add mavenCentral() in the repository list for the dependency management. It will look something like that:

	allprojects {
    	repositories {
        	mavenCentral()
    	}
	}

Then in your application's build.gradle always add the core module:

	implementation 'com.tagcommander.lib:core:5.4.9'

And in addition to the core module you can add the other modules you need the same way. See each module's documentation for more specific information.

For example:

	implementation 'com.tagcommander.lib:ServerSide:5.5.8'

Jar file
--------

If you'd rather use the jar files directly in your project, you can get them from our github account: https://github.com/CommandersAct/AndroidV5

> [!WARNING]
>  You will always need to at least add the Core module to your project.

After you downloaded the modules you need, add them to your libs folder and either ask gradle to compile with all the jars in your lib directory or directly with the chosen files.

	// All the jars.
	compile fileTree(dir: 'libs', include: '*.jar')
	// Specific files
	compile files('libs/TCCore-release-5.4.9.jar')
	compile files('libs/TCServerSide-release-5.5.8.jar')
	compile files('libs/TCConsent-release-5.3.10.jar')

Aar file
--------

If you'd rather use the aar files directly in your project, you can get them from our github account: https://github.com/CommandersAct/AndroidV5

> [!WARNING]
>  You will always need to at least add the Core module to your project.

To be able to compile with the aar files, you will first need to tell gradle how to use them properly. In your project's build.gradle complete your repository list with 'flatDir' list in the following exemple:

    allprojects {
	    repositories {
	        mavenCentral()
	        flatDir {
	            dirs 'libs'
	        }
	    }
	}

After you downloaded the modules you need, add them to your libs folder and ask gradle to compile with them.

    compile (name:'TCCore-release-5.4.9', ext:'aar')
    compile (name:'TCServerSide-release-5.5.8', ext:'aar')
    compile (name:'TCConsent-release-5.3.10', ext:'aar')



Demo Application
----------------

You can find a full example of a working app integrating our libraries in the following repo :

https://github.com/CommandersAct/tcmobiledemo-v5


Support and contacts
====================
![CommandersAct logo](./res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com

Commanders Act | 7b rue taylor - 75010 PARIS - France
***

This documentation was generated on 05/02/2026 14:40:02
