---
title: Customizing Your Fire TV Web App
permalink: customizing-your-fire-tv-web-app.html
sidebar: firetv
product: Fire TV 
github: true
---

You can customize your web app for Fire TV in the following ways:

*  You can code for device-specific appearance or behavior and have your app detect the device on which it is running.
*  You can customize navigation and the way focus is indicated.

In addition, you can scale up an app developed for a 720p display so it takes full advantage of Fire TV's 1080p display.

* TOC
{:toc}

## Providing a Device-Specific Experience

An app or web page can read the user agent string to detect a specific platform and then provide a specific user experience. User agent strings can include the version of the host operating system, the version of the browser, and other information. The user agent strings for the web app platform on FireTV are nearly identical to those on Fire tablets, but with a differing device model identifier. Also see the following sources of information:

*  For a list of non – Fire TV user agent strings, see the "Web App Development" section of [Amazon Web Apps Frequently Asked Questions](https://developer.amazon.com/public/solutions/platforms/webapps/faq).
*  For the rules for the device model and how to detect current and future Fire TV devices, see [Identifying Fire TV Devices][identifying-amazon-fire-tv-devices].   

## Using a CSS to Customize the Appearance of Your Web App

It is important that focused items on a page are styled in a way which clearly signifies that a subsequent press on the center button would select it. The default selection indicators (yellow border and/or blue background) are generally not recommended and should be customized on a per-application basis.  To define this styling, developers should use the CSS `focus` property.

```css
button:focus {
    border : #ffffff 2px solid;
    outline : 0;
}
```

Some elements when focused will show a blue highlight background. The following CSS property should remove it; however, note that any element this is applied to will have a transparent background which isn't always desired (input text field, for example).

```css
*:focus {
    outline:none;
    background-color:rgba(0,0,0,0);
}
```

For information on the design guidelines for Fire TV web apps, see [Design and User Experience Guidelines][design-and-user-experience-guidelines].

## Customizing Focus and Navigation in Your Web App

If you want your web app to handle the selection highlighting or directional navigation on its own, simply capture the key event and consume it. For a working example of capturing key events and customizing navigation, see the template app in the [Web App Starter Kit for Fire TV][the-web-app-starter-kit-for-fire-tv], which is available on GItHub at [https://github.com/amzn/web-app-starter-kit-for-fire-tv](https://github.com/amzn/web-app-starter-kit-for-fire-tv).

## Displaying an App Developed in a 720p Environment

The resolution for Fire TV apps is1080p (1920x1080). If your app was developed for a 720p interface, it fills only 2/3 of the display. In this case, the best solution is to alter your app to target 1080p. However, if you want to simulate a 720p environment, you can do so by putting the following meta tag in the header of your page. 

```xml
<meta name=”viewport” content=”initial-scale=1.5, user-scalable=no”>
```

This meta tag sets a viewport, representing the area the web app page occupies, and sets a zoom level of 150%. The result is that the web app content looks like it would on a 720p display but fills the 1080p display area.

For more information, see "Resolution and Page Scaling section" in [Getting Started with Web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv].

{% include links.html %}
