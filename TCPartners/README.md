![alt tag](../res/ca_logo.png)


Partners' Implementation Guide
==============================

Last update : *05/02/2026*

Release version : *5.0.1*

## Table of Contents

- [Partners' Implementation Guide](#partners-implementation-guide)
- [Introduction](#introduction)
- [Partners](#partners)
- [Adobe Audience Manager (AAM)](#adobe-audience-manager-aam)
  - [Hit](#hit)
- [Freewheel](#freewheel)
- [Support and contacts](#support-and-contacts)

Introduction
============

In some specific cases we need to have a direct connection from the phone to a vendor.

In this case hits need to be sent from the phone and we need to treat the response from the server inside the app.


Partners
========

TCPartners or TCMobilePartners is the class used as the super-type of all partners.

A TCPartner is by default a partner that will listen to all hits you're passing to the SDK so he can work on them.
You can change this activation by using on of the 3 following functions:

```java
	/**
	 * This function tells the partner to activate on all hits.
	 */
	public void activateOnAllHits();

	/**
	 * This function tells the partner to only treat hit when the specified key is in the datalayer.
	 * @param key the key to activate the treatment.
	 */
	public void activateOnKey(String key);

	/**
	 * This function tells the partner to only treat hit when the specified key/value pair is in the datalayer.
	 * @param key the specific key.
	 * @param value the specific value.
	 */
	public void activateOnKeyValue(String key, String value);

	/**
	 * This function tells the partner to only treat hit when the specified key is NOT in the datalayer.
	 * * @param key the key to prevent the activation.
	 */
	public void activateOnAllHitsButKey(String key)

	/**
	 * This function tells the partner to only treat hit when the specified key and value are NOT in the datalayer.
	 * @param key the specific key.
	 * @param value the specific value.
	 */
	public void activateOnAllHitsButKeyValue(String key, String value)
```

So think carefully about which activation method you want for your partners.


Adobe Audience Manager (AAM)
============================

The point of this connector is the send information to Adobe Audience Manager and get back the segments corresponding to the app user.

```java
	TCPartners_AdobeAudienceManager.getInstance().setContext(context);
	TCPartners_AdobeAudienceManager.getInstance().initWith(81811, 20201);
	TCPartners_AdobeAudienceManager.getInstance().setUniqueIdentifier("3385ACC1-D465-2D13-A4E3-9A5A865A232C")


If you want to use your custom configuration to use offline segments ID, please also add this line.

    TCPartners_AdobeAudienceManager.getInstance().addOfflineConfiguration(3311, 1);
```

This connector only works if we have and IDFA or AAID.

Hit
---

Since we're potentially sending information to several partners we need to differentiate the data for AAM.
We're basing ourselves on the datalayer and are taking all the keys prefixed "c_" as keys to add to the hits sent to AAM.

If among the data layer, the connector finds the key #USER_ID#, we will send an "identified" hit. Which simply behave slightly differently, but has the same use.


Freewheel
=========

Our Freewheel implementation is only made to forward the segments computed in Adobe for them.

This means we only need 2 things to make it work.

The first one is the callback function that should be called when we parsed the segment information.

The second is the domain which correspond to the application. This is needed because AAM can send information from several different app domains when you have several configured.

You will have to register to a callback to receive the content of the segments.

And will receive a response of the format:

	{
		aam_fr=sid=81025,
		aam_oas=PYT_63359=Y,
		aam_fw=PYT_63359=Y&PYT_619=Y&PYT_7398=Y&PYT_94221
	}

To initialize Freewheel:

```java
	TCPartners_Freewheel.getInstance().setSegmentDomain(".tf1.fr");
	TCPartners_Freewheel.getInstance().callback = this;

And to recover the segments:

	public void onSegmentReceived(Map<String, String> segments)
	{
		TCLogger.getInstance().logMessage("Segments:" + segments, Log.ERROR);
	}
```

Support and contacts
====================

![alt tag](../res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com

Commanders Act | 3/5 rue Saint Georges - 75009 PARIS - France
***

This documentation was generated on 05/02/2026 14:40:02
