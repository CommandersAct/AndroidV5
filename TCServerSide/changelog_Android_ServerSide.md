Changelog Android
=================

<div class="warning"></div>
>  If you want to check the previous version's changelog, you can find it here :

*5.5.7 : 08/08/2024*

	~ Fix NullPointerException in TCPredefinedVariables.updateVariablesForNewSession().

*5.5.6 : 06/25/2024*

    ~ Fixing occasional consent failure when TCConsent(5.3.3+) is initialized before TCServerSide(5.5.5+)

*5.5.5 : 06/18/2024*

	~ Fix numberSessions NullPointerException in TCPredefinedVariables.

*5.5.4 : 05/28/2024*

	~ Small protection against crash when visitDuration is 0.

*5.5.3 : 03/11/2024*

    ~ Fixing PB_ALWAYS_ENABLED on refused consent (Requires TCCore 5.4.4+ and TCServerSide 5.5.3).

*5.5.2 : 12/08/2023*

	+ Adding consent_version in app payload to help debugging. (Requires Consent 5.2.9+ and Core 5.4.2+)

*5.5.1 : 10/19/2023*

	~ using language and country code from the device and not from the app.
	+ Added 2 function to use legacy ID TC_UNIQUEID to ensure continuity for the old clients still using it.

*5.5.0 : 09/26/2023*

	/!\ Requires TCCore 5.4.0
	+ Added Firebase fork allowing user to send event to both our server-side and Firebase at the same time.
	+ Added device->language as the device default language
	+ Added device->region as the 2 letter country code for the language
	~ Fixing typo in payment_info event name

*5.4.2 : 05/09/2023*

	+ Video events.

*5.4.1 : 04/06/2023*

	+ Added threads security on permanent data.

*5.4.0 : 03/22/2023*

	/!\ Requires TCCore 5.3.2
	+ New events format
	+ Added consistent_anonymous_id for TCUser
	~ Refactored TCUser.firstName & TCUser.lastName & TCApp.nameSpace to camel case


*5.3.1 : 12/08/2022*

	+ Added a way to add a simple json list with addAdditionalParameter


*5.3.0 : 11/17/2022*

    - /!\ Requires Core 5.3.0+
	+ Added `additionalProperties` methods for customising TCApp, TCDevice, TCLifecycle, TCProduct, TCItem & Events
	+ Put most of their properties as public also, but it's not recommended to change them.
	- TCEvent `addAdditionalParameter` methodes are now deprecated, please use `addAdditionalProperty`
	~ valid json payload when PB_ALWAYS_ENABLED and no consent has been given yet
	+ Added "affiliation" as a payload information inside TCItem


*5.2.0 : 10/11/2022*

	- /!\ Requires Core and Consent 5.2.0+
	~ fixing user information in the payload.
	~ fixing ServerSide state bug on relaunch.
	~ Event parameters type cleaning for better Kotlin compatibility.

*5.1.1 : 08/04/2022*

	~ Added back user_agent as a variable inside the device payload.

*5.1.0 : 05/24/2022*

	+ Added enums of classic values for payment methods and purchase status.
	+ Added a function to add AAID/ad_tracking_enabled to the payload.
	~ Modified the event payload to add refused vendors instead of accepted.

*5.0.0 : 03/28 2022*

	~ Renaming SDK to ServerSide since it's now its exact purpose.
	~ Renaming TagCommander to TCServerSide.
	+ Creating all standard "Events" to pass information to the server-side. This is the biggest change, please check the module documentation.
	+ Added a sourceKey to default TCServerSide parameters.
	- Removed some classes that are now useless like TCAppVars or the old TCProduct.
	- Removed from the predefined variables some variables that are not used anymore.

*4.6.0 : 11/23 2021*

	~ Updated the full library for AndroidX
	- Removing all LocalBroadcasts


*4.5.1 : 05/17 2021*
	/!\ Requires TCCore 4.6.1
	+ new way to change the SDK default beheviour depending on the privacy you are using. Please check the 'Privacy' part in the SDK's README.

*4.5.0 : 04/12 2021*

	- /!\ Removed the automatic collection of Google Advertising ID to be RGPD compliant. If you need it, you'll need to collect it and add it to the datalayer.

*4.4.2 : 07/30 2020*

	~ Consent hits used for statistics are now by-passing disabled SDK.

*4.4.1 : 04/24 2020*

	~ Modified TagCommander's activateSDK and deactivateSDK to throw the corresponding notification to propagate it to the whole system.

*4.4.0 : 12/18 2019*

	/!\ With this update you need to also update the core module to 4.5.0+
	- Removed battery monitoring.

*4.3.1 : 02/04 2019*

	~ Put back function to deactivate the SDK manually in the case we don't need the full privacy module and still want to deactivate the SDK.

*4.3.0 : 12/05 2018*

	- Moved TCUser_agent in Core
	- Moved TCNetworkManager and TCWaitingQueue which are now part of Core./Users/jeanjulien/Sources/Mobile/Docs/changelogs/changelog_iOS_SDK.md

*4.2.2 : 08/06 2018*

	~ Refactoring on consent hits to give them a proper route.

*4.2.1 : 08/01 2018*

	+ Protecting the addition of data in the datalayer when the SDK is disabled.

*4.2.0 : 06/22 2018*

	~ Cleaning on android > 15 which is now always true.

*4.1.5 : 01/15 2018*

	~ Forcing the calls to Commanders Act server-side to be https. Please read the documentation at "Network monitor" as this is impacted.

*4.1.4 : 12/01 2017*

	+ You can now disable the SDK by calling disableSDK() nothing will be treated by the SDK after this.

*4.1.3 : 11/27 2017*

	+ Added Background Mode, a way to force the SDK to work when the application is in background.
	- Removed the ways to directly touch the DynamicStores used by the system classes.
	~ Better synchronisation for the DynamicStores

*4.1.2 : 10/10 2017*

	- removed Location permissions in the SDK manifest since it's not useful if you're not using TCLocation. You will need to add it in your application yourself if it's not already present.

*4.1.1 : 05/02 2017*

	~ Updated modules' manifests to remove useless notions like application->allowrtl

*4.1.0 : 02/06 2017*

	+ Beacon module release.

*4.0.2 : 01/10 2017*

	~ removed the dependencies in the POM generated for the JCenter deployement.

*4.0.1 : 12/14 2016*

	~ Updated CompileSDK, BuildTools, TargetSDK, appCompat and PlayServices versions.
	~ Corrected warnings in Lint report.

*4.0.0 : 12/12 2016*

    + Added JCenter support
    ~ Transformed the SDK from a light SDK to a "Server-side" only SDK. We will now only send information to our server to manage the reste of the treatment.
    - Removed functionalities for managing tags (Conditions and Exernal configuration)
