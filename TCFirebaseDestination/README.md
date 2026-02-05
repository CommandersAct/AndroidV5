![alt tag](../res/ca_logo.png)


Firebase Destination Implementation Guide
=========================================

Last update : *05/02/2026*

Release version : *5.1.4*

## Table of Contents

- [Firebase Destination Implementation Guide](#firebase-destination-implementation-guide)
- [Introduction](#introduction)
- [Setup](#setup)
- [Supported Firebase Event](#supported-firebase-event)
- [Google Consent Mode :](#google-consent-mode)

Introduction
============

Commanders Act's Firebase destination library for Android.
This library is for android only, please refer to [TCServerSide iOS documentation](https://github.com/CommandersAct/iOSV5/tree/master/TCServerSide) if you want the same functionalities on iOS.

Setup
=====
You'll need to correctly set up Firebase SDK first into your app, please refer to the official firebase documentation to do so.
Once you have your firebase SDK running and your `google-services.json` into your app bundle, you only need to add the Firebase Destination module into your app build.gradle.


```
    implementation 'com.tagcommander.lib:FirebaseDestination:5.0.0'
```


#Usage :

You can set your firebase user properties directly into your firebaseAnalytics instance.

```
    FirebaseAnalytics.setUserProperty("favorite_food", food)
```

You'll need to initialise the module with your application context :

```
        TCFirebase.getInstance().initialize(appContext)
```

Supported Firebase Event  
========================

We highly recommend only using TCCustomEvent when forwarding events to firebase. 
Make sure your events are compatible with firebase specifications to prevent any errors.

code example in kotlin : 

```kotlin
        val item1 = JSONObject().apply {
            put("item_id",      "1234")
            put("item_name",    "XWU-1")
            put("item_category","football")
            put("item_variant", "blue")
        }

        val item2 = JSONObject().apply {
            put(Param.ITEM_ID,   "5678")   // Firebase constants still work
            put(Param.ITEM_NAME, "ZPA-13")
            put("item_category",            "basketball")
            put("item_variant",             "orange")
        }

        val items: MutableList<JSONObject> = mutableListOf(item1, item2)

        val addToCartEvent = TCCustomEvent("add_to_cart").apply {
            addAdditionalProperty("currency", "USD")
            addAdditionalProperty("value",   30)          
            addAdditionalProperty("items",   items)      
            addAdditionalProperty("item_variant", "1234")
            addAdditionalProperty("price", 1234)       
        }

        tc?.execute(addToCartEvent)
```

Specs and requirements differ between TCEvents and Firebase Events, if you still want to use our TCEvents, you'll need to make sure that your TCEvents match Firebase recommendations too (required, allowed and non authorized parameters)
Events will be mapped like the following, and the TCServerSide will try and log the event to firebase.
You'll also need to configure every new parameter in your firebase console


|       TCEvent Property       |        Firebase Property     |
|------------------------------|------------------------------|
|  event.items[i].id           |  event.items[i].item_id      |
|  event.items[i].X            |  event.items[i].X            |
|  event.items[i].product.name |  event.items[i].item_name    |
|  event.items[i].product.X    |  event.items[i].tc_product_X | 

The predefined variables related to events (such as TCDevice and TCNetwork) aren't included in the Firebase event because they are already being gathered by Firebase SDK.


Google Consent Mode : 
=====================

You can use our TCConsent module to automatically set and collect consents from your users. For further details, please refer to our TCConsent documentation.