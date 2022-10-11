
<html>
<body>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<h1 id="serversides-implementation-guide">ServerSide's Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>11/10/2022</em><br />
Release version : <em>5.2.0</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#serversides-implementation-guide">ServerSide's Implementation Guide</a></li>
<li><a href="#introduction">Introduction</a><ul>
<li><a href="#main-technical-specifications">Main Technical Specifications</a></li>
<li><a href="#event">Event</a></li>
<li><a href="#event-details">Event details</a></li>
<li><a href="#executing-an-event">Executing an event</a></li>
</ul>
</li>
<li><a href="#serversides-module-integration">ServerSide's module integration</a><ul>
<li><a href="#steps">Steps</a></li>
<li><a href="#integration-of-the-serverside-module">Integration of the ServerSide Module</a></li>
<li><a href="#gradle-additions">Gradle additions</a></li>
<li><a href="#android-permissions">Android permissions</a></li>
<li><a href="#compatibility">Compatibility</a></li>
</ul>
</li>
<li><a href="#using-the-serversides-module">Using the ServerSide's module</a><ul>
<li><a href="#initialisation">Initialisation</a></li>
<li><a href="#executing-events">Executing events</a></li>
<li><a href="#additional-parameters">Additional parameters</a></li>
<li><a href="#custom-events">Custom events</a></li>
<li><a href="#consent">Consent</a></li>
<li><a href="#install-referrer">Install Referrer</a></li>
<li><a href="#background-mode">Background Mode</a></li>
<li><a href="#deactivating-the-serversides-module">Deactivating the ServerSide's module</a></li>
<li><a href="#getting-aaid">Getting AAID</a></li>
</ul>
</li>
<li><a href="#troubleshooting">Troubleshooting</a><ul>
<li><a href="#debugging">Debugging</a></li>
<li><a href="#testing">Testing</a></li>
<li><a href="#network-monitor">Network monitor</a></li>
<li><a href="#common-errors">Common errors</a></li>
</ul>
</li>
<li><a href="#helpers">Helpers</a><ul>
<li><a href="#persisting-variables">Persisting variables</a></li>
</ul>
</li>
<li><a href="#kotlin">Kotlin</a></li>
<li><a href="#example-tcdemo">Example: TCDemo</a></li>
<li><a href="#migration-v4-to-v5">Migration v4 to v5</a><ul>
<li><a href="#why-a-new-version-of-the-sdk">Why a new version of the SDK</a></li>
<li><a href="#event-based">Event based</a></li>
<li><a href="#changes">Changes</a></li>
<li><a href="#example">Example</a></li>
</ul>
</li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>Commanders Act enables marketers to easily add, edit, update, and deactivate tags on web pages, videos and mobile applications with little-to-no support from IT departments.</p>
<p>Instead of implementing several SDK's in the application, Commanders Act for mobile provides clients with a single module which sends data to our servers which then create and send information to your partners.</p>
<p>Thanks to remote configuration tools, it is also possible to modify the configuration without having to resubmit your application.</p>
<p>The purpose of this document is to explain how to add the ServerSide module into your application.</p>
<h2 id="main-technical-specifications">Main Technical Specifications</h2>
<ul>
<li>Weight about 40 ko in your application.</li>
<li>Fully threaded and asynchronous.</li>
<li>Offline mode (the hits are stored in the phone to be replayed when is convenient)</li>
<li>Very low CPU and memory usage.</li>
<li>Information collected and sent automatically while respecting GDPR.</li>
<li>Background mode, in the case you need to send data while the application is in background.</li>
</ul>
<h2 id="event">Event</h2>
<p>An event represent something happening inside your application. For example, we have "add to cart" or "login" events.
Inside the library they are represented each by a specific class which in turn provide you with information needed for this event to be used by your solutions.</p>
<p>For example, we know that for a "view cart" event, you will have to provide a list of the items inside the cart for the event to be valid.
We also add "value" and "currency" that are generally used by solutions for this event that can be filled inside the class.</p>
<p>Your company alongside our consulting team will usually define step by step what events the want the application to send and what parameters are needed for the solutions which will in turn treat those events.</p>
<p>You should be provided with a document explaining all events you need to implement inside your application and when they should be sent.</p>
<p>The event and the information we gather independently will create a hit to our servers with a JSON payload.</p>
<h2 id="event-details">Event details</h2>
<p>All events and their payloads are detailed here: <a href="https://community.commandersact.com/platform-x/developers/tracking/events-reference">events-reference</a></p>
<p>You will also find information about what you can add inside the TCUser which is sent with every hit.
Be aware that some data inside TCUser require consent from the user te be read and used.</p>
<p>You can also check this page to see the link between the event names and the SDK's Class names and all information inside the payload here:
<a href="https://community.commandersact.com/platform-x/developers/tracking/about-events/mobile-sdk-event-specificity">mobile-sdk-event-specificity</a></p>
<h2 id="executing-an-event">Executing an event</h2>
<p>When you call the sendData method, a hit will be packaged and sent to Commanders Act's server.</p>
<p><img alt="alt tag" src="../res/server_side_module_scheme.png" /></p>
<h1 id="serversides-module-integration">ServerSide's module integration</h1>
<h2 id="steps">Steps</h2>
<p>You can divide the integration of TagCommander's ServerSide module into the next few steps:</p>
<ol>
<li>Adding the Core and ServerSide libraries to your Project.</li>
<li>Implementing the ServerSide module and adding events to your application.</li>
<li>Verify that all tags are being sent.</li>
</ol>
<h2 id="integration-of-the-serverside-module">Integration of the ServerSide Module</h2>
<p><a href="../README.md">Please check the Developers Implementation Guide to chose the best way to implement this module in your project.</a></p>
<h2 id="gradle-additions">Gradle additions</h2>
<p>You might need to add some dependencies in your build.gradle file for the ServerSide's module to work properly.</p>
<pre><code>implementation 'androidx.appcompat:appcompat:1.4.1'
</code></pre>
<h2 id="android-permissions">Android permissions</h2>
<p>TagCommander requires the following permissions:</p>
<ul>
<li>android.permission.INTERNET</li>
<li>android.permission.ACCESS_NETWORK_STATE</li>
<li>android.permission.ACCESS_WIFI_STATE</li>
</ul>
<h2 id="compatibility">Compatibility</h2>
<ul>
<li>Minimum Android version: 17</li>
<li>Build Target version: 31</li>
<li>Build Tools Version: 28.0.3</li>
</ul>
<h1 id="using-the-serversides-module">Using the ServerSide's module</h1>
<h2 id="initialisation">Initialisation</h2>
<p>It is recommended to initialize TCServerSide in your <code>onCreate(Bundle savedInstanceState)</code> in your <code>MainActivity</code> so it will be operational as soon as possible.</p>
<p>You will need 2 things provided by our consulting team. A siteID which is representing the web platform in which you setup your destinations.
And a sourceKeyID which will represent the Android source inside your setup.</p>
<p>You need to pass your application context while instantiating TCServerSide.</p>
<p>If you are using our Consent module, you can also change during this initialisation the default TCServerSide behaviour while waiting for the user consent.
More information a bit later in this document.</p>
<p>A single line of code is required to initialize an instance of TCServerSide, and you can add one more for better logging:</p>
<pre><code>//!\\ Important while integrating TCServerSide
TCDebug.setDebugLevel(Log.VERBOSE);
TCServerSide TCS = new TCServerSide(siteID, sourceKey, appContext);
</code></pre>
<h2 id="executing-events">Executing events</h2>
<p>Each time you are required to launch an event, simply instantiate the corresponding event, fill it with what your tagging plan suggest and execute it.</p>
<pre><code>ArrayList&lt;TCItem&gt; items = new ArrayList&lt;&gt;();
items.add(new TCItem("iID1", new TCProduct("pID1", "pName1", 1.5f), 1));
items.add(new TCItem("iID2", new TCProduct("pID2", "pName2", 2.5f), 2));
TCPurchaseEvent event = new TCPurchaseEvent("ID", 11.2f, 4.5f, "EUR", "purchase", "creditCard", "waiting", items);
TCS.execute(event);
</code></pre>
<h2 id="additional-parameters">Additional parameters</h2>
<p>Events are tailored for the most common solutions' needs. But you might need to add parameters that are not specified in the event you are trying to send.</p>
<pre><code>TCPageViewEvent pageViewEvent = new TCPageViewEvent("Consent");
pageViewEvent.pageName = "Configuration";
pageViewEvent.addAdditionalParameter("currentConsent", "refused");
</code></pre>
<p>Here for example this could be tracking some user going back to your configuration to open the consent interface. And you would want to know what was the consent before re-opening.
Of course this is a simple example only here to show the addAdditionalParameter method.</p>
<h2 id="custom-events">Custom events</h2>
<p>In some case, the classic events might not suit your needs, in this case you can build complete custom events.
It is important to name them properly as this will be the base of forwarding them to your destinations.</p>
<pre><code>TCCustomEvent event = new TCCustomEvent("eventName");
event.addAdditionalParameter("myParam", "myValue");
TCS.execute(event);
</code></pre>
<h2 id="consent">Consent</h2>
<p>To manage the privacy of the user's data you can use our Consent product, another product or nothing at all.</p>
<p>By default, the ServerSide module will try to see if you have added our Privacy module. If so, it will put itself into a waiting for consent mode.
In this mode, it will record all hits but wait to consent information to either send everything or delete all waiting hits.</p>
<p>If you don't use our Consent module, the ServerSide's will be enabled by default.</p>
<p>If you want to change this behaviour, we added a way to initialise the ServerSide module with additional information about the behaviour.
We have 3 behaviours:</p>
<pre><code>- PB_DEFAULT_BEHAVIOUR which is the one described just before
- PB_ALWAYS_ENABLED which forces the ServerSide's module to always send information. This is used when you have tags that don't require consent.
- PB_DISABLED_BY_DEFAULT which forces the ServerSide's module to disabled. It won't record hits before consent is given and you won't have any up by default time when using tagging the app loading screens. This is used when you're not using our Consent module.
</code></pre>
<p>Consent will then be forwarded inside the TCUser. For more information, please check documentation about the <a href="../TCConsent/README.md">Consent module</a>. </p>
<p>To initialise the ServerSide's module with another behaviour, please call the following function:</p>
<pre><code>TCS = new TCServerSide(siteID, sourceKey, appContext, ETCConsentBehaviour.PB_DEFAULT_BEHAVIOUR);
</code></pre>
<h2 id="install-referrer">Install Referrer</h2>
<p>If you want the source channel of your application in Commanders Act, please point the INSTALL_REFERRER broadcast toward our receiver TCReferrerReceiver :</p>
<p>It's as simple as adding the following lines in the AndroidManifest.xml of your application and inside the "Application" tag.</p>
<pre><code>&lt;receiver
    android:name="com.tagcommander.lib.TCReferrerReceiver"
    android:exported="true"&gt;
    &lt;intent-filter&gt;
        &lt;action android:name="com.android.vending.INSTALL_REFERRER" /&gt;
    &lt;/intent-filter&gt;
&lt;/receiver&gt;
</code></pre>
<p>Once the broadcast is received, TagCommander will store the full string into the #TC_INSTALL_REFERRER# predefined variable.</p>
<p>Example:</p>
<pre><code>utm_source=adMob&amp;utm_medium=banner&amp;utm_term=running+shoes&amp;utm_content=theContent
&amp;utm_campaign=couponReduc&amp;anid=adMob
</code></pre>
<h2 id="background-mode">Background Mode</h2>
<p>While the application is going to background, the ServerSide's module sends all data that was already queued then stops. This is in order to preserve battery life and not use carrier data when not required.</p>
<p>But some applications need to be able to continue sending data because they have real background activities. For example listening to music.</p>
<p>For those cases, we added a way to bypass the way the ServerSide's module usually react to background. Please call:</p>
<pre><code>TCS.enableRunningInBackground();
</code></pre>
<p>One drawback is that we're not able to ascertain when the application will really be killed. In normal mode, we're saving all hits not sent when going in the background, which is not possible here anymore. To be sure to not loose any hits in background mode, we will save much more often the offline hits. This only applies if the ServerSide is offline, meaning that you don't have internet.</p>
<h2 id="deactivating-the-serversides-module">Deactivating the ServerSide's module</h2>
<p>We have two ways to enable/disable our ServerSide module. The one described here should be used when your are not using our Consent module.</p>
<p>If you want to show a privacy message to your users allowing them to stop the tracking, you might want to use the following function to stop it if they refuse to be tracked.</p>
<pre><code>TCS.disableServerSide();
</code></pre>
<p>What this function does is stopping all systems in the ServerSide's module that update automatically or listen to notifications like background or internet reachability. This will also ignore all calls to the SDK by your application so that nothing is treated anymore and you don't have to protect those calls manually.</p>
<pre><code>TCS.enableServerSide();
</code></pre>
<p>In the case you need to re-enable it after disabling it the first time, you can use this function.</p>
<h2 id="getting-aaid">Getting AAID</h2>
<p>For privacy reason, the server-side module can't read and use the AAID automatically. We need to first be sure that your user have accepted the corresponding category inside the privacy.</p>
<pre><code>ServerSideInstance.addAdvertisingIDs());
</code></pre>
<p>This method will check and add if possible the AAID and the boolean "is ad tracking enabled".</p>
<h1 id="troubleshooting">Troubleshooting</h1>
<p>The ServerSide also offers methods to help you with the Quality Assessment of the implementation.</p>
<h2 id="debugging">Debugging</h2>
<p>We recommend using <code>Log.VERBOSE</code> while developing your application:</p>
<pre><code>/*
 * Verbose is recommended during test as it prints information
 * that helps figuring what is working and what's not.
 */
  TCDebug.setDebugLevel(Log.VERBOSE);
</code></pre>
<ul>
<li>Verbosity</li>
</ul>
<table>
<thead>
<tr>
<th>Constant Name</th>
<th>Verbosity</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Log.VERBOSE</code></td>
<td>Print everything.</td>
</tr>
<tr>
<td><code>Log.DEBUG</code></td>
<td>Most useful information for debugging</td>
</tr>
<tr>
<td><code>Log.INFO</code></td>
<td>Basic information about TagCommander's state</td>
</tr>
<tr>
<td><code>Log.WARN</code></td>
<td>Warnings only</td>
</tr>
<tr>
<td><code>Log.ERRORS</code></td>
<td>Errors only</td>
</tr>
<tr>
<td><code>Log.ASSERT</code></td>
<td>Assertions only (not used).</td>
</tr>
</tbody>
</table>
<ul>
<li>The internal architecture is working with internal notifications. You can ask the Logger to display all the internal notifications with TCDebug.setNotificationLog(true);.</li>
</ul>
<p>If you don't call TCDebug.setDebugLevel, not log will be printed at all.</p>
<h2 id="testing">Testing</h2>
<p>There are four ways to verify that the module executes the tags in your application:</p>
<ul>
<li>By reading the debug messages in the console.</li>
<li>To check the interfaces inside the platform.</li>
<li>By going to your vendor's platform and check that the hits are displayed and that the data is correct. Please be aware that hits may not display immediately in the vendor account. This delay differs widely between vendors and may also vary for the type of hit under the same vendor.</li>
<li>You can also use a network monitor like Wireshark or Charles to check directly what is being sent on the wire to your vendors.</li>
</ul>
<h2 id="network-monitor">Network monitor</h2>
<p>Starting Android 7 (Nougat) you will have more troubles while trying to profile your applications with tools like Charles. Google introduced <a href="https://android-developers.googleblog.com/2016/07/changes-to-trusted-certificate.html">changes to trusted certificates</a>.</p>
<p>Basically what is needed in order to see https hits in Charles and alike is a bit more configuration than just adding SSL certificate to the phone. You will need to add in your manifest application:</p>
<pre><code>android:networkSecurityConfig="@xml/network_security_config"
</code></pre>
<p>And create a file named network_security_config.xml under the res/xml folder which should contain:</p>
<pre><code>&lt;network-security-config&gt;
    &lt;base-config&gt;
      &lt;trust-anchors&gt;
          &lt;certificates src="system" /&gt;
          &lt;certificates src="user" /&gt;
      &lt;/trust-anchors&gt;
    &lt;/base-config&gt;
&lt;/network-security-config&gt;
</code></pre>
<p>With this, you should be set!</p>
<h2 id="common-errors">Common errors</h2>
<div class="warning"></div>

<blockquote>
<ul>
<li>Make sure you have the latest version.</li>
<li>Enable the debug logs if you have any doubt.</li>
<li>Check if TCServerSide is called when you think it should be. You should see it in the console logs or inside the monitoring interface.</li>
<li>Make sure a second time that you have the latest version. (this really is the most common issue)</li>
<li>Check all your IDs</li>
</ul>
</blockquote>
<h1 id="helpers">Helpers</h1>
<h2 id="persisting-variables">Persisting variables</h2>
<p>The ServerSide module can help storing variables that remain the same in the whole application, such as vendors ID, in a TCServerSide instance, instead of passing them to the events instance each time you want to execute an event.</p>
<p>Those variables will be passed in all events as additional parameters and will persist for the whole run of the application.</p>
<pre><code>TCS.addPermanentData("VENDOR_ID", "UE-55668779-01");
</code></pre>
<p>They can also be removed if necessary.</p>
<pre><code>TCS.removePermanentData("VENDOR_ID");
</code></pre>
<h1 id="kotlin">Kotlin</h1>
<p>If you want to use Kotlin as your main language, there is absolutely nothing special to do.
Compile with the latest versions and call our module as usual.</p>
<h1 id="example-tcdemo">Example: TCDemo</h1>
<p>To check an example of how to use this module, please check: </p>
<p><a href="https://github.com/TagCommander/Tag-Demo/tree/master/Android">Tag Demo</a></p>
<h1 id="migration-v4-to-v5">Migration v4 to v5</h1>
<h2 id="why-a-new-version-of-the-sdk">Why a new version of the SDK</h2>
<p>CommandersAct made a big move forward to bring all his products together in a whole new platform.</p>
<p>As the mobile counterpart of all products we needed to re-work our SDKs in the same manner and create more logical connections with the whole suit.</p>
<p>We have renamed some modules to this end. SDK is now named ServerSide as it is only used to send information to our platform.
And TCPrivacy has been renamed to Consent since it is the name of our product inside our suit. And it is used to gather consent.</p>
<h2 id="event-based">Event based</h2>
<p>The biggest change as a user of the mobile SDK will to the way you send information to our servers.</p>
<p>Before you would create a big blob of data, fill it with anything needed or not even needed and send this.
We would then filter on the server-side this data and try to fill the tags with relevant information.</p>
<p>But as you may know, the previous server-side wouldn't allow much possibilities other than transferring the data.
With the new server-side you can rework your data in our interfaces and have more control over the data used by your solutions.</p>
<p>With this new version you will have to send "events".</p>
<p>An Event is a logical entity used by your other solutions (also named destinations) in a form that they can treat directly.
If you are using Facebook Conversion, you know that you can send "purchase" events for example which will be treated by our server-side to fit exactly what is needed by Facebook in this case.
This allow to be more precise and thus have less testing on both sides to know if what you send is indeed correctly used by your solution.</p>
<p>All custom events are defined on our online documentation including all parameters needed, all possible and their required formats.
Of course while using the ServerSide module, you can also check directly each event classes.</p>
<p>The hard part should not be for developers but for consulting which should re-organise all information currently sent in events.</p>
<h2 id="changes">Changes</h2>
<p>Many classes have been renamed, hopefully you'll only need 2 or 3 of them in your implementation.</p>
<p>Most notably: (module.classname)</p>
<pre><code>SDK.TagCommander -&gt; ServerSide.TCServerSide
privacy.TCPrivacy -&gt; consent.TCConsent
privacy.TCPrivacyAPI -&gt; consent.TCConsentAPI
privacy.TCPrivacyCenter -&gt; consent.TCPrivacyCenter
</code></pre>
<p>You don't need container ID anymore, all is on the same siteID. But you'll need a key specific to define the source.</p>
<p>You don't need to put any TCServerSide instance in your Consent implementation anymore.</p>
<p>You might need to use the TCUser class to forward relevant information about your user.</p>
<h2 id="example">Example</h2>
<pre><code>// Only sourceKey is new here, it's available on the platform and can be used to disable specific sources.
int TC_SITE_ID = 29; // defines this site account ID
int TC_PRIVACY_ID = 6; // defines this container ID
String sourceKey = "NJtcKaoCYuZEFEzDSGZDxRgMBMUw==";

TCS = new TCServerSide(TC_SITE_ID, sourceKey, context, ETCConsentBehaviour.PB_DEFAULT_BEHAVIOUR);
TCConsent.getInstance().setSiteIDPrivacyIDAppContext(TC_SITE_ID, TC_PRIVACY_ID, context);

// You can set in stone some information about your user and that will be sent with each events.
TCUser.getInstance().email = "superUser@gmal.coum";

// Here an example of a purchase event with the item purchased.
ArrayList&lt;TCItem&gt; items = new ArrayList&lt;&gt;();
items.add(new TCItem("iID1", new TCProduct("pID1", "some product", 1.5f), 1));
TCPurchaseEvent event = new TCPurchaseEvent("ID", 11.2f, 4.5f, "EUR", "purchase", "creditCard", "waiting", items);
TCS.execute(event);
</code></pre>
<p>And that's it!
Support and contacts
====================
<img alt="alt tag" src="../res/ca_logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@commandersact.com</em></p>
<p>http://www.commandersact.com</p>
<hr />
<p>This documentation was generated on 11/10/2022 14:55:46</p>
</body>
</html>