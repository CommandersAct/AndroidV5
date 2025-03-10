
<html>
<body>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<h1 id="core-guide">Core Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>10/03/2025</em><br />
Release version : <em>5.4.7</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#core-guide">Core Guide</a></li>
<li><a href="#introduction">Introduction</a></li>
<li><a href="#dependencies">Dependencies</a><ul>
<li><a href="#using-proguard">Using ProGuard</a></li>
</ul>
</li>
<li><a href="#support-and-contacts">Support and contacts</a></li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>As we expended Commanders Act's mobile possibilities, it rapidly became apparent that we could not fit everything at the same time for everybody in only one big library. In fact, it's contradictory with Tag Management itself, so we decided to cut capabilities into smaller modules so people wouldn't need to add everything in their project, saving space in your application.</p>
<p>But even with that said, a part of our code is pretty useful in several of our modules, so we created a Core module to prevent code repetition and thus also bigger applications if you need several modules.</p>
<h1 id="dependencies">Dependencies</h1>
<p>The Core module is mandatory if you are using Commanders Act's mobile solution, so we simply put the dependencies needed for the Core module directly in the documentations of the other modules.</p>
<p>Core is building with the following dependencies :</p>
<pre><code>implementation 'androidx.appcompat:appcompat:1.4.1'
</code></pre>
<h2 id="using-proguard">Using ProGuard</h2>
<p>If your release build uses ProGuard to strip and obfuscate your code, you will need to add some configuration for the modules to keep working.</p>
<p>Just add the following line in your proguard-rules.pro.</p>
<pre><code>-keep class com.tagcommander.lib.** { *; }
</code></pre>
<p>You may also need to add the following rule depending on your other libraries dependencies</p>
<pre><code>-keep class org.json.** { *; }
</code></pre>
<h1 id="support-and-contacts">Support and contacts</h1>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@commandersact.com</em></p>
<p>http://www.commandersact.com</p>
<hr />
<p>This documentation was generated on 10/03/2025 15:41:21</p>
</body>
</html>