![alt tag](../res/ca_logo.png)

Consent's Implementation Guide
==============================

Last update : *18/06/2026*

Release version : *5.4.0*

## Table of Contents

- [Consent's Implementation Guide](#consents-implementation-guide)
- [Introduction](#introduction)
- [Configure TCConsent](#configure-tcconsent)
  - [Choose your Modules Configuration](#choose-your-modules-configuration)
  - [Choose Consent flavour](#choose-consent-flavour)
  - [Choose UI Components Configuration](#choose-ui-components-configuration)
- [Technical Setup](#technical-setup)
  - [Minimum Requirements](#minimum-requirements)
  - [Initialisation](#initialisation)
- [Saving consent](#saving-consent)
  - [With our Privacy Center](#with-our-privacy-center)
  - [Manually displayed consent](#manually-displayed-consent)
  - [AcceptAll / RefuseAll](#acceptall-refuseall)
  - [Forwarding consent to Server-Side (external consent only)](#forwarding-consent-to-server-side-external-consent-only)
  - [Testing your integration](#testing-your-integration)
- [Consent Banner Setup](#consent-banner-setup)
  - [Configuration](#configuration)
  - [Usage](#usage)
  - [Options](#options)
  - [Design and colours](#design-and-colours)
  - [Button actions](#button-actions)
- [Privacy Center Setup](#privacy-center-setup)
  - [Launching the Privacy Center](#launching-the-privacy-center)
  - [Customisation](#customisation)
  - [Force JSON update from CDN](#force-json-update-from-cdn)
  - [Loading a specific screen directly](#loading-a-specific-screen-directly)
- [Reacting to consent](#reacting-to-consent)
- [Retaining consent](#retaining-consent)
  - [Using your own consent ID](#using-your-own-consent-id)
  - [Displaying the consent ID to the user](#displaying-the-consent-id-to-the-user)
  - [Consent validity duration](#consent-validity-duration)
  - [Resetting consent](#resetting-consent)
  - [Consent version](#consent-version)
  - [Displaying consent](#displaying-consent)
- [Privacy statistics](#privacy-statistics)
  - [Stop privacy statistics tracking](#stop-privacy-statistics-tracking)
- [Google Consent Mode (Firebase)](#google-consent-mode-firebase)
  - [Debugging Google Consent Mode](#debugging-google-consent-mode)
- [Forwarding consent to webViews](#forwarding-consent-to-webviews)
- [Consent internal API](#consent-internal-api)
- [TCDemo](#tcdemo)
- [Support and contacts](#support-and-contacts)

Introduction
============

The Consent module manages your users' consent: displaying a consent UI, saving consent on the device, checking its validity, and forwarding it to the ServerSide module.

This module can:

- Display a consent banner or Privacy Center.
- Save consent on the device and reload it on every launch.
- Check consent validity (default: 6 months).
- Send a hit to our servers to record the consent.
- Send hits to our servers for statistical purposes.
- Save the IAB consent string (when used with the IAB module).
- Forward consent to developers via callbacks if they need it outside of the module.
- If used alongside the ServerSide module:
    - Enable or disable the ServerSide module based on consent.
    - Automatically add consent categories to ServerSide hits.

Configure TCConsent
===================

Choose your Modules Configuration
---------------------------------

- **With ServerSide** — modules: Core, Consent, ServerSide

    The module will automatically start or stop the ServerSide based on the saved or
    incoming consent. You don't need to manage this manually — just initialise the module
    and it handles the rest.

- **Standalone** — modules: Core, Consent (you manage your own solutions via callbacks)

    Without the ServerSide module, consent is still saved and callbacks still fire — but
    you are responsible for enabling or disabling your own third-party solutions based on
    what comes back in `consentUpdated`.

Choose Consent flavour
-----------------------

- **Non-IAB — modules: Core, Consent**

    Consent is collected and saved against your own custom categories and vendors as defined in privacy.json, or passed to saveConsent: function.

- **IAB — modules: Core, Consent, TCIAB**

    [IAB (Interactive Advertising Bureau)](https://iabeurope.eu/) defines the Transparency and Consent Framework (TCF), the industry standard for collecting and communicating user consent for digital advertising. 
    When the TCIAB module is linked, consent is collected and saved as a TCF-compliant consent string in addition to the standard format. 
    The Privacy Center gains an IAB-compliant first layer, and the category and vendor screen includes all the IAB categories and defined Vendors — alongside your own custom categories and vendors, which are still displayed. See the [TCIAB documentation for details](../TCIAB/README.md).

- **IAB + AC String — modules: Core, Consent, TCIAB (AC String enabled in code)**

    [Google AC String](https://support.google.com/admanager/answer/9681920) is a complementary consent signal used by Google's ad technology providers, on top of the IAB TCF string. It is enabled in code by calling useAcString(true) before initialisation and requires a google-atp-list.json file and a list of Google vendors in privacy.json. See the [TCIAB documentation for details](../TCIAB/README.md).

Choose UI Components Configuration
----------------------------------

Before choosing your components, it helps to understand the layers of a consent flow, and our UI Components:

#### Consent layers

- **First layer** — The first screen a user sees. Typically a banner or modal where they can accept consent in one tap. The Consent Banner is a non-IAB first layer option; the Privacy Center can also act as a first layer when using IAB.

- **Second layer** — The detailed screen reached via a "Details" or "Manage" action, where users make granular choices per category and vendor. The Privacy Center always provides this layer.

> [!WARNING] IAB compliance:
> When using the IAB module, both the first layer and second layer must meet IAB specifications and be validated by IAB.

#### UI Components

#### Privacy Center

`TCPrivacyCenterViewController` is the full consent management UI where users toggle individual categories and vendors. Its behaviour adapts automatically based on whether the TCIAB module is linked — no code change needed.

| | With TCIAB | Without TCIAB |
|---|---|---|
| Opens to | IAB-compliant first layer, then category/vendor screen on Detail | Category/vendor screen directly |
| Vendors shown | Custom + IAB vendors | Custom vendors only |

> Linking or removing TCIAB is sufficient to switch modes.

For setup and launch options, see [Privacy Center Setup](#privacy-center-setup) below.

#### Consent Banner *(non-IAB only)*

`showBanner()` displays a lightweight first-layer banner. From its Details button, you can open the Privacy Center or any custom screen of your own. Not suitable for IAB setups at this time.

> [!NOTE]
> If you are using the TCIAB module, the Privacy Center already handles the first layer automatically — you do not need to call `showBanner()`.

For setup and display options, see [Consent Banner Setup](#consent-banner-setup) below.

> [!IMPORTANT]
> If you're unsure which setup to use, contact your account manager.

###  UI Components Configuration Options

- **With TCIAB (IAB)** - Both layers must meet IAB requirements. Options:

    - Use the Privacy Center for both layers (recommended)
    - Build your own first layer → bind to `TCConsent` → Privacy Center handles the second layer. See [Loading a specific screen directly](#loading-a-specific-screen-directly).

- **Without TCIAB (non-IAB)**  - no IAB constraints. Any combination works:

    - Our Consent Banner or your own banner as a first layer
    - Our Privacy Center or your own custom screen as a second layer

  *Privacy Center first layer (IAB):*

  ![privacy_center_first_layer](../res/privacy_center_first_layer.jpg)

  *Privacy Center second layer (IAB & non-IAB):* with vendor screen on the left, categories on the right
  
  ![privacy_center_second_layer](../res/privacy_center_second_layer.png)

  *Consent Banner (non-IAB first layer):*
  
  ![banner_screenshot](../res/banner_screenshot.jpg)

Technical Setup
===============

> [!IMPORTANT]
> If you are using our UI (Banner and/or Privacy Center), you must include an offline copy of `privacy.json` in your project. This prevents issues for users with no or poor internet. If you are also using IAB, include `vendor-list.json` and the relevant `purposes-xx.json` translation file.

Documentation for `privacy.json` is available here: [Privacy JSON Documentation](../res/Privacy_JSON_Documentation.md)

If you are using your own Privacy Center, please check the following documentation for functions to call from your UI:

> [User built Privacy Center guide](../res/user_privacy_center.md)

Minimum Requirements
--------------------

Minimum Android SDK version: 21

Initialisation
--------------

**With our UI components (Privacy Center and/or Banner, `privacy.json` required):**

```java
TCConsent.getInstance().setSiteIDPrivacyIDAppContext(site_id, privacy_id, context);
```

**Without our UI components (custom privacy center):**

```java
TCConsent.getInstance().initWithCustomPCM(site_id, privacy_id, context);
```

At init, the module checks saved consent and puts the ServerSide on hold if nothing is found. Once consent is given or loaded, the ServerSide is started or stopped accordingly.

> [!IMPORTANT]
> Register your callbacks **before** calling init. The module checks consent at startup and fires callbacks immediately.

Saving consent
==============

With our Privacy Center
-----------------------

Nothing to do — the Privacy Center propagates consent to all systems automatically.

Keep your custom category IDs between 1 and 999 in your `privacy.json`.

Manually displayed consent
--------------------------

> [!INFO]
> The `saveConsent*` methods cannot be used when using IAB. IAB compliance requires that consent be collected through a validated UI — the SDK cannot generate a TCF-compliant consent string from an interface it does not control. In IAB mode, use `acceptAllConsent()` / `refuseAllConsent()` for all-or-nothing consent, or our UI components for per-category and per-vendor granularity.
>
> If you need to collect consent manually through your own UI, the `saveConsent*` methods remain available in the **nonIAB configuration**.

If you build your own consent UI, once the user validates consent, pass it to the module:

in java:

```java
Map<String, String> consent = new HashMap<>();
consent.put("PRIVACY_CAT_1", "1");
consent.put("PRIVACY_CAT_2", "0");
consent.put("PRIVACY_CAT_3", "1");
consent.put("PRIVACY_VEN_61", "1");
TCConsent.getInstance().saveConsentFromConsentSourceWithPrivacyAction(consent, ETCConsentSource.PrivacyCenter, ETCConsentAction.Save);
```

in kotlin:

```kotlin
val consent: MutableMap<String, String> = HashMap()
consent["PRIVACY_CAT_1"] = "1"
consent["PRIVACY_CAT_2"] = "0"
consent["PRIVACY_CAT_3"] = "1"
consent["PRIVACY_VEN_61"] = "1"
TCConsent.getInstance().saveConsentFromConsentSourceWithPrivacyAction(consent, ETCConsentSource.PrivacyCenter, ETCConsentAction.Save)
```

Prefix category IDs with `PRIVACY_CAT_` and vendor IDs with `PRIVACY_VEN_`. Values:

- `1` — accepted
- `2` — mandatory (cannot be refused)
- `0` — refused

Sources: `Popup` or `PrivacyCenter`

Actions: `AcceptAll`, `RefuseAll`, `Save`

AcceptAll / RefuseAll
---------------------

> [!WARNING]
> These methods only work if you are using our UI and have `privacy.json` in your project.

> [!NOTE]
> In the IAB variant, `acceptAllConsent()` and `refuseAllConsent()` are the only supported programmatic consent methods. For manual per-category or per-vendor consent in IAB mode, you must use our Privacy Center.

For clients displaying a custom first screen before our interface, with a way to accept or refuse all consent directly:

```java
TCConsent.getInstance().acceptAllConsent();
TCConsent.getInstance().refuseAllConsent();
```

Forwarding consent to Server-Side (external consent only)
---------------------------------------------------------

Only needed if you use ServerSide with your own consent implementation external to our platform. Otherwise everything is handled automatically.

in java:

```java
HashMap<String, String> ext = new HashMap<>();
ext.put("key01", "true");
ext.put("key02", "1");
ext.put("312", "0");
TCUser.getInstance().setExternalConsent(ext);
```

in kotlin:

```kotlin
val ext: HashMap<String, String> = HashMap()
ext["key01"] = "true"
ext["key02"] = "1"
ext["312"] = "0"
TCUser.getInstance().externalConsent = ext
```

Since it's external, and we don't really know how it's working, you can pass any string/string and we'll forward it as is.

Testing your integration
------------------------

Make sure logging is set to `Log.VERBOSE` (`TCDebug.setDebugLevel(Log.VERBOSE)`).

After consent is accepted or refused — whether via `acceptAllConsent()`, `refuseAllConsent()`, `saveConsentFromConsentSourceWithPrivacyAction()`, or a button press in the Privacy Center or Banner — you should see the following in your Logcat:

```
D  CommandersAct: sending: https://privacy.trustcommander.net/privacy-consent/?tc_firsttime=1
D  CommandersAct: with POST data:
D  CommandersAct: {"privacyBeacon":{"id_privacy":3,"site":XXXX,"version":"XX","do_not_track":false,"privacy_action":"1","optin_categories":"1,2,3,...
```

If you see this, consent was saved successfully on the device and forwarded to our servers. You can also check the saved values inside `MySharedPreferencesTC.xml` inside your app shared preferences.

If not, check that logging is enabled and that one of the save methods above was actually called.

Consent Banner Setup
====================

> [!NOTE]
> The Consent Banner (`showBanner()`) is non-IAB. If you are using the TCIAB module, the Privacy Center already handles the first layer automatically — you do not need to call `showBanner()`.

The Consent Banner is a lightweight UI component to quickly collect consent before optionally opening the full Privacy Center.

Configuration
-------------

The banner can be displayed in two modes:

- **Bottom sheet** (`TCBannerType.BOTTOM_SHEET`) — slides up from the bottom of the screen as an overlay
- **Full screen** (`TCBannerType.FULL_SCREEN`) — displayed as a centered modal card

Both support a compact layout mode (`compactLayout = true` in `TCBannerOptions`) which reduces the visual weight of the banner. Only enable it when you are confident it meets your local regulations — buttons retain the same size and font, but the reduced visual prominence of the refuse option may not be sufficient in all jurisdictions.

Textual and visual elements are defined in `privacy.json` under `"texts" -> "banner"` and are mandatory. Refer to the [privacy.json documentation](../res/Privacy_JSON_Documentation.md) for full details.

![Privacy Banner Screenshot](../res/banner_screenshot.jpg)
*Left: full screen — Centre: bottom sheet — Right: compact layout*

Usage
-----

```kotlin
TCConsent.getInstance().showBanner(
    activity = activity,
    type = TCBannerType.BOTTOM_SHEET,
    options = TCBannerOptions(),
    onDetailsButtonClick = {
        // Open the Privacy Center or your own screen here
    }
)
```

Parameters:

- `type` — banner display mode: `.BOTTOM_SHEET` for a bottom sheet or `.FULL_SCREEN` for a modal card
- `options` — layout and behaviour options (see [Options](#options) below); defaults to `TCBannerOptions()` if omitted
- `bannerTheme` — optional colour scheme override (see [Design and colours](#design-and-colours))
- `onDetailsButtonClick` — callback triggered when the user taps the details button

Options
-------

All options are set through `TCBannerOptions` and have safe defaults, so you only need to specify what you want to override:

```kotlin
TCBannerOptions(
    dimAmount = 0.4f,
    isDismissible = false,
    iconName = "my_app_icon",
    iconSize = 24,
    buttonsAlignment = TCButtonsAlignment.HORIZONTAL,
    buttonsOrder = listOf(TCBannerButtonOrder.REFUSE, TCBannerButtonOrder.DETAILS, TCBannerButtonOrder.ACCEPT),
    compactLayout = false
)
```

| Option | Description | Default | Notes |
|--------|-------------|---------|-------|
| `dimAmount` | Background dim level behind the banner | `0.4f` | `0f` = transparent, `1f` = fully black |
| `isDismissible` | Allow dismissal by tapping outside the banner | `false` | ⚠️ No consent is collected when dismissed this way |
| `iconName` | Name of the drawable resource to display before the title | `null` | Must exist in your app's drawable resources |
| `iconSize` | Size in dp of the icon displayed before the title | `24` | — |
| `buttonsAlignment` | Button layout direction: `HORIZONTAL` or `VERTICAL` | `VERTICAL` | Ignored when `compactLayout` is `true` |
| `buttonsOrder` | Display order of the buttons | `[REFUSE, DETAILS, ACCEPT]` | Ignored when `compactLayout` is `true` |
| `compactLayout` | Use the compact button layout | `false` | See note below |

> [!NOTE]
> Compact layout reduces the visual prominence of the refuse option. Only enable it when you are confident it meets your local regulations — it may not be sufficient in all jurisdictions.

Design and colours
------------------

By default, the banner inherits your app's Material 3 theme colours (`colorSurface`, `colorOnSurface`). Dark mode is supported automatically via the app theme.

To override colours, pass a `TCBannerTheme` to `showBanner()`:

```kotlin
TCConsent.getInstance().showBanner(
    activity = activity,
    bannerTheme = TCBannerTheme(
        lightColorScheme = myLightScheme,
        darkColorScheme = myDarkScheme
    ),
    onDetailsButtonClick = { ... }
)
```

The banner is designed for mobile screens but supports larger screens on tablets.

Colour resolution priority (highest to lowest):

1. `TCBannerTheme` passed to `showBanner()` — supports Dark/Light mode
2. App theme colours (`colorSurface` / `colorOnSurface`) — supports Dark/Light mode
3. Material 3 defaults

Button actions
--------------

- **Accept** — calls `TCConsent.getInstance().acceptAllConsent()`
- **Refuse** — calls `TCConsent.getInstance().refuseAllConsent()`
- **Details** — triggers the `onDetailsButtonClick` callback you pass to `showBanner()`; open the Privacy Center or your own screen here

Privacy statistics are collected automatically on banner display and on each button tap.

Privacy Center Setup
====================

The Privacy Center is the full consent management screen where users toggle individual categories and vendors.

It is driven by `privacy.json`, which describes the interfaces built natively inside the application.
An offline copy is mandatory; an additional copy can be hosted on our CDN for remote updates — the module checks for updates automatically. Your account consultant will provide the `privacy.json` for your project.
Please check Privacy JSON Documentation for more details on how to set every text or option of your Privacy Center.

In the Android SDK the Privacy Center is implemented as an Activity. The offline JSON must be saved in the `src/main/assets` folder.

**IAB and non-IAB launching**

The Privacy Center's behaviour on launch depends on whether the [TCIAB](../TCIAB/README.md) module is linked.

- **Without TCIAB** — the Privacy Center opens directly to the category and vendor management screen, showing only your custom categories and vendors.

- **With TCIAB** — the Privacy Center first shows an IAB-compliant screen (the TCF first layer). The user must scroll through it before the action buttons become active. Tapping Detail opens the full category and vendor screen, which now includes IAB purposes, special purposes, features, special features, and IAB-registered vendors alongside your custom ones.

> [!NOTE]
> No code change is needed between the two modes — linking or removing the TCIAB module is sufficient.

Launching the Privacy Center
----------------------------

```java
Intent PCM = new Intent(getContext(), com.tagcommander.lib.consent.TCPrivacyCenter.class);
startActivity(PCM);
```

Customisation
-------------

Change the default switch state to off:

```java
TCConsent.getInstance().switchDefaultState = false;
```

Disable the back button to force consent:

```java
TCConsent.getInstance().deactivateBackButton = true;
```

Change the activity title:

```java
Intent PCM = new Intent(getContext(), com.tagcommander.lib.privacy.TCPrivacyCenter.class);
PCM.putExtra(kTCIntentExtraCustomTitle, "My custom title");
```

Force JSON update from CDN
--------------------------

By default, a new `privacy.json` is applied immediately. If your configuration is large enough to block the main thread, disable immediate parsing:

```java
TCConsent.getInstance().shouldForceJsonUpdate(false);
```

This saves the new JSON but only parses and uses it on the next launch.

Loading a specific screen directly
-----------------------------------

To open the purpose screen directly (useful when you have your own first-layer banner):

in java:

```java
Intent PCM = new Intent(getContext(), com.tagcommander.lib.consent.TCPrivacyCenter.class);
PCM.putExtra(com.tagcommander.lib.consent.TCConsentConstants.kTCPC_START_SCREEN,
             com.tagcommander.lib.consent.TCConsentConstants.kTCStartWithPurposeScreen);
startActivity(PCM);
```

in kotlin:

```kotlin
val PCM = Intent(getContext(), TCPrivacyCenter::class.java)
PCM.putExtra(TCConsentConstants.kTCPC_START_SCREEN, TCConsentConstants.kTCStartWithPurposeScreen)
startActivity(PCM)
```

For the vendor screen, replace `kTCStartWithPurposeScreen` with `kTCStartWithVendorScreen`.

Reacting to consent
===================

If you need to react to the user giving consent, or to consent being loaded at startup, implement `TCPrivacyCallbacks`.

> [!WARNING]
> Register your callbacks **before** initialising the Consent module. The module checks consent at init and fires callbacks immediately.

**`consentUpdated(Map<String, String> categories)`**

Called when consent is loaded at startup, given inside the Privacy Center, or manually passed to the SDK. The map contains `PRIVACY_CAT_n` and `PRIVACY_VEN_n` keys with `"0"` or `"1"` values. May be empty if nothing was consented to.

**`consentOutdated`**

Called after 6 months without any change in user consent. Use this to force re-displaying the consent screen, similar to first launch.

**`consentCategoryChanged`**

Called when a category is added, removed, or its ID changes in the configuration. Re-display the Privacy Center when this fires.

**`significantChangesInPrivacy`**

Created for IAB. Only fires when `"significantChanges"` is set in `privacy.json`. Not automatic.

You can also listen to SDK start/stop events by registering an Observer implementing `TCEventManager.TCLifecycleListener` via `TCEventManager.registerLifecycleListener()`.

Retaining consent
=================

Consent is saved to our servers automatically. The identifier used by default is `TCDevice.consentID`.

If you need to prove consent or reset saved information, create a dedicated screen in your app for this purpose.

This ID is important — it is the key used to retrieve consent records when proof is required.

Using your own consent ID
-------------------------

```java
TCUser.getInstance().consentID = "myConsentID";
```

Displaying the consent ID to the user
--------------------------------------

You might want to be able to display to your end user the ID used to save the consent. You can simply get it like this:

```java
TCUser.getInstance().consentID
```


Consent validity duration
-------------------------

Default validity is 6 months. To change it via `privacy.json`, add the following inside the `"information"` block:

```json
"consentDurationInMonths": "13"
```

To change it in code (call before init):

```java
TCConsent.getInstance().setConsentDuration(13);
```

Resetting consent
-----------------

```java
TCConsent.getInstance().resetSavedConsent();
```

This resets consent on the device every time it is called. Managing resets per app version must be done manually.

If you are using our Privacy Center, you can also use the `resetSave` field in `privacy.json`. Contact your consultant for details.

Consent version
---------------

To manually change the consent version (e.g. when using your own privacy center):

```java
TCConsent.getInstance().consentVersion = "132";
```

Displaying consent
------------------

If you are familiar with Commanders Act Consent for web, you know that we actually record two things. The first thing is "displaying the consent form". This allows you to prove that a user has indeed been shown the consent screen even if he somehow left without accepting/refusing to give his consent.


Privacy statistics
==================

We have dashboards that provide detailed statistics on the choices your users make. Depending on your app's configuration choices, you may need to call some additional functions.

The four possible flows are:

- Custom banner/popup → our Privacy Center
- Custom banner/popup → custom Privacy Center
- Directly to our Privacy Center
- Custom Privacy Center only

Whenever `saveConsent*` is called, provide the full list of accepted and refused purposes and vendors.

Reference list of functions for our interfaces:

```java
TCConsent.getInstance().refuseAllConsent();
TCConsent.getInstance().acceptAllConsent();
TCConsent.getInstance().statEnterPCToVendorScreen();
TCConsent.getInstance().statViewPrivacyPoliciesFromBanner();
TCConsent.getInstance().getNumberOfIABVendors(); // IAB only
```

Stop privacy statistics tracking
---------------------------------

To disable privacy stat tracking, configure the following parameter:

```java
TCConsent.getInstance().do_not_track = value;
```

Google Consent Mode (Firebase)
==============================

[Google Consent Mode](https://developers.google.com/tag-platform/security/concepts/consent-mode) is a framework that lets you adjust how Google's Firebase Analytics behave based on the consent status of your users. When a user accepts or refuses consent, Google Consent Mode signals that to Firebase so they can adapt their data collection accordingly.

If you are using `TCFirebaseDestination`, you can configure the Consent module to forward consent to `FirebaseAnalytics` automatically.

To use TCConsent to drive Google Consent Mode (GCM) in Firebase Analytics, add a `google_consent_mode` section to the root of your `privacy.json`:

```json
"google_consent_mode": {
    "use_consent_mode": true,
    "infer_ad_from_tcf": false,
    "category_mapping": {
        "ad_storage": 1,
        "ad_user_data": 2,
        "ad_personalization": 3,
        "analytics_storage": 4
    }
}
```

Please refer to the [privacy.json documentation](../res/Privacy_JSON_Documentation.md) for more information.

If you use IAB with `infer_ad_from_tcf: true` (IAB only), the three `ad_*` categories are mapped automatically from the TCF consent string and the `category_mapping` entries for them are ignored.

Debugging Google Consent Mode
------------------------------

TCConsent does not produce logs specific to Firebase consent mode — debugging should be done at the Firebase SDK level and through the Google console.

To verify your integration:

- **Enable DebugView on the Firebase SDK.** Refer to the [official Firebase documentation](https://developers.google.com/tag-platform/security/guides/app-consent?platform=android) for how to do this on Android.

- **Confirm consent is being saved by TCConsent first.** If the user has not accepted or refused yet, nothing will be forwarded. See the [Testing your integration](#testing-your-integration) section above to verify consent is recorded correctly before investigating the Firebase side.

- **Set a breakpoint inside `firebaseConsentChanged`** to confirm the callback fires and that the consent values it receives are what you expect.


Forwarding consent to webViews
==============================

To forward consent to a webView's local storage:

```java
String JSON = TCConsent.getInstance().getConsentAsJson();
```

> [!NOTE]
> This only provides the formatted JSON. You still need JS code inside the web container to consume it. Contact your consultant for that part.

Consent internal API
====================

`TCConsentAPI` provides utility methods to check consent state at any time:

```java
// Should the privacy center be displayed?
public static boolean shouldDisplayPrivacyCenter(Context context)

// Has consent already been given?
public static boolean isConsentAlreadyGiven(Context appContext)

// Epoch timestamp of last saved consent (0 if never)
public static Long getLastTimeConsentWasSaved(Context appContext)

// Category / vendor checks
public static boolean isCategoryAccepted(int ID, Context appContext)
public static boolean isVendorAccepted(int ID, Context appContext)

// Lists of accepted items
public static List<String> getAcceptedCategories(Context appContext)
public static List<String> getAcceptedVendors(Context appContext)
public static List<String> getAllAcceptedConsent(Context appContext)
public static List<String> getAcceptedGoogleVendors(Context appContext)

// IAB-specific checks (IAB only — requires TCIAB module)
public static boolean isIABPurposeAccepted(int ID, Context appContext)
public static boolean isIABVendorAccepted(int ID, Context appContext)
public static boolean isIABSpecialFeatureAccepted(int ID, Context appContext)
```

TCDemo
======

[TCDemo_ServerSide_And_Consent](https://github.com/CommandersAct/TCMobileDemo-V5/tree/master/Android/)

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