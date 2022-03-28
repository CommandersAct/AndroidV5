Changelog Android
=================

*5.0.0 : 03/28 2022*

	~ Only internal changes to go with all other 5.0.0 modules.

*4.6.0 : 11/23 2021*

	~ Updated the full library for AndroidX
	- Removing all LocalBroadcasts

*4.5.0 : 05/20/2021*

	+ Added a way to manually provide the key used by Adobe to provide segments.

*4.4.5 : 04/12/2021*

	~ Removed GAID collection.

*4.4.4 : 10/21 2019*

	+ Adding a way to create offline segments for faster customisation.

*4.4.3 : 09/25 2019*

	+ Adding a "all but for key + value" activation system for partners.

*4.4.2 : 09/23 2019*

	+ Protection when segment list is empty.

*4.4.1 : 09/20 2019*

    ~ /!\ Update Core module alongside this module.
    + Allowing for offline and distance segments to be declared beforehand.
    + Adding a "all but for key" activation system for partners.

*4.3.3 : 06/04 2019*

    ~ Re-checking the initialisation of Adobe Audience Manager each hit to prevent a fail in it's initialisation.


*4.3.2 : 05/09 2019*

	~ Adobe Audience Manager now check privacy by itself, and doesn't wait for the "starting SDK notification".


*4.3.1 : 04/23 2019*

    ~ Calling initSegmentation when getting IDFA and allowedToStart.


*4.3.0 : 03/11 2019*

	+ Adobe Audience Manager
	+ Freewheel to parse Adobe's Segments
