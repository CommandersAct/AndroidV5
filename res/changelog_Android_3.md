Changelog Android
=================

*3.2.2016-09-26*

    + Added a new predefined variable representing the first launch of the application.

*3.2.2016-08-22*

    + Added a new predefined variable representing the first execute after the launch of the application.

*3.2.2016-06-03*

    ~ Fixed concurrency issues with empty operations while saving offline hits.

*3.2.2016-05-24*

    ~ Modified the location updates to every 30 minutes.
    + Location updates interval can now be set with TCLocation.GPSInterval which override the default value.

*3.2.2016-05-19*

    - Removed IP and Location as mandatory parameters for hits to be sent.
    + if the tracking of ads is limited by the user, default IDFA is now "00000000-0000-0000-0000-000000000000" instead of an empty string.

*3.2.2016-05-17*

    ~ Protected the SDK against non given permissions.

*3.2.2016-04-27*

    ~ Crash fix : Missing TCLifeCycleCallback class on ICE_CREAM_SANDWICH (Android 14 and less).
      /!\ The background and foreground timestamps won't be computed on 2.x and 3.x devices.

*3.2.2016-04-21*

    + INSTALL_REFERRER is now available in the #TC_INSTALL_REFERRER# predefined variable. Please read the documentation to send the ReferrerBroadcast to TagCommander.
    ~ TCDemo has better variable names.
    ~ Some improvements on the documentation.
    ~ Fixed a very rare crash with corrupted stored data.
    ~ Offlines hits are kept a longer time.
    ~ Updated Android build tools (23.0.3).

*3.2.2016-04-07*

    ~ Completed TCProduct and added the possibility to add custom properties.
    ~ Updated dependency to Google Play Service to version 8.4.0

*3.2.2016-03-31*

    ~ Corrected some timestamps in TCComScore start hits.

*3.2.2016-03-15*

    ~ Corrected a bug on Lifecycle where the on Foreground event would be sent too often, that lead to issues in background and foreground time.

*3.2.2016-03-03*

    ~ Added a generic "try catch" around WebSettings.getDefaultUserAgent to catch issues due to the update of the system webview.

*3.2.2016-02-17*

    ~ Corrected a small bug on TCCustomURL when we have additional parameters, but no regular parameters.

*3.2.2016-02-10*

    ~ ComScore protection against initialisation hits with no conditions.

*3.2.2016-01-27*

    ~ Updated Google Play services and Gradle plugin version.

*3.2.2015-12-24*

    ~ Corrected a bug on IP adresses with IP v6.
    + Merry Christmas from the TagCommander Mobile team.

*3.2.2015-10-28*

    + Added synonym system to rename most predefined variables to a simpler and cleaner name while still supporting the old names.
    ~ updated the documentation of the predefined variables. (Don't hesitate to ask for this documentation if you need it!)
    ~ updated build tools, compile sdk, gradle version and play services version.

*3.1.2015-10-07*

    + New major version with Adserving support with TCAdServing
    ~ Support for Flurry 6.1.0 (Analytics and Adserving)
    + Support for Smart Ad Server 4.2.2 (Adserving)
    + Added support for ComScore scorecardresearch
    + Support for additional parameters for all the vendors
    + Added initialization and configuration tags
    ~ Refactoring and regrouping of all predefined variables
    ~ CDN downloading is now using HTTPS
    ~ Miscallenous unit tests fixes
    + Added platform changelog in PDF documentation

*2.6.2015-08-06*

	~ Better way of handling dependencies against vendors.

*2.6.2015-07-30*

	+ Added predefined variables #TC_LAST_SESSION_LAST_HIT# and #TC_LAST_SESSION_LAST_HIT_MS#

*2.6.2015-06-24*

    + Added support for Ad Master's Conversion Master SDK

*2.5.2015-06-16*

    ~ Updated dependency to Google Play Service to version 7.5.0

*2.5.2015-05-26*

    + Added support of Lotame's CrowdControl
    + Helper class to pass information to Flurry.

*2.4.2015-05-07*

    ~ Fix on container reload frequency

*2.4.2015-04-27*

    + Better thread synchronisation on removing hit from the TCWaitingQueue
    ~ Fix crash on Null parsing in TCPredefinedVariables init
    - Simplified Google Play Service dependency to only "play-services-location" (version 6.5.87)

*2.4.2015-03-30*

    ~ Fix session time (20 seconds to 30 minutes)
    ~ Fix rare crash on onLowBattery
    + Better log message on execute if no container is found
    + TagCommander version in samplingTag

*2.4.2015-03-27*

    + New predefined internal variable : #TC_NAV_TIMESTAMP_LAST_CALL#, #TC_NAV_TIMESTAMP_LAST_CALL_MS#
    + New predefined internal variable : #TC_TIMESTAMP_CURRENT_VISIT#, #TC_TIMESTAMP_CURRENT_VISIT_MS# (default session time is 30 minutes)
    ~ Fixed #TC_NAV_NUMBER_VISITS# (cold starts) #TC_NUMBER_VISITS# (session enabled)
    ~ Much more accurate timings onForeground / on Background
    ~ Fixed bad WAITING / EXECUTING state in TCNetworkManager
    ~ Fixed timings on location delegate and #TC_IDFA#
    ~ Better handling of MainActivity destroyed and recreated
    ~ Better Support for ComScore (onStart tag)
    ~ TCDemo : Fixed weird TagCommander instantiation

*2.3.2014-12-05*

    + Support for ComScore
    + Added many predefined variables and added a milliseconds counterpart on all timestamps
    + Fixed #TC_NAV_TIMESTAMP_CURRENT_VISIT# to update at each call

*2.2.2014-09-12*

    + Support for Criteo
    + Better compatibility with Pro Guard
    + Better and cleaner documentation
    ~ #TC_IDFA# as a waiting condition
    + Better unit tests in TCParser
    + Bug fix : Don't apply TCFunction if invalid

*2.2.2014-08-18*

    + Support for embedded Google Conversion (v2.1.0)
    + Two-Dimension array support
    + Mapped Variables
    + Internal Variables
    + Processing functions
    ~ Cleanup internal initialisation
    ~ More robust vendors notifications
    ~ Updated documentation
    ~ Automated releases
    ~ Code cleanup
    ~ Better logs messages
    + Variable suggestion when it's not found
    + New conditions : DOESNTCONTAIN, NOTEMPTY, NOTEQUAL, NOTGREATERTHAN, NOTLESSTHAN
    ~ Automated releases
    ~ Bug fix : special cases removeQuotes
    + Some more performance optimisations


*2.0.2014-07-15*

    ~More documentation on the migration from v1 to v2

*2.0.1:*

    ~ Bug fix : memory leak when the application has many pending hits
    ~ Bug fix : TCSampling is now working properly
    + Compatibility with Android 10 (but at the cost of the offline mode between Android 10 and Android 13)

*2.0.0*

    - First release
