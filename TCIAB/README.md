![alt tag](../res/ca_logo.png)


TCIAB's Implementation Guide
==============================

**Android**

Last update : *18/06/2026*<br />
Release version : *5.1.1*

## Table of Contents

- [TCIAB's Implementation Guide](#tciabs-implementation-guide)
- [Introduction](#introduction)
- [Setup](#setup)
- [JSON Configurations](#json-configurations)
  - [vendor-list.json](#vendor-listjson)
  - [purposes-xx.json](#purposes-xxjson)
  - [privacy.json](#privacyjson)
  - [TCIABPublisherRestrictions.json](#tciabpublisherrestrictionsjson)
  - [google-atp-list.json](#google-atp-listjson)
- [IAB 2.1](#iab-21)
- [IAB 2.2](#iab-22)
- [Filtering vendors](#filtering-vendors)
- [Selecting buttons](#selecting-buttons)
- [Initialisation](#initialisation)
- [With the ServerSide](#with-the-serverside)
- [Retaining consent](#retaining-consent)
- [Reacting to consent](#reacting-to-consent)
- [Generating publisher TC in consent string](#generating-publisher-tc-in-consent-string)
- [Loading a specific screen directly](#loading-a-specific-screen-directly)
- [Troubleshooting](#troubleshooting)
- [Support and contacts](#support-and-contacts)

Introduction
============

This module adds IAB TCF v2 consent string support to the Consent module.

> [!INFO]
> This module only supports TCF v2. TCF v1 is not supported.

Setup
=====

The IAB module must be used alongside the Consent module.

You will need several JSON configuration files. The module updates them automatically, but having offline copies prevents issues on poor or no internet connections.

JSON Configurations
===================

vendor-list.json
----------------

Contains all IAB-registered vendors and the definitions for all purposes, special purposes, features and special features (English only). Created and maintained by IAB.

Download an offline copy from:
https://vendorlist.consensu.org/v2/vendor-list.json

Keep the same filename.

purposes-xx.json
----------------

Translation files for purposes. Required if your app uses more than one language.

Download from https://register.consensu.org/translation under "List of translations for purpose descriptions v2.0". Keep the same filenames.

After initialising the Consent module, set the language:

```java
    TCConsent.getInstance().setLanguage("fr");
    // Use ISO 639-1 language codes
```

privacy.json
------------

Declares information used to save consent in our dashboards, plus texts for elements not officially defined by IAB.

> [!INFO]
> This file must be provided by a Commanders Act consultant.

If you use multiple languages, you will find a `"texts"` block for defaults and `"texts_xx"` blocks for each language.

TCIABPublisherRestrictions.json
-------------------------------

Optional. Represents the restrictions your company applies to its partners.

If you use this file, place it alongside the other JSON configurations and call:

```java
    TCConsent.getInstance().useCustomPublisherRestrictions();
```

> [!IMPORTANT]
> This file should be created by your Commanders Act contact and decided by your project manager.

google-atp-list.json
--------------------

Optional. Only required if you use Google AC-String.

Place the file alongside the other JSON configurations and call the following **before** initialising the Consent module:

```java
    TCConsent.getInstance().useAcString(true);
```

If using AC-String, also verify that you have a list of Google vendors inside your `privacy.json`.

This file can only be provided by your consultant. It will be updated automatically by the library.

IAB 2.1
=======

Add the following to the `"texts"` section of your `privacy.json` to display new information correctly:

```javascript
    "texts": {
      "generic": {
            "month": "months",
            "day": "days",
            "seconds": "seconds",
            "hours": "hours"
      },
      "vendors": {
            "deviceStorageTitle": "Storage Type:",
            "deviceStorageCookieLifetime": "Cookie lifetime: ",
            "deviceStorageOther": "Others",
            "deviceStorageCookies": "Cookies"
      }
    }
```

IAB 2.2
=======

Required steps when upgrading to an IAB v2.2-compatible version of TCConsent (5.4.0 or higher):

1. Update all offline in-app JSONs to v2.2-compatible versions: `vendor-list.json` and all `purposes-xx.json` translation files.
2. Update your `privacy.json` (both offline and CDN versions) to a v2.2-compatible version and recheck your IAB vendor filter (`"vendors"` key at root).
3. Make sure you have a `{total_number}` placeholder inside `"texts" -> "popup" -> "purposeTitle"`.

Add the following to your `privacy.json`:

```
  "texts_xx": {
    "generic": {
        "illustationsButton": "illustrations:",
        "dataCategoriesDef": "Data Categories:"
    },
    "vendors": {
        "legIntClaimTitle": "Politique de legitimate"
    },
    "popup": {
        "purposeTitle": "We and our {total_number} partners"
    }
  }
```

Filtering vendors
=================

To show only the vendors your company uses (instead of the full IAB list), add a filter inside `privacy.json` under `"information"`:

```json
    "vendors": "8,18,467,310"
```

This also filters which purposes and special features are shown to the user.

> [!IMPORTANT]
> This should be decided by your project manager and added to the JSON by your Commanders Act contact.

Selecting buttons
=================

The IAB interface has two layers:

- **First layer** — the initial screen. Default buttons: `Detail`, `AcceptAll`, `RefuseAll`
- **Second layer** — purpose and vendor screens. Default buttons: `Save`, `AcceptAll`, `RefuseAll`

IAB requires at least a `Detail` button on the first layer and a `Save` button on the second. Since September 2020, CNIL requires that if you have an "Accept All" button, a visually identical "Refuse All" button must also be present.

To customise which buttons appear and in what order, add to `privacy.json`:

```json
    "components": {
        "firstLayerButton": ["Detail", "AcceptAll", "RefuseAll"],
        "secondLayerButton": ["Save", "AcceptAll", "RefuseAll"]
    }
```

Initialisation
==============

No additional initialisation is required. Adding this module as a dependency alongside TCConsent is sufficient — the IAB consent string will be generated and managed automatically as part of the normal TCConsent init.

See the [TCConsent initialisation](../TCConsent/README.md#initialisation) for the standard setup.

With the ServerSide
===================

All IAB consent information saved by this module is automatically forwarded to every ServerSide hit. You can use any IAB purpose as a category and build rules in your container accordingly.

Retaining consent
=================

[Please see the specific documentation here](../TCConsent#retaining-consent)

Reacting to consent
===================

[Please see the specific documentation here](../TCConsent#reacting-to-consent)

Generating publisher TC in consent string
=========================================

The publisher TC part of the consent string is not generated by default. To enable it, set the following boolean in `TCConsent` / `TCMobilePrivacy`:

```java
    TCConsent.getInstance().generatePublisherTC = true;
```

Loading a specific screen directly
===================================

The Privacy Center has two layers. The first layer is the initial popup screen. From there, the user can navigate to the purpose screen or the vendor screen — both of which are considered the second layer.

If you want to build your own first layer (aka Banner in its IAB format), you can open either second layer screen directly:

in java :

```java
    Intent PCM = new Intent(getContext(), com.tagcommander.lib.consent.TCPrivacyCenter.class);
    PCM.putExtra(com.tagcommander.lib.consent.TCConsentConstants.kTCPC_START_SCREEN,
                 com.tagcommander.lib.consent.TCConsentConstants.kTCStartWithPurposeScreen);
    startActivity(PCM);
```

in kotlin :

```kotlin
    val PCM = Intent(getContext(), TCPrivacyCenter::class.java)
    PCM.putExtra(TCConsentConstants.kTCPC_START_SCREEN, TCConsentConstants.kTCStartWithPurposeScreen)
    startActivity(PCM)
```

For the vendor screen, replace `kTCStartWithPurposeScreen` with `kTCStartWithVendorScreen`:

in java :

```java
    PCM.putExtra(com.tagcommander.lib.consent.TCConsentConstants.kTCPC_START_SCREEN,
                 com.tagcommander.lib.consent.TCConsentConstants.kTCStartWithVendorScreen);
```

in kotlin :

```kotlin
    PCM.putExtra(TCConsentConstants.kTCPC_START_SCREEN, TCConsentConstants.kTCStartWithVendorScreen)
```

Troubleshooting
===============

Make sure logging is set to `Log.VERBOSE` (`TCDebug.setDebugLevel(Log.VERBOSE)`). After consent is saved via any method (`acceptAllConsent()`, `refuseAllConsent()`, `saveConsentFromConsentSourceWithPrivacyAction()`, or a button press in the Privacy Center), you should see the generated IAB consent string in your Logcat:

```
I  CommandersAct: saved Consent String: CQkPrrBQkPrrBBaE2BENCe..........
```

If you see this, the IAB consent string was correctly generated and saved. If not, check that logging is enabled and that one of the save methods above was actually called.

Support and contacts
====================

![alt tag](../res/ca_logo.png)

***
**Support**
*support@commandersact.com*

http://www.commandersact.com

Commanders Act | 25 rue de Tolbiac, 75013 Paris - France
***

This documentation was generated on 18/06/2026 09:00:32