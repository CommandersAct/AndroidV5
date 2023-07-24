Changelog Android
=================

<div class="warning"></div>
> If you want to check the previous version's changelog, you can find it here :

*5.3.4 : 07/24/2023*

	~ added required functionalities for IAB TCF v2.2

*5.3.3 : 06/28/2023*

	~ Fixing background hits sometimes duplicating some events
	~ Fixing consent given after some events where created and consent might not be properly replaced
	- Removed offline time limit retention as most vendors now support timestamps

*5.3.2 : 03/22/2023*

	+ Pretty format for event logging
	~ Added consistent_anonymous_id for TCUser 
	~ Bug on full consent not always overriding properly old consent.

*5.3.1 : 12/26/2022*

	+ Added a way to add a simple json list with addAdditionalParameter


*5.3.0 : 11/17/2022*

	+ Added `additionalProperties` methods for customizing TCNetwork & TCUser

*5.2.0 : 10/11/2022*

	- /!\ Requires ServerSide and Consent 5.2.0+
	~ fixing ServerSide state bug on relaunch.

*5.1.0 : 05/24/2022*

	~ adding functions for faster saving of HashMaps.
	- Simplified collection of aaid/ad_tracking_enabled.

*5.0.0 : 03/28 2022*

	~ Only internal renamings to go with all other 5.0.0 modules.

*4.7.0 : 11/23 2021*

	~ Updated the full library for AndroidX
	- Removing all LocalBroadcasts

*4.6.6 : 11/10 2021*

	~ fix for ConnectivityManager sometimes unable to register connection.

*4.6.5 : 10/18 2021*

	~ fix for network receiver's crash on Android 11 and re-registering after a disable SDK in the same session.

*4.6.4 : 09/28 2021*

	~ Hot fix for network callbacks which were badly unregistered.

*4.6.3 : 09/12 2021*

	~ Fixing network callbacks which were lost sometimes.

*4.6.2 : 09/01 2021*

	~ [Privacy] Fixing configuration parsing which would lead to a privacy version at 0 in some rare cases.

*4.6.1 : 05/12 2021*

	+ functions to manipulate the operation queue's state from the SDK.

*4.6.0 : 04/12 2021*

	- /!\ Removed the automatic collection of Google Advertising ID to be RGPD compliant. If you need it, you'll need to collect it and add it to the datalayer.

*4.5.4 : 02/23 2020*

	~ Modified configuration retrieval to add if-modified-since in the header.

*4.5.3 : 09/07/2020*

    ~ Modified the sharedPreferences functions to allow fetching for data in integer format.

*4.5.2 : 08/09/2020*

	~ Modified the sharedPreferences functions to allow fetching data either in default storage or in the TC storage.

*4.5.1 : 07/30/2020*

	~ Configuration files updates are now raising a notification.

*4.5.0 : 12/18 2019*

	/!\ With this update you need to also update the SDK module to 4.4.0+
	- Removed battery monitoring.

*4.4.3 : 10/23 2019*

	~ Correction for 4.4.2 where no offline configuration would still cause a crash in preSaveOfflineJSON.

*4.4.2 : 10/14 2019*

	+ Removed the need to have the offline counterpart of configuration files.

*4.4.1 : 09/20 2019*

    + Refactoring on configuration files which now are in the priority queue.

*4.3.2 : 03/12 2019*

    + Added route for Partners hits

*4.3.1 : 01/18 2019*

    + Modified call for android SDK <= 19 to force TLSv1.2 to match server updates.

*4.3.0 : 12/05 2018*

	+ Moved TCUser_agent in Core
	+ Moved TCNetworkManager and TCWaitingQueue which are now part of Core./Users/jeanjulien/Sources/Mobile/Docs/changelogs/changelog_iOS_SDK.md

*4.2.1 : 08/01 2018*

	+ We can now modify a TCHTTPOperation to put additional post data.

*4.2.0 : 06/22 2018*

	+ Privacy notifications.

*4.1.9 : 05/25 2018*

    ~ Changed list types and synchronisation methods for the DynamicStores

*4.1.8 : 05/23 2018*

    ~ Default logging to nothing. Please call setDebugLevel only if you want logs.

*4.1.7 : 03/27 2018*

	~ Better synchronisation for the DynamicStores

*4.1.6 : 12/01 2017*

	+ You can now disable the SDK by calling disableSDK() nothing will be treated by the SDK after this.

*4.1.5 : 11/27 2017*

	+ Added Background Mode, a way to force the SDK to work when the application is in background.
	- Removed the ways to directly touch the DynamicStores used by the system classes.
	~ Better synchronisation for the DynamicStores

*4.1.4 : 11/10 2017*

	+ Better synchronisation around the DynamicStore to prevent issues with late AAID.

*4.1.3 : 08/03 2017*

	+ Added TC_SDK_ID and TC_NORMALIZED_ID

*4.1.2 : 05/02 2017*

	~ Updated modules' manifests to remove useless notions like application->allowrtl

*4.1.1 : 03/29 2017*

	~ Corrected a missing log in the TCCore module.

*4.1.0 : 02/06 2017*

	+ Beacon module release.

*4.0.2 : 01/10 2017*

	~ removed the dependencies in the POM generated for the JCenter deployement.

*4.0.1 : 12/14 2016*

	~ Updated CompileSDK, BuildTools, TargetSDK, appCompat and PlayServices versions.
	~ Corrected warnings in Lint report.

*4.0.0 : 12/12 2016*

	+ Added JCenter support
    + Separated all the core functions into a separated and reusable module.
    + Simplified TCDebug class
    - Removed the log output "file"
