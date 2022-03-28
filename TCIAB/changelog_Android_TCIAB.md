Changelog Android
=================

*5.0.0 : 03/28 2022*

	~ Small refactorings to be up to date with the other changes from ServerSide, Core and Consent.

*4.7.1 : 11/23 2021*

	~ Fix on resetSave for AC-String.

*4.7.0 : 09/01 2021*

    /!\ Requires TCPrivacy 4.9.0+
    + We can now build Google AC-String.

*4.6.1 : 07/19 2021*

    ~ Fixing publisher TC Builder so that it will be possible to work with custom categories later on.

*4.6.0 : 05/18 2021*

	/!\ Requires TCPrivacy 4.8.0+
	~ Refactored the project namespace to be able to push it to Maven Central.

*4.5.4 : 04/12 2021*

    ~ Forcing uppercase language name for consent string standards.

*4.5.3 : 09/07 2020*

    /!\ Requires Core 4.5.3+
	~ Corrected the type of some of the key stored for IAB.
	+ Adding the possibility to save the PublisherTC part of the consent string. Please check the documentation.

*4.5.2 : 08/27 2020*

    /!\ Requires Core 4.5.2+
	~ Corrected the sharedPreferences used for IAB keys, now correctly saving in default shared preferences.

*4.5.1 : 08/07 2020*

    ~ Corrected IABTCF_gdprApplies which wasn't saved correctly.
    ~ Corrected TCData which didn't record properly legitimate interests

*4.5.0 : 07/30 2020*

	/!\ Requires Privacy 4.6.0+, Core 4.5.1+
	+ IAB's consentString is now in version 2.
	+ Saving information in TCData.

*4.3.2 : 10/31 2019*

    ~ Corrected the key IABConsent_SubjectToGDPR which was not saved as a string.

*4.3.1 : 10/24 2019*

	+ Added several new keys to the user defaults.
	+ IABConsent_SubjectToGDPR which defaults to "1"
	+ IABConsent_ParsedVendorConsents String of “0”s and “1”s, where the character at position N indicates the consent status to vendorID N as defined in the Global Vendor List.
	+ IABConsent_ParsedPurposeConsents String of “0”s and “1”s, where the character at position N indicates the consent status to purposeID N as defined in the Global Vendor List.

*4.3.0 : 05/15 2019*

	+ Release of the first version of IAB consent string framework
