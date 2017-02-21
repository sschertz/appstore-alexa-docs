The VoiceView accessibility feature of Fire OS enables vision-impaired users to navigate the Fire TV user interface. With VoiceView, visually impaired users are able to use the **Up**, **Down**, **Left**, and **Right** buttons on the remote to move the input focus around the screen.

When the focus changes, VoiceView speaks the currently focused item. This functionality is similar to the "Standard Navigation" mode of the previous version of VoiceView.

This page discusses the user experience (UX) recommendations for implementing VoiceView in your app.  

* TOC
{:toc}

## Checklist for VoiceView Implementation

Use the following checklists to ensure that your Fire TV app implements the VoiceView screen reader to meet the standards recommended by Amazon for the upcoming Fire TV software release.

### Technical Implementation Recommendations

Your app should implement the following aspects of VoiceView:  

*   Verify that your app's UI is compatible with the Android Accessibility Framework.
*   Implement Content Descriptions so that VoiceView can read descriptions for images, buttons, and other on-screen objects.
*   Add Usage Hints to list and grid content so that VoiceView can help guide the interactions of vision-impaired users.
*   Implement Described By for static content, so that VoiceView can read that content when the associated object gains focus.

### User Experience (UX) Recommendations

Your app should implement the following UX recommendations for VoiceView:  

*   VoiceView reads the content for items that gain focus, including all information that is visually represented onscreen, such as a title or episode number.
*   VoiceView reads Orientation Text and Usage Hints to help users learn how to navigate the screen UI.
*   VoiceView reads static content either automatically through Described By text, manually when a user either presses the Menu button, or manually when a user enters Review Mode and steps through on-screen items.
*   VoiceView reads setup information, including activation URLs and codes.
*   VoiceView supports features and tasks such as program selection, program information, playback, and CC settings.

## Overview of VoiceView Behavior

Users can navigate the Fire TV user interface and VoiceView-enabled apps using the **Menu** button and other controls.

### Using the Menu button with VoiceView

Use the following conventions for the **Menu** button when VoiceView is enabled:  

*   To enable or disable VoiceView, press the **Back** and **Menu** buttons at the same time for two seconds.
*   When VoiceView is enabled, VoiceView controls the **Menu** button and the **Play**/**Pause** button (when VoiceView is speaking).
*   The system or app receives double-press events from the **Menu** button.  
*   Single-pressing the **Menu** button initiates the reading of information from the screen in the following order:
    1.  Usage hints
    2.  Orientation text
    3.  Described By text
    4.  All other static content

### Navigating with VoiceView

Use the following navigation conventions when VoiceView is enabled:

*   When VoiceView reads Usage hints, Orientation text, Described By text, and all other static content, the user can control navigation using the **Rewind** and **Fast Forward** buttons.
*   When a user accesses a screen for the first time, VoiceView automatically reads the Orientation text first.
*   When a user moves focus to a control after pausing, VoiceView reads Usage hints and Described by text.
*   When speaking, the **Play/Pause** button silences VoiceView.
    *   If a movie or music is playing and VoiceView is speaking, pressing **Play/Pause** one time silences VoiceView.  
    *   After VoiceView is silenced, pressing **Play/Pause** a second time pauses the playing media.
*   VoiceView only reads static content not marked as Described By text when a user presses the **Menu** button.  

## Navigating with Review Mode

VoiceView's Review Mode allows a user to explore the grid layout of a Fire TV screen in detail, similarly to using linear navigation with a screen reader on a tablet. Use the following conventions for using Review Mode in your app:

*   Press-and-hold the **Menu** button to enter Review Mode, which will be announced by VoiceView.
*   The **Left** and **Right** buttons on the directional controller control linear navigation.
*   If a user presses **Select** when a non-actionable item is in focus, VoiceView speaks "Item not selectable."
*   Press-and-hold the **Menu** button a second time to exit Review Mode.
*   After exiting Review Mode, the focus for accessibility returns to the previous cached location of keyboard focus, and VoiceView repeats an announcement of that focus.  

### Using Review Mode with WebViews

A WebView can represent a complex UI within an application, containing both actionable items, such as links, and non-actionable items, such as static text and images. VoiceView supports WebViews because they are frequently used by many apps in the Amazon Appstore. In a VoiceView-enabled WebView, a user can use Review Mode to be able to navigate through the actionable items and gauge the context of the static content items. Note that Described By content is not available in WebViews.  

### Navigating Focus Granularity Options in Review Mode

When you first enable Review Mode, the level of granularity defaults to moving by individual controls. Press **Up** or **Down** to cycle through the available granularity options, such as character, word, control, or window. Granularity options in web content include link, list, or heading. Use the following conventions to navigate the various granularity options:  

*   Press **Left** to return to the previous item at the currently selected granularity.
*   Press **Right** to move to the next item.
*   The granularity level is reset to "control” when you enter Review Mode.

## Implementing VoiceView

This section discusses guidelines for implementing VoiceView for the following types of content:

*   Static/non-focusable content
*   Orientation text
*   Usage hints

### Implementing VoiceView for Static or Non-focusable Content

VoiceView has implemented a separate navigation scheme for static or non-focusable content, which enables vision-impaired users to easily navigate this type of content using the **Menu, Fast Forward,** and **Rewind** buttons. Additionally, VoiceView supports markup that apps can use to associate static content with a selectable item.

To associate a piece or container of static content with an item:

1.  Set the key for `com.amazon.accessibility.describedBy` in the `extras` bundle from the `AccessibilityNodeInfo` for the item.  
2.  Set the value to be a string containing a space-delimited list of the view Ids of the containers or views which contain the description of this item.  

    When VoiceView encounters an item with the `com.amazon.accessibility.describedBy` key set, it will request a list of `AccessibilityNodeInfo` objects for the view Ids specified by the `com.amazon.accessibility.describedBy` value. VoiceView then reads the appropriate text or content descriptions, depending on your verbosity settings.

The following examples explain how a user would navigate static content in two different scenarios:

{% include callout.html color="blue" content="**Example 1**: Consider a Fire TV movie details view where the movie title, year, duration, star ratings, etc. are all static content. The user presses **Menu** to prompt VoiceView to speak this content. The user then navigates using the **Fast Forward** and **Rewind** buttons." %}

{% include callout.html color="blue" content="**Example 2**: Consider a Fire TV launcher screen that is used to navigate a movie catalogue. The text at the top of the screen updates to show the title, description, rating, and other information. Because this non-focusable content updates each time that the user selects a movie, the node containing the selected movie should be described by with the `describedBy` extra." %}

### Implementing VoiceView for Orientation Text

To help new users understand how Fire TV screens are laid out, VoiceView supports Orientation Text. The key for the `com.amazon.accessibility.orientationText` extra configures theOrientation Text for an `AccessibilityNodeInfo`.

*   The first time a user encounters an `AccessibilityNodeInfo`, VoiceView reads the text.
*   On subsequent visits, users can request context information by pressing the **Menu** button, which causes VoiceView to read the Orientation Text.

{% include callout.html color="blue" content="**Example**: The first time a user lands on the main Fire TV home page, VoiceView reads, \"Launcher. Use left and right to move between items on menus. Then use up and down to move between categories such as New Releases or Action Movies. Use left and right to move between items in the category.\"" %} 

### Implementing VoiceView for Usage Hints  

For users without visual impairment, the layout of a screen provides visual cues as to how to navigate and interact with that screen. For example, if item A is located above item B on a screen, the user might intuitively know to press the **Down** button to navigate from A to B. However, visually impaired users might require additional hints to aid their interactions with a screen. To help with this issue, VoiceView supports a set of extras that define Usage Hints to help with navigation within a screen:  

<table>
<tbody>
<tr>
<th>Key</th>
<th>Value</th>
<th>Example</th>
</tr>
<tr>
<td markdown="span">`com.amazon.accessibility.usageHint.remote`  
(Fire TV only)  
</td>
<td>String describing how to use or navigate the item using a remote.</td>
<td>"Press left and right to find an item."</td>
</tr>
<tr>
<td markdown="span">`com.amazon.accessibility.usageHint.touch`  
(Fire Tablets only)  
</td>
<td>Description of how to interact with an item using a touch screen.</td>
<td>"Double tap to select. Double tap and hold for options."</td>
</tr>
</tbody>
</table>

{% include callout.html color="blue" content="**Example**: Consider a screen with a multi-row grid of content, and the first of those rows is in focus, for example \"Customers Also Watched\". In this case, use `com.amazon.accessibility.usageHint.remote` with the text such as \"Press **Left** and **Right** to find an item, press **Up** and **Down** to move between collections of items.\""%}
