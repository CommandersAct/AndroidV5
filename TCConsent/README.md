![alt tag](../res/ca_logo.png)


Consent's Implementation Guide
==============================

Last update : *05/02/2026*

Release version : *5.3.10*

## Table of Contents

- [Consent's Implementation Guide](#consents-implementation-guide)
- [Introduction](#introduction)
- [Choose your privacy](#choose-your-privacy)
- [Setup](#setup)
  - [Minimum Requirements](#minimum-requirements)
  - [With the ServerSide](#with-the-serverside)
  - [Standalone](#standalone)
- [Saving consent](#saving-consent)
  - [With the Privacy Center](#with-the-privacy-center)
  - [Manually displayed consent](#manually-displayed-consent)
- [Stop privacy stats tracking](#stop-privacy-stats-tracking)
  - [Forwarding to Server-Side](#forwarding-to-server-side)
  - [AcceptAll / RefuseAll](#acceptall-refuseall)
- [Retaining consent](#retaining-consent)
  - [Using your own user ID](#using-your-own-user-id)
  - [Displaying chosen ID](#displaying-chosen-id)
- [Displaying consent](#displaying-consent)
- [Reacting to consent](#reacting-to-consent)
  - [Resetting consent :](#resetting-consent)
- [Forwarding consent to webViews](#forwarding-consent-to-webviews)
- [Changing consent version](#changing-consent-version)
- [Consent internal API](#consent-internal-api)
- [Privacy Center](#privacy-center)
  - [Change the default state of the switch button to disabled:](#change-the-default-state-of-the-switch-button-to-disabled)
  - [Deactivate the back button to force the consent:](#deactivate-the-back-button-to-force-the-consent)
  - [Change Activity title:](#change-activity-title)
  - [Force Json update from CDN :](#force-json-update-from-cdn)
  - [Privacy statistics](#privacy-statistics)

Introduction
===============

The Consent module can be used in a lot of different ways, after this short introduction, you will find links to each of the different ways and their specific documentations.

Having the user consent is essential to send sensible information like the IDFA/AAID or using any personal information to serve advertising.

We created this module to simplify the management of your user's privacy and the way to use it.


This module can:

    - Display a consent page (if needed)
    - Save consent inside the phone and reload it every time the application is launched.
    - Check the validity of the consent. The validity duration is set to 6 months by default.
    - Send a hit to our servers to record the consent. For statisical purposes.
    - Save the consent String (if used alongside IAB)
    - Enable or disable the ServerSide. (if used alongside the ServerSide module)
    - Add the categories automatically to the hits the ServerSide sends. (if used alongside the ServerSide module)
    - Forward the consent to the developpers if they need it outside of the module.


Choose your privacy
=======================

Consent comes with two major flavors:

    - With Tag Management (With ServerSide)
    - Standalone

And 3 different ways to display it:

    - Manually and then forwarding us the information
    - Using our Privacy Center for IAB version 2
    - Using our Privacy Center for simple Consent solution

If you're unsure of which one you should use, please contact the person in charge of your account.

[To use IAB V2 please see here](./TCIAB/README.md)


Setup
========

/!\ If you are using our interface, you need to have a version of privacy.json inside your project. This will prevent any issues with users with bad or no internet at all. If you are using IAB please also take vendor-list.json and the translation file purposes-fr.json.
If you are not using our interface, you can't use our privacy.json, if you want a way to use a configuration file, please ask your dev team to manage this file.

After initialisation the Consent module will check the consent validity. If the consent is too old a callback will be called. Please check the Callback part.
The default value is 6 months.

If you're using our interface, and thus our privacy.json, you can change the duration on this validity.
To do this, add "consentDurationInMonths": "13" inside the "information" bloc.

If you're not using our interface, you'll have to manually change it in the code.
We express this duration in months. The duration of a month is calculated by 365/12 days.
Please first call the following method before initializing the Consent module else:

```java
    TCConsent.getInstance().setConsentDuration(13);
```

Minimum Requirements
--------------------

Consent module requires a minimum SDK version of 17.

With the ServerSide
-----------------------

Modules: Core, Consent, ServerSide

This module can use the same model you are using on the web, if you do so, please start by getting the IDs of the categories you are going to use.
Join those IDs with a "consent version". Default is 1, but if you change the implementation, increment this version.

The setup is really simple, pass to the TCConsent object your site ID, privacy ID and application context.

```java
    TCConsent.getInstance().setSiteIDPrivacyIDAppContext(site_id, privacy_id, context);
```

If you're using your own Privacy Center, use the following function instead:

```java
    TCConsent.getInstance().initWithCustomPCM(site_id, privacy_id, context);
```

This call will check the saved consent, putting the ServerSide module on hold if nothing is found, and start/stop the ServerSide if something is saved.

Standalone
----------

Modules: Core, Consent

You won't need the ServerSide module, and will need to implement a callback to manage your solutions when consent is given or re-loaded.

The setup is really simple, pass to the TCConsent object your site ID, privacy ID and application context.

```java
    TCConsent.getInstance().setSiteIDPrivacyIDAppContext(site_id, privacy_id, appContext);
```

If you're using your own Privacy Center, use the following function instead:

```java
    TCConsent.getInstance().initWithCustomPCM(site_id, privacy_id, context);
```

Saving consent
==============

Here is where the IDs of the categories matters.

With the Privacy Center
---------------------------

If you're using the Privacy Center, nothing has to be done here, it will automatically propagate the consent to all other systems. And the ID will be the one used in the configuration file. Please check the Privacy Center part for more information.

Please keep your category IDs between 1 and 999.

Manually displayed consent
------------------------------

Once the user validated his consent, you can then send the information to the Consent module as follows:

in java :

```java
	Map<String, String> consent = new HashMap<>();
	consent.put("PRIVACY_CAT_1", "1");
	consent.put("PRIVACY_CAT_2", "0");
	consent.put("PRIVACY_CAT_3", "1");
	consent.put("PRIVACY_VEN_61", "1");
	TCConsent.getInstance().saveConsentFromConsentSourceWithPrivacyAction(consent, PrivacyCenter, Save);
```

in kotlin :

```kotlin
	val consent: MutableMap<String, String> = HashMap()
	consent["PRIVACY_CAT_1"] = "1"
	consent["PRIVACY_CAT_2"] = "0"
	consent["PRIVACY_CAT_3"] = "1"
	consent["PRIVACY_VEN_61"] = "1"
	TCConsent.getInstance().saveConsentFromConsentSourceWithPrivacyAction(consent, ETCConsentSource.PrivacyCenter, ETCConsentAction.Save);
```

Stop privacy stats tracking
===========================

You can set your `do_not_track` property on your privacy stats payload : 

```java
        TCConsent.getInstance().do_not_track = value;
```

Please prefix your category IDs with "PRIVACY_CAT_" and your vendor IDs with "PRIVACY_VEN_".

The value expected are:

    * 1 means accepting this category or vendor.
    * 2 is for mandatory vendors or categories.
    * 0 is refusing.

Source can be either:

    * Popup
    * PrivacyCenter

And consent action:

    * AcceptAll
    * RefuseAll
    * Save

If you're using the ServerSide, this will propagate the information to the TCServerSide and the TCUser and manage its state.

If you are using mandatory categories (categories that can't be opted out), you can use refuse all and pass those categories with "2".

Forwarding to Server-Side
-------------------------

Only if you use Server-Side and a consent manually displayed and consent external to our platform. 
Otherwise, everything is done automatically, so nothing to do here.

You will need to add consent to the TCUser object to forward it to our server-side.

in java :

```java
        HashMap<String, String> ext = new HashMap<>();
        ext.put("key01", "true");
        ext.put("key02", "1");
        ext.put("312", "0");
        TCUser.getInstance().setExternalConsent(ext);
```

in kotlin :

```kotlin
        val ext: HashMap<String, String> = HashMap()
        ext["key01"] = "true"
        ext["key02"] = "1"
        ext["312"] = "0"
        TCUser.getInstance().externalConsent = ext
```

Since it's external, and we don't really know how it's working, you can pass any string/string and we'll forward it as is.

AcceptAll / RefuseAll
-------------------------

/!\ Those methods only work if you are using our interface and thus have a privacy.json in your project (and maybe IAB's JSON as well).

Those are intended for clients that are displaying a first "popup" screen before our interfaces and that have a way to either open the privacy center of accept/refuse the consent.

We created functions to call if you want to create a simple way to accept or refuse all consent from outside our user interface.

```java
	TCConsent.getInstance().acceptAllConsent();
	TCConsent.getInstance().refuseAllConsent();
```

Retaining consent
====================

The saving of the consent on our servers is done automatically.

But since we are saving the consent in our servers, we need to identify the user one way or another. By default, the variable used to identify the user consenting is #TC_SDK_ID#, but you can change it to anything you'd like.

If you're looking for a way to prove consent or reset saved information, you'll need to create a specific screen in app for this.

This can be used to save the display of the consent, and giving the consent.

This ID is very important because it will be the basic information used to get back the consent when you need a proof.

Using your own user ID
----------------------

You will be able to get the information more easily since this is an ID available by several means for you.

To modify the ID used for saving the consent, you can change the information inside the TCUSer.

```java
	TCUser.getInstance().consentID = "myConsentID";
```

Displaying chosen ID
--------------------

You might want to be able to display to your end user the ID used to save the consent. You can simply get it like this:

```java
	TCUser.getInstance().consentID
```

Displaying consent
==================

If you are familiar with Commanders Act Consent for web, you know that we actually record two things. The first thing is "displaying the consent form".
This allows you to prove that a user has indeed been shown the consent screen even if he somehow left without accepting/refusing to give his consent.

In some cases, client also use this to infer user consent since he continued using the application after he was shown the consent screen.
We don't recommend this behaviour, please discuss it with your setup team first.


Reacting to consent
===================

If you need to react to the user giving consent, or the loading of the consent at the start of the Consent module we created several callbacks to help.

Currently, we have a callback function that lets you get back the categories and set up your other partners accordingly.
This is the function where you would tell your ad partner "the user don't want to receive personalized ads" for example.

/!\ Don't forget to register to the callbacks *before* the initialisation of the Consent Module since the module will check consent at init and use the callback at this step.


Implement TCPrivacyCallbacks to get access to those callbacks:

```java
	void consentUpdated(Map<String, String> categories);
```

Called when either:

* We load the saved consent
* consent is given inside the privacy center
* you manually give us the user selected consents

We have a Map which is the same as the one given to our SDK with keys PRIVACY_CAT_n and PRIVACY_VEN_n and value "0" or "1".
In the case nothing was consented to, you might also have an empty map (but not null).

```java
	void consentOutdated();
```

This is called after 6 months without change in the user consent. This can allow you to force displaying the consent the same way you would on first launch.

```java
	void consentCategoryChanged();
```

When you make a change in the JSON, there is nothing special to do.
But when this change is adding or removing a category, or changing an ID, we should re-display the Privacy Center.

```java
	void significantChangesInPrivacy();
```

This one is slightly different from the last one, it was created for IAB and will not be sent automatically. It is conditioned by the field "significantChanges" in the privacy.json so that it will only launch when you need it to.

Please also note that you can listen to starting and stopping the SDK events, you'll need to register your Observer that implements 'TCEventManager.TCLifecycleListener' interface via 'TCEventManager.registerLifecycleListener()' method.

Resetting consent : 
-------------------

To reset user consent on devices, you can use the following method:

```java
    TCConsent.getInstance().resetSavedConsent()
```

Please note that this method resets the consent on the device each time it is called. If you need to handle resets for specific app versions, you will have to manage that manually.

Alternatively, if you are using our PrivacyCenter, you can use the resetSave field in your privacy.json. For implementation details, please contact your consultant.

Forwarding consent to webViews
==============================

Some clients need to have the consent forwarded in their webViews to manage a web container inside it.
We created a function to get the privacy as a JSON string so you can save it inside the webView''s local storage.

> [!WARNING]
> This function only help to save it to the local storage by giving the required format, you will still need to have JS code in the web container to use it. Please ask your consultant for this part.

```java
    String JSON = TCConsent.getInstance().getConsentAsJson();
````

## Forwarding consent to FirebaseAnalytics :

Now thanks to Google Consent Mode, if you are using our TCFirebaseDestination, you can configure the TCConsent module to forward and set the consent to FirebaseAnalytics once the user has opted-in/out for your mapped categories.

To do this, please add the following section to the root of your privacy.json : 

```json
    "google_consent_mode": {
        "use_consent_mode": true, // boolean value to activate the mapping
        "infer_ad_from_tcf": false, // boolean value for default IAB mapping
        "category_mapping": { // Custom categories ID mapping only
            "ad_storage": 1, 
            "ad_user_data": 2,
            "ad_personalization": 3,
            "analytics_storage": 4
        }
    }
```

If you are using IAB and infer_ad_from_tcf the 3 ad_somthing categories are automatically mapped and thus the categories here are not taken into consideration.

Changing consent version
=========================

If the case you need to manually change the consent version (if you're using your own privacy center for example), you can use the following:


```java
    TCConsent.getInstance().consentVersion = "132";
```

Consent internal API
====================

We created several methods to check given consent. They are simple, but make it easier to work with consent information at any given time.
You'll find those in the class TCConsentAPI:

```java
	/**
	* Checks if we should display privacy center for any reason.
	* @param appContext the application context.
	* @return True or False.
	*/
	public static boolean shouldDisplayPrivacyCenter(Context context)

	/**
	* Checks if consent has already been given by checking if consent information is saved.
	* @param appContext the application context.
	* @return true if the consent was already given, false otherwise.
	*/
	public static boolean isConsentAlreadyGiven(Context appContext);

	/**
	* Return the epochformatted timestamp of the last time the consent was saved.
	* @param appContext the application context.
	* @return epochformatted timestamp or 0.
	*/
	public static Long getLastTimeConsentWasSaved(Context appContext);

	/**
	* Check if a Category has been accepted.
	* @param ID the category ID.
	* @param appContext the application context.
	* @return true or false.
	*/
	public static boolean isCategoryAccepted(int ID, Context appContext);

	/**
	* Check if a vendor has been accepted.
	* @param ID the vendor ID.
	* @param appContext the application context.
	* @return true or false.
	*/
	public static boolean isVendorAccepted(int ID, Context appContext);

	/**
	* Get the list of all accepted categories.
	* @param appContext the application context.
	* @return a List of PRIVACY_CAT_IDs.
	*/
	public static List<String> getAcceptedCategories(Context appContext);

	/**
	* Get the list of all accepted vendors.
	* @param appContext the application context.
	* @return a List of PRIVACY_VEN_IDs.
	*/
	public static List<String> getAcceptedVendors(Context appContext);

	/**
	* Get the list of all google vendors accepted.
	* @param appContext the application context.
	* @return a List of acm_ID.
	*/
	public static List<String> getAcceptedGoogleVendors(Context appContext);

	/**
	* Get the list of everything that was accepted.
	* @param appContext the application context.
	* @return a List of PRIVACY_VEN_IDs and PRIVACY_CAT_IDs.
	*/
	public static List<String> getAllAcceptedConsent(Context appContext);

	/**
	* Check if a purpose has been accepted.
	* @param ID the purpose ID.
	* @param appContext the application context.
	* @return true or false.
	*/
	public static boolean isIABPurposeAccepted(int ID, Context appContext);

	/**
	* Check if a vendor has been accepted.
	* @param ID the vendor ID.
	* @param appContext the application context.
	* @return true or false.
	*/
	public static boolean isIABVendorAccepted(int ID, Context appContext);

	/**
	* Check if a special feature has been accepted.
	* @param ID the vendor ID.
	* @param appContext the application context.
	* @return true or false.
	*/
	public static boolean isIABSpecialFeatureAccepted(int ID, Context appContext);
```

Privacy Center
==============

The Privacy Center is represented by a JSON file that describes the interfaces that will be created by native code inside the application.

For now this JSON has to be created and managed manually. An offline version is mandatory inside the app and if you need to update it remotely you can have another version on our CDNs.
The module will check for updates of the file automatically.

Your account should have a consultant that will provide you the corresponding JSON for your project.


In the Android SDK we create an Activity which is an easy way to display a "page" without have to create a specific fragment space for it.
Offline JSON has to be saved in the src/main/assets folder.

To start the Privacy Center, you have to launch the corresponding activity.

    Intent PCM = new Intent(getContext(), com.tagcommander.lib.consent.TCPrivacyCenter.class);
    startActivity(PCM);

Some part of the Privacy Center can be customised with your code.

Change the default state of the switch button to disabled:
---------------------------------------------------------

```java
	TCConsent.getInstance().switchDefaultState = false;
```

Deactivate the back button to force the consent:
------------------------------------------------

Going back without consenting will result in a user not consenting at all. This means that no privacy will be saved, no tag can be called and no consent-string will be created if you use IAB.

	TCConsent.getInstance().deactivateBackButton = true;

Change Activity title:
----------------------

You can change the default activity title in your the Intent, using the kTCIntentExtraCustomTitle key. 

	Intent PCM = new Intent(getContext(), com.tagcommander.lib.privacy.TCPrivacyCenter.class);
	PCM.putExtra(kTCIntentExtraCustomTitle, "My custom title");


Force Json update from CDN :
----------------------------

Previously we prevented parsing new privacy.json configuration immediatly to avoid re-doing resource-intensive tasks.
We changed this behaviour since we managed to make this parsing more efficient.
But it might happen that your configuration is still too big and it freezes the main thread. If so, you can call this method with false.
This will only save the new privacy.json and only parse it and use it the next time you launch the application.

```java
	TCConsent.getInstance().shouldForceJsonUpdate(false)
```

Privacy statistics
------------------

We have dashboards that allow to have detailed statistics on the choices your users are making.
Depending on your app privacy configuration you might have to call some additional functions.

    - Custom « banner/popup » -> our privacy center
    - Custom « banner/popup » -> Custom privacy center
    - Directly to our privacy center
    - Custom privacy center

Whenever saveConsent* is called you will need to provide the full list of purposes and vendors that have been consented to and refused.

We reworked saveConsent methods to only use one. If you are using the old functions they will still work for now.
Otherwise, please check the above section "Manually displayed consent" for how this method works.


/!\ Also, please note that you will need to call **statViewBanner** when you display your custom banner.

![alt tag](../res/TCPC_customBanner.jpeg)
![alt tag](../res/TCPC_PC.jpeg)
![alt tag](../res/CustomBanner.jpeg)
![alt tag](../res/CustomPC.jpeg)

Copy/paste-able list of functions for our interfaces:

```java
        TCConsent.getInstance().refuseAllConsent();
        TCConsent.getInstance().acceptAllConsent();
        TCConsent.getInstance().statEnterPCToVendorScreen();
        TCConsent.getInstance().statViewPrivacyPoliciesFromBanner();
        TCConsent.getInstance().getNumberOfIABVendors();
```

Copy/paste-able list of functions for custom interfaces:

```java
        TCConsent.getInstance().saveConsentFromConsentSourceWithPrivacyAction(consent, ETCConsentSource.Popup, ETCConsentAction.RefuseAll);
        TCConsent.getInstance().statEnterPCToVendorScreen();
        TCConsent.getInstance().statViewPrivacyPoliciesFromBanner();
        TCConsent.getInstance().statViewPrivacyPoliciesFromPrivacyCenter();
        TCConsent.getInstance().statViewPrivacyCenter();
        TCConsent.getInstance().statShowVendorScreen();
```

## Stop privacy stats tracking

You can set your `do_not_track` property on your privacy stats payload : 

        TCConsent.getInstance().do_not_track = value;

## TCDemo

You can, of course, check our demo project for a simple implementation example.

[TCDemo_ServerSide_And_Consent](https://github.com/CommandersAct/TCMobileDemo-V5/tree/master/Android/)

# Support and contacts

![alt tag](../res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com

Commanders Act | 3/5 rue Saint Georges - 75009 PARIS - France
***

This documentation was generated on 05/02/2026 14:40:02
