Changelog Android
=================

<div class="warning"></div>
>  If you want to check the previous version's changelog, you can find it here :

*5.1.0 : 05/24/2022*

	+ Added enums of classic values for payment methods and purchase status.
	+ Added a function to add AAID/ad_tracking_enabled to the payload.
	~ Modified the event payload to add refused vendors instead of accepted.

*5.0.0 : 03/28 2022*

	~ Renaming SDK to ServerSide since it's now its exact purpose.
	~ Renaming TagCommander to TCServerSide
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
