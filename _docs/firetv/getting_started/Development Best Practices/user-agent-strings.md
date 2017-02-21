---
title: User Agent Strings for Fire TV
permalink: user-agent-strings.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---


The Fire TV platform includes the Android WebView ([`android.webkit.WebView`][1]), the Amazon WebView ([`com.amazon.android.webkit.AmazonWebView`][2]), and the Amazon web app platform. Each has an associated user agent string.

An app or web page can read the user agent string to detect Fire TV and then provide a specific user experience. User agent strings can include the version of the host operating system, the version of the browser, and other information.

{% include note.html content="The Fire TV platform does not include a browser." %}

{% include tip.html content="If you're trying to identify different Fire TV devices, see [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices]." %}

* TOC
{:toc}

## User Agent Strings

When reading the user agent string, do not rely on specific version numbers within the string that are subject to change when the software is updated. To provide a Fire TV-platform specific experience, test for the string "AmazonWebAppPlatform" in combination with a device model that starts with "AFT".

The following table shows the user agent strings for Fire TV:

<table class="grid">
  <thead>
    <tr>
      <th>User Agent</th>
      <th>String</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Android WebView<br />(<code>android.webkit.WebView</code>)</td>
      <td><code>Mozilla/5.0 (Linux; U; Android &lt;android&gt;; &lt;locale&gt;; &lt;device&gt; Build/&lt;build&gt;) AppleWebKit/&lt;webkit&gt; (KHTML, like Gecko) Version/4.0 Mobile Safari/&lt;safari&gt;</code></td>
      <td><code>Mozilla/5.0 (Linux; U; Android 4.2.2; en-us; AFTB Build/JDQ39) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30</code></td>
    </tr>
    <tr>
      <td>Amazon WebView<br />(<code>com.amazon.android.webkit.AmazonWebView</code>)</td>
      <td><code>Mozilla/5.0 (Linux; Android &lt;android&gt;; &lt;device&gt; Build/&lt;build&gt;) AppleWebKit/&lt;webkit&gt; (KHTML, like Gecko) Chrome/&lt;chrome&gt; Mobile Safari/&lt;safari&gt;</code></td>
      <td><code>Mozilla/5.0 (Linux; Android 4.2.2; AFTB Build/JDQ39) AppleWebKit/537.22 (KHTML, like Gecko) Chrome/25.0.1364.173 Mobile Safari/537.22</code></td>
    </tr>
    <tr>
      <td>Amazon Web App Platform</td>
      <td><code>Mozilla/5.0 (Linux; Android &lt;android&gt;; &lt;device&gt; Build/&lt;build&gt;) AppleWebKit/&lt;webkit&gt; (KHTML, like Gecko) Chrome/&lt;chrome&gt; Mobile Safari/&lt;safari&gt; cordova-amazon-fireos/&lt;amazon&gt; AmazonWebAppPlatform/&lt;amazon&gt;</code></td>
      <td><code>Mozilla/5.0 (Linux; Android 4.2.2; AFTB Build/JDQ39) AppleWebKit/537.22 (KHTML, like Gecko) Chrome/25.0.1364.173 Mobile Safari/537.22 cordova-amazon-fireos/3.4.0 AmazonWebAppPlatform/3.4.0;2.0</code></td>
    </tr>
  </tbody>
</table>
## Placeholders in User Agent Strings

The following placeholders in the user agent string are for version numbers that vary by device, for values that can be altered by the user, or for values that can change when Amazon updates the software on the device:

*   `<android>` indicates the Android version number, for example, 4.2.2.
*   `<locale>` indicates the chosen language and country or region for the phone. The value consists of the lowercase hyphenated concatenation of the two-letter ISO 639-1 language code and the two-letter ISO 3166-1 alpha-2 country code, for example, en-us.
*   `<device>` is the value of `android.os.Build.MODEL`, for example, ATFB. Test for a device that starts with "AFT" to cover all devices on the Fire TV platform.
*   `<build>` is the value of `android.os.Build.ID`, for example, JDQ39.
*   `<webkit>`, `<chrome>`, and `<safari>` indicate the version numbers for WebKit, Chrome, and Safari, for example, 534.30.
*   `<amazon>` indicates the version number of the Amazon web app platform, for example, 3.4.0.


[1]: http://developer.android.com/reference/android/webkit/WebView.html
[2]: https://developer.amazon.com/public/solutions/platforms/android-fireos/docs/understanding-hybrid-apps

{% include links.html %}
