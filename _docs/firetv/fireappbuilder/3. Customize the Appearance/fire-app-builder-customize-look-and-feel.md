---
title: Customize the Look and Feel of Your App
permalink: fire-app-builder-customize-look-and-feel.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

You can customize much of the app's look and feel through your app's custom.xml file (located in **res > values**) as well as navigator.json (in **app > assets**). Here's a list of what you can easily change in the app:

* Fonts
* Splash screen
* Recommended content
* Background colors
* Content cards
* Buttons
* Search bar
* Content reload times
* Homepage layout
* App icon
* Terms of use

* TOC
{:toc}

## Change the Font

You can change your app's fonts through the `branding` object in Navigator.json (inside **app > assets**):

```json
"branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "Roboto Light",
    "boldFont" : "Roboto Bold",
    "regularFont" : "Roboto Regular"
  }
```

Here's how the fonts are used:

| Property | Where It's Used |
|----|-----|
| `lightFont` | Used in description and body text. |
| `boldFont` | Used in titles. |
| `regularFont` | Used in buttons and subtitles.|

(With the `globalTheme` property, there aren't other values you can select.)

You can assign any valid device or custom font for any of the three font options. For example, you could apply `Roboto Bold` to all three fonts if you want.

You can use any of these device fonts:

* **Amazon Ember fonts**: Amazon Ember, Amazon Ember Bold, Amazon Ember Bold Italic, Amazon Ember Italic, Amazon Ember Light, Amazon Ember Light Italic, Amazon Ember Medium, Amazon Ember Medium Italic, Amazon Ember Thin, Amazon Ember Thin Italic, AndroidClock Regular, AndroidClock-Large Regular
* **Roboto fonts**: Roboto Black, Roboto Black Italic, Roboto Bold, Roboto Bold Italic, Roboto Condensed Bold, Roboto Condensed Bold Italic, Roboto Condensed Italic, Roboto Condensed Light, Roboto Condensed Light Italic, Roboto Condensed Regular, Roboto Italic, Roboto Light, Roboto Light Italic, Roboto Medium, Roboto Medium Italic, Roboto Regular, Roboto Thin, Roboto Thin Italic
* **Verdana fonts**: Verdana, Verdana Bold, Verdana Bold Italic, Verdana Italic
* **Miscelleneous fonts**: Carrois Gothic SC, Clockopia, Code2000, Coming Soon, Cutive Mono, Dancing Script, Dancing Script Bold
Droid Sans Mono, Kindle Symbol, MotoyaLMaru W3 mono, MT Chinese Surrogates, NanumGothic, Source Code Pro Medium

For example, if you wanted to use Amazon Ember fonts, you could customize the `branding` object like this:

```json
  "branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "Amazon Ember",
    "boldFont" : "Amazon Ember Bold",
    "regularFont" : "Amazon Ember"
  }
```

The screens would then look like this:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fontsnewhome" type="png" %}

And this:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fontsnewdetails" type="png" %}

These Amazon Ember fonts will be used in the titles, subtitles, descriptions, body, and buttons.

Beyond using different device fonts, you can also use custom fonts. If you use a custom font, store the font in your app's **assets/fonts** directory. Then provide the path to the font in the `branding` object, such as `fonts/Proxima-Nova-Light.tff` (instead of just entering `Proxima Nova Light`):

```json
  "branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "fonts/Proxima-Nova-Light.tff",
    "boldFont" : "fonts/Proxima-Nova-Light.tff",
    "regularFont" : "fonts/Proxima-Nova-Light.tff"
  }
```

## Customize the Splash Screen

By default, the sample app in Fire App Builder shows the following Splash screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_splashdiagram" type="svg" alt="Splash screen" %}

You can adjust the Splash screen settings in the custom.xml file:

```xml
<!-- Splash Screen Customization -->

<!-- Background to display on Splash Screen -->
<drawable name="splash_background">@drawable/bg_generic_nopreview</drawable>
<!-- Company logo to show on Splash Screen -->
<drawable name="splash_logo">@drawable/fire_app_builder_white</drawable>
<!-- Copyright string text color -->
<color name="copyright">#E6FFFFFF</color>

<!-- End of Splash Screen Customization -->
```

The references to `@drawable` refer to images in **TVUIComponent > res > drawable**. Either replace these images with your own customized images, or add unique image files and update the references in the XML to point to your images.

The `bg_generic_nopreview` image is just a 1900 x 1080px black image. This same background is used for the fullbrowse homepage layout, but you can specify a different image reference here if you want.

The `splash_logo` should contain your logo on a transparent background. The logo image size in the sample application is 356 x 108px, but you can use a larger image if you want. The image will be scaled down automatically.

Your logo appears as an overlay on top of the splash screen's background image, so if you're customizing the images, make sure the combination looks good. A light logo on a dark background will have the necessary contrast.

To customize the Copyright text on the Splash screen, see the **strings.xml** folder inside your app's **res > values > strings.xml** folder.

```xml
<!-- Copyright string -->
<string name="copyright">Copyright 2016. All Rights Reserved.</string>
```

## Customize the Recommended Content Section {#recommendations}

On the Content Details screen, a list of "Recommended Content" appears below the content preview.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_recommendedcontentdiagram" type="svg" alt="Recommended content" %}

For more details on configuring Recommended Content, see ["Recommended Content (Through Tags)"][fire-app-builder-set-up-recipes-content#tags] in Set Up the Content Recipe.

## Customize the Homepage Layout

The default home screen layout uses the `ContentBrowseActivity`. This layout is referred to as the "Homepage Browse layout."

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="Home" caption="<b>Home with the <code>ContentBrowseActivity</code></b>. This view arranges the videos in various channels or groups. When you view a channel, the first video in that channel group appears as the featured background image, with its title and description in the upper-left." %}

You can change the homepage layout to a more compressed view by using the `FullContentBrowseActivity` instead. This alternative homepage layout is referred to as the "Homepage Full Browse layout."

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fullbrowse" type="png" alt="Home with FullContentBrowseActivity" caption="<b>Home with <code>FullContentBrowseActivity</code></b>. With this activity, all the videos appear in a more compressed grid, with the channels listed on the left. None of the videos are superimposed as large featured images in the background." %}

The left sidebar can collapse in when the user is browsing through the video titles. This gives more space and focus to your video content.

To change the homepage to the more compressed, Full Browse layout:

1.  Open the **Navigator.json** file (located in **app > assets**).
2.  In the `graph` object, locate the **CONTENT_HOME_SCREEN**:

    ```json
    "com.amazon.android.tv.tenfoot.ui.activities.ContentBrowseActivity": {
          "verifyScreenAccess": false,
          "verifyNetworkConnection": true,
          "onAction": "CONTENT_HOME_SCREEN"
        }
    ```

3.  Change `ContentBrowseActivity` to `FullContentBrowseActivity`.

The following diagram shows the properties you can customize on the homepage's Browse layout (the default):

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_homepagediagram" type="svg" %}

The <code>browse_background</code> property is set through the TVUIComponent's custom.xml file and references an image rather than a hex value.

The following diagram shows the properties for the homepage's Full Browse layout:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_althomepagediagram" type="svg" %}

The following code in your app's custom.xml shows where these homepage options are set:

```xml
    <!-- Browse Customization -->

    <!-- The background gradient color, if no background drawable is provided.
         Used in default_background.xml -->
    <color name="background_gradient_start">#000000</color>
    <color name="background_gradient_end">#DDDDDD</color>
    <!-- The header's bar color (left-hand navigation bar) -->
    <color name="browse_headers_bar">#0DFFFFFF</color>
    <!-- Selected header text color -->
    <color name="browse_header_selected">#FFFFFFFF</color>
    <!-- Non-selected header text color -->
    <color name="browse_header">#CCD8D8D8</color>
    <!-- Header color when left-hand navigation bar is closed
         and rows are showing full screen -->
    <color name="browse_row_header">#99FFFFFF</color>
    <!-- Search orb color (the circle shape around the search icon) -->
    <color name="search_orb">#EE962D</color>
    <!-- Header text size -->
    <dimen name="browse_header_text">16sp</dimen>
    <!-- Company logo -->
    <drawable name="company_logo">@drawable/fire_app_builder_white_2</drawable>

    <drawable name="browse_bg_color">@drawable/bg_generic_nopreview</drawable>
    <!-- End of Browse Customization -->
```
{% include note.html content="Disregard the comments in the Browse Customization section in the application's custom.xml file above. Some of the comments are out of date, and some of the elements aren't properly configured. See the following table to understand what each element does." %}

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="60%" />
   </colgroup>
   <thead>
      <tr>
         <th>Element</th>
         <th>Description</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><code>background_gradient_start</code></td>
         <td>Does nothing. Ignore.</td>
      </tr>
      <tr>
         <td><code>background_gradient_end</code></td>
         <td>Does nothing. Ignore.</td>
      </tr>
      <tr>
         <td><code>browse_headers_bar</code></td>
         <td>Changes the color of the left navigation bar when you're in the compressed homepage layout.</td>
      </tr>
      <tr>
         <td><code>browse_header_selected</code></td>
         <td>Does nothing. Ignore.</td>
      </tr>
      <tr>
         <td><code>browse_header</code></td>
         <td>Does nothing. Ignore.</td>
      </tr>
      <tr>
         <td><code>browse_row_header</code></td>
         <td>For the homepage Browse layout, changes the category title above the videos rows. For the Full Browse layout, changes the color of the category titles in the left navigation bar. Both the selected and non-selected category titles receive this color. The selected category titles are bold, and the non-selected category titles are softer and muted.</td>
      </tr>
      <tr>
         <td><code>search_orb</code></td>
         <td>Does nothing. Ignore.</td>
      </tr>
      <tr>
         <td><code>browse_header_text</code></td>
         <td>Changes the size of the category titles.</td>
      </tr>
      <tr>
         <td><code>company_logo</code></td>
         <td>This image is stored in <b>TVUIComponent > res > drawable</b>. Replace the image with your own logo. The image should be transparent and at least 356px wide by 108px tall. You can use a larger image &mdash; it will be scaled down in the app.</td>
      </tr>
      <tr>
         <td><code>browse_bg_color</code></td>
         <td>The background image used for both the Full Browse layout and the Splash screen. This image is stored in <b>TVUIComponent > res > drawable</b>. The image is a 1900 x 1080px PNG image with a black color.</td>
      </tr>
      <tr>
         <td><code>browse_background</code></td>
         <td>This element isn't included in application's custom.xml file &mdash; Ignore. You can simply add the line. This element controls the background image used for the homepage <i>Browse</i> layout. More details about this image are listed below.</td>
      </tr>
      <tr>
         <td><code>browse_bg_color</code></td>
         <td>This element isn't included in application's custom.xml file &mdash; Ignore. You can simply add the line. This element controls the background image used for the homepage <i>Full Browse</i> layout. More details about this image are listed below.</td>
      </tr>
   </tbody>
</table>

### Background Image for Homepage Browse Layout

The background color for the homepage Browse layout (the default) isn't configurable through the custom.xml values above because there's a line missing in custom.xml:

```
    <drawable name="browse_background">@drawable/bg_generic</drawable>
```

You can add this line to your application's custom.xml file and control your background image file used here. Alternatively, you can edit the custom.xml inside **TVUIComponent > res > values > custom.xml**. (The custom.xml file in your app just overwrites the other values in XML files, so setting the property in either file will work. But as a best practice, leave the component files unedited and just edit your application's config.xml file.)

Like the `browse_bg_color` element, the drawable image referenced here is stored in **TVUIComponent > res > drawable** (assuming you're browsing in the Android view).

The bg_generic.png image looks like this:

{% include image.html url="https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/fireappbuilder/bg_generic.png" file="firetv/fireappbuilder/images/bg_generic" type="png" caption="<b>bg_generic.png</b>. The image is a 1900 x 1080px PNG image with transparency in the upper-right area." max-width="80%" %}

To change this default image, create your own image file with the same dimensions. Make the upper-right corner transparent, similar to the existing image. Name it **bg_generic.png** and replace the existing file in **TVUIComponent > res > drawable**.

To make it easy to customize the image, you can [download the Photoshop file](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/fireappbuilder/bg_generic.psd._V523835574_.zip) used to create the image. In Photoshop, view the image's layers and use the Paint Bucket tool to apply a new color to the two layers. In the downloadable file, the color has been changed from black to blue to make the colors more apparent.

For the Full Browse layout, you change this image in a similar way &mdash; by replacing the bg_generic_nopreview.png in **TVUIComponent > res > drawable** or by creating a unique image file and updating the reference to it in your application's custom.xml file.

## Customize the Content Cards

You can customize the background colors and dimensions of the content cards. The "cards" refers to the rectangular thumbnails that show the media details. For example, the Home screen displays a row of content cards under the "Jamaican Attractions" title.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_moviecardsdiagram" type="svg" alt="Fire App Builder cards home" %}

The following code shows the available settings to customize the appearance of these cards:

```xml
<!-- Movie Info Card Customization -->

<!-- Background color of the card info -->
<color name="card_info_bg">#2B2B2B</color>
<!-- Card info title text color -->
<color name="card_info_title_text">#E6FFFFFF</color>
<!-- Card info content text color -->
<color name="card_info_content_text">#FFFFFFFF</color>
<dimen name="card_info_title_text">8sp</dimen>
<!-- Card info content text size -->
<dimen name="card_info_content_text">12sp</dimen>
<!-- Card info selected title text size -->
<dimen name="card_info_selected_title_text">10sp</dimen>
<!-- Card info selected content text size -->
<dimen name="card_info_selected_content_text">15sp</dimen>

<!-- End of Movie Info Card Customization -->
```

## Customize the Content Details

You can customize various elements on the Content Details screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentdetailsdiagram" type="svg" %}

The relevant code in the custom.xml file is as follows:

```xml
<!-- Details Customization -->

<!-- Text color for unfocused action button -->
<color name="action_button_text_color">#E6FFFFFF</color>
<!-- Text color for focused action button -->
<color name="action_button_text_color_focused">#E6FFFFFF</color>
<!-- Background color for focused action button -->
<drawable name="action_button_focused">@drawable/btn_generic_focused</drawable>
<!-- Background color for normal state of action button -->
<drawable name="action_button_normal">@drawable/btn_normal</drawable>
<!-- Details description title text color -->
<color name="details_description_title">#E6FFFFFF</color>
<!-- Details description body text color -->
<color name="details_description_body">#FFFFFFFF</color>
<!-- Action button text size -->
<dimen name="action_text">14sp</dimen>
<!-- Details description title text size -->
<dimen name="details_description_title_text">24sp</dimen>
<!-- Details description body text size -->
<dimen name="details_description_body_text">16sp</dimen>

<!-- End of Details Customization -->
```

## Customize the Playback Overlay

You can customize the playback overlay on the Content Renderer screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrendererdiagram" type="svg" alt="Content renderer screen" %}

```xml
<!-- Playback Overlay Customization -->

<!-- Background color of the playback overlay -->
<color name="playback_background">#9922262A</color>
<!-- Background color of the progress bar -->
<color name="playback_background_progress_bar">#FF373737</color>
<!-- Color of buffered progress bar -->
<color name="playback_buffered_progress">#FF5A5A5A</color>
<!-- Color of the progress bar -->
<color name="progress_bar">#FFDADADA</color>
<!-- Text color of the playback time -->
<color name="playback_time_text">#FFFFFFFF</color>
<!-- Hide More options status activation status-->
<bool name="hide_more_options">true</bool>
<!-- End of Playback Overlay Customization -->
```

## Customize the Search Bar

You can customize the background color and text used in the search bar.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_searchscreendiagram" type="svg" alt="Search screen diagram" %}

The relevant code in custom.xml is as follows:

```xml

<!-- Search Bar Customization -->

<!-- Background color of search orb -->
<color name="search_orb_background">#EE962D</color>
<!-- Icon to display inside of search orb -->
<drawable name="search_icon">@drawable/ic_search</drawable>
<!-- Background color of Search screen -->
<drawable name="search_background">@drawable/bg_gradient_search</drawable>
<!-- Search hint text -->
<string name="search_bar_hint">I\'m looking for…</string>

<!-- End of Search Bar Customization -->
```

## Customize the Content Reload Time

You can customize the time it takes for content to reload (content refers to  the videos and other details that your app loads from the media feed). By default the reload time is 14400000 milliseconds, or 4 hours. After this time expires, the Navigator.js file (located in **app > assets > resources**) will reload the recipes and data loader settings.

```xml
<!-- Content Reload time in milliseconds -->

<integer name="time_to_reload_content">14400000</integer>

<!-- End Content Reload time in milliseconds -->
```

## Customize the App Icon

You can change the app icon. This is the image thumbnail that appears in your list of apps on Fire TV.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_appicondiagram" type="svg" alt="App icon" %}

To update this file, change the ic_launcher.svg files. Switch to the **Project** view, and then look in **app > src > main > res**. There are 4 app icon files, each corresponding to different screen sizes:

* mipmap-hdpi (72x72px)
* mipmap-mdpi (48x48px)
* mipmap-xhdpi (96x96px)
* mipmap-xxhdpi (96x96px)

The app icon has a transparent background.

## Update the Terms of Use

The Terms of Use section appears in the footer of the app and links to the terms_of_use.html file (located in **app > assets**).

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_termsofusediagram" type="svg" alt="Terms of Use diagram" %}

The Terms of Use file is a sample file that you should edit before distributing your app.  For instance, you might choose to include terms of use, an end user license agreement, privacy notices, and/or other legal notices in this file.

The Terms of Use file also includes notices for open source components that are built in to the sample app by default. These notices are provided as a convenience only. Amazon makes no representations as to their accuracy or completeness and will not be responsible for any inaccuracies or incompleteness.

## Next Steps

To use components that leverage authentication, ads, in-app purchasing, or analytics, see [Components Overview][fire-app-builder-interfaces-and-components].

{% include links.html %}
