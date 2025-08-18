
<html>
<body>
<p><img alt="alt tag" src="./res/ca_logo.png" /></p>
<h1 id="firebase-destination-implementation-guide">Firebase Destination Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>18/08/2025</em><br />
Release version : <em>5.1.3</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#firebase-destination-implementation-guide">Firebase Destination Implementation Guide</a></li>
<li><a href="#introduction">Introduction :</a></li>
<li><a href="#setup">Setup :</a></li>
<li><a href="#usage">Usage :</a><ul>
<li><a href="#supported-firebase-event">Supported Firebase Event</a></li>
</ul>
</li>
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
<h2 id="supported-firebase-event">Supported Firebase Event</h2>
<p>We highly recommend only using TCCustomEvent when forwarding events to firebase. 
Make sure your events are compatible with firebase specifications to prevent any errors.</p>
<p>code example in kotlin : </p>
<p>```      <br />
        val item1 = JSONObject().apply {
            put("item_id",      "1234")
            put("item_name",    "XWU-1")
            put("item_category","football")
            put("item_variant", "blue")
        }</p>
<pre><code>    val item2 = JSONObject().apply {
        put(Param.ITEM_ID,   "5678")   // Firebase constants still work
        put(Param.ITEM_NAME, "ZPA-13")
        put("item_category",            "basketball")
        put("item_variant",             "orange")
    }

    val items: MutableList&lt;JSONObject&gt; = mutableListOf(item1, item2)

    val addToCartEvent = TCCustomEvent("add_to_cart").apply {
        addAdditionalProperty("currency", "USD")
        addAdditionalProperty("value",   30)          
        addAdditionalProperty("items",   items)      
        addAdditionalProperty("item_variant", "1234")
        addAdditionalProperty("price", 1234)       
    }

    tc?.execute(addToCartEvent)
</code></pre>
<p>```</p>
<p>Specs and requirements differ between TCEvents and Firebase Events, if you still want to use our TCEvents, you'll need to make sure that your TCEvents match Firebase recommendations too (required, allowed and non authorized parameters)
Events will be mapped like the following, and the TCServerSide will try and log the event to firebase.
You'll also need to configure every new parameter in your firebase console</p>
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
<td>event.items[i].X</td>
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