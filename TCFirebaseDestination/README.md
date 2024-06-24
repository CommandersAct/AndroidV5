
<html>
<body>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<h1 id="firebase-destination-implementation-guide">Firebase Destination Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>24/06/2024</em><br />
Release version : <em>5.1.1</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#firebase-destination-implementation-guide">Firebase Destination Implementation Guide</a></li>
<li><a href="#introduction">Introduction :</a></li>
<li><a href="#setup">Setup :</a></li>
<li><a href="#usage">Usage :</a></li>
<li><a href="#google-consent-mode">Google Consent Mode :</a></li>
</ul>
</div>
<h1 id="introduction">Introduction :</h1>
<p>Commanders Act's Firebase destination library for Android.
This library is for android only, please refer to <a href="https://github.com/CommandersAct/iOSV5/tree/master/TCServerSide">TCServerSide iOS documentation</a> if you want the same functionalities on iOS.</p>
<h1 id="setup">Setup :</h1>
<p>You'll need to correctly set up Firebase SDK first into your app, please refer to the official firebase documentation to do so.
Once you have your firebase SDK running and your <code>google-services.json</code> into your app bundle, you only need to add the Firebase Destination module into your app build.gradle.</p>
<p><code>implementation 'com.tagcommander.lib:FirebaseDestination:5.0.0'</code></p>
<h1 id="usage">Usage :</h1>
<p>You can set your firebase user properties directly into your firebaseAnalytics instance.</p>
<p><code>FirebaseAnalytics.setUserProperty("favorite_food", food)</code></p>
<p>You'll need to initialise the module with your application context :</p>
<p><code>TCFirebase.getInstance().initialize(appContext)</code></p>
<p>Once done, every time you execute a TCEvent, it will be remapped and sent to google.</p>
<p>All of the TCEvents properties will be re-mapped into a Firebase event holding your executed TCEvent name.</p>
<p>For TCEcommerce events (TCAddToCartEvent, ...etc), we use the following mapping :</p>
<table>
<thead>
<tr>
<th>TCEvent Property</th>
<th>Firebase Property</th>
</tr>
</thead>
<tbody>
<tr>
<td>event.items[i].id</td>
<td>event.items[i].item_id</td>
</tr>
<tr>
<td>event.items[i].X</td>
<td>event.items[i].tc_item_X</td>
</tr>
<tr>
<td>event.items[i].product.name</td>
<td>event.items[i].item_name</td>
</tr>
<tr>
<td>event.items[i].product.X</td>
<td>event.items[i].tc_product_X</td>
</tr>
</tbody>
</table>
<p>The predefined variables related to events (such as TCDevice and TCNetwork) aren't included in the Firebase event because they are already being gathered by Firebase SDK.</p>
<h1 id="google-consent-mode">Google Consent Mode :</h1>
<p>You can use our TCConsent module to automatically set and collect consents from your users. For further details, please refer to our TCConsent documentation.</p>
</body>
</html>