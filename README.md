
<html>
<body>
<p><img alt="alt tag" src="res/ca_logo.png" /></p>
<h1 id="developers-implementation-guide">Developers' Implementation Guide</h1>
<p><strong>Android</strong></p>
<p>Last update : <em>22/03/2023</em><br />
Release version : <em>5</em></p>
<p><div id="end_first_page" /></p>

<div class="toc">
<ul>
<li><a href="#developers-implementation-guide">Developers' Implementation Guide</a></li>
<li><a href="#introduction">Introduction</a></li>
<li><a href="#latest-available-versions">Latest available versions</a></li>
<li><a href="#adding-a-module-to-your-project">Adding a module to your project</a><ul>
<li><a href="#mavencentral">mavenCentral</a></li>
<li><a href="#jar-file">Jar file</a></li>
<li><a href="#aar-file">Aar file</a></li>
</ul>
</li>
<li><a href="#support-and-contacts">Support and contacts</a></li>
</ul>
</div>
<h1 id="introduction">Introduction</h1>
<p>TagCommander for mobile is a collection of small SDKs each designed to serve a dedicated purpose.
The modules are the following :</p>
<p><a href="TCCore/README.md">Core : Used as a base by the other modules.</a></p>
<p><a href="TCServerSide/README.md">ServerSide : Tag management system collecting data through a server-side approach.</a></p>
<p><a href="TCSegment/README.md">Segment : Get your user segmentation from our servers.</a></p>
<p><a href="TCConsent/README.md">Consent : Pass the Consent settings to our tag system</a></p>
<p>For each of those modules, please check their respective documentation for more information.</p>
<h1 id="latest-available-versions">Latest available versions</h1>
<p>Core : <em>5.3.2</em></p>
<p>ServerSide : <em>5.4.0</em></p>
<p>Consent : <em>5.2.1</em></p>
<p>IAB : <em>5.0.0</em></p>
<p>Partners : <em>5.0.0</em></p>
<p>Segment : <em>5.0.0</em></p>
<h1 id="adding-a-module-to-your-project">Adding a module to your project</h1>
<p>If you want to add a module to your android project, you have several possibilities.</p>
<pre><code>- Using mavenCentral to manage the dependency.
- Using directly the jar in your project.
- Using the aar file in your project.
</code></pre>
<p>All of them will require you to modify a bit your build.gradle.</p>
<h2 id="mavencentral">mavenCentral</h2>
<p>The easiest way is to go with mavenCentral. It will help you get updates on the module on a regular basis without doing much work.</p>
<p>/!\ The IAB module is not on mavenCentral yet, so it will need to be added manually with the aar or jar.</p>
<p>If it's not present in your project's build.gradle add mavenCentral() in the repository list for the dependency management. It will look something like that:</p>
<pre><code>allprojects {
    repositories {
        mavenCentral()
    }
}
</code></pre>
<p>Then in your application's build.gradle always add the core module:</p>
<pre><code>implementation 'com.tagcommander.lib:core:5.3.2'
</code></pre>
<p>And in addition to the core module you can add the other modules you need the same way. See each module's documentation for more specific information.</p>
<p>For example:</p>
<pre><code>implementation 'com.tagcommander.lib:ServerSide:5.4.0'
implementation 'com.tagcommander.lib:segment:5.0.0'
</code></pre>
<h2 id="jar-file">Jar file</h2>
<p>If you'd rather use the jar files directly in your project, you can get them from our github account: https://github.com/CommandersAct/AndroidV5</p>
<div class="warning"></div>

<blockquote>
<p>You will always need to at least add the Core module to your project.</p>
</blockquote>
<p>After you downloaded the modules you need, add them to your libs folder and either ask gradle to compile with all the jars in your lib directory or directly with the chosen files.</p>
<pre><code>// All the jars.
compile fileTree(dir: 'libs', include: '*.jar')
// Specific files
compile files('libs/TCCore-release-5.3.2.jar')
compile files('libs/TCServerSide-release-5.4.0.jar')
compile files('libs/TCSegment-release-5.0.0.jar')
compile files('libs/TCConsent-release-5.2.1.jar')
</code></pre>
<h2 id="aar-file">Aar file</h2>
<p>If you'd rather use the aar files directly in your project, you can get them from our github account: https://github.com/CommandersAct/AndroidV5</p>
<div class="warning"></div>

<blockquote>
<p>You will always need to at least add the Core module to your project.</p>
</blockquote>
<p>To be able to compile with the aar files, you will first need to tell gradle how to use them properly. In your project's build.gradle complete your repository list with 'flatDir' list in the following exemple:</p>
<pre><code>allprojects {
    repositories {
        mavenCentral()
        flatDir {
            dirs 'libs'
        }
    }
}
</code></pre>
<p>After you downloaded the modules you need, add them to your libs folder and ask gradle to compile with them.</p>
<pre><code>compile (name:'TCCore-release-5.3.2', ext:'aar')
compile (name:'TCServerSide-release-5.4.0', ext:'aar')
compile (name:'TCSegment-release-5.0.0', ext:'aar')
compile (name:'TCConsent-release-5.2.1', ext:'aar')
</code></pre>
<h1 id="support-and-contacts">Support and contacts</h1>
<p><img alt="alt tag" src="../res/ca_logo.png" /></p>
<hr />
<p><strong>Support</strong>
<em>support@commandersact.com</em></p>
<p>http://www.commandersact.com</p>
<p>Commanders Act | 7b rue taylor - 75010 PARIS - France</p>
<hr />
<p>This documentation was generated on 22/03/2023 15:12:32</p>
</body>
</html>