Accessibility is the degree to which a product or service can be used by a customer with a particular disability. Accessible products and services enable users with disabilities to easily and efficiently use those products and services.

* TOC
{:toc}

## Accessibility Overview

Accessible systems have three major components:

*   Application being made accessible
*   Accessibility framework
*   Assistive technology

Assistive technologies help a person with a disability accomplish a task or use a product. Examples of assistive technologies include screen reading software for blind users, screen magnification software for users with low vision, or wheel chairs for users who are unable to walk.

This page provides a conceptual over view of the assistive techonologies that are available from Amazon for your Fire OS apps.

## Available Fire OS Assistive Technologies

Fire OS currently supports several assistive technologies:

*   **VoiceView**: Enable a blind user to interact with objects on the screen via speech output and either touch or keyboard input.
*   **Screen Magnifier**: Enable a vision-impaired person to view an enlarged version of the screen.
*   **Large Font**: Increase the font size throughout most of the UI.
*   **Color Inversion**: Easily view and read content on your Fire tablet with the provided high-contrast color combinations for text and background colors.  
*   **Color Correction**: Adjust the screen's color output to help people with colorblindness distinguish more colors.
*   **Stereo to Mono Audio**: Switch the stereo audio setting to mono on your Fire tablet to direct the audio into a specific ear bud.  
*   **System Closed Captioning**: Customize the appearance of closed captions for Amazon Instant Videos and web videos in the Silk Browser.

Currently all technologies are available on Fire Tablets. Fire TV only supports VoiceView.  

## Activating and Using Assistive Technologies for Fire OS

This section describes how users can enable the available assistive technologies on each supported device.

### Activating and Using VoiceView on Fire Tablets

To activate VoiceView on a Fire Tablet, navigate to **Settings > Accessibility > VoiceView**, and enable VoiceView. Alternatively, you can hold down the power button until the power dialog appears, then press and hold two fingers to the screen.  

To use VoiceView on a Fire Tablet:  

1.  Touch an item on the screen to move the VoiceView cursor to that item.  
    *   When an item has VoiceView's focus, a green rectangle will appear, surrounding the item.
    *   When a user moves the VoiceView cursor to an item, VoiceView speaks the item's description.
2.  Double-tap anywhere on the screen to activate the item in the focus of the VoiceView cursor. 

OR

Instead of touching items to move the VoiceView cursor, you can move sequentially through all items on the screen by swiping right to move to the next item, and swiping left to move to the previous item.

### Activating VoiceView on Fire TV  

To activate VoiceView on Fire TV:

1.  Enable VoiceView by holding down the **Back** and **Menu** keys on the Fire TV remote for 2 seconds.  

    VoiceView has two navigation modes available: Standard Navigation Mode or Enhanced Navigation Mode. You can switch to Standard Navigation Mode by holding down the **Menu** key. Note that in Standard Navigation Mode, VoiceView's cursor will only move among actionable items, such as buttons.   

2.  In Enhanced Navigation Mode, press the **Right** and **Left** directional keys on the remote  to move VoiceView's cursor, shown as the green focus rectangle, to an item.   

3.  Press the **Select** key to activate an item.  

    {% include note.html content="**Left** and **Right** move the cursor in an order that is logically, not visually, determined.  For example, they will move the cursor to the next and previous items in a list, regardless of whether that list is displayed horizontally or vertically. Enhanced Mode also allows access to reach items you cannot reach otherwise such as descriptive text that is not actionable." %}  

### Activating the Screen Magnifier on Fire Tablets

Note that the Screen Magnifier is not currently available on Fire TV. To activate the Screen Magnifier on Fire Tablets:

1.  With the screen magnifier enabled, triple-tap with one finger to show an enlarged view of the screen.
2.  Pan around the screen by dragging two fingers.
3.  Triple-tap with one finger to exit the enlarged view of the screen.

## Differences between VoiceView on Fire OS and TalkBack on Android

While Fire OS's VoiceView and Android's TalkBack are both accessibility services that interact with the Android accessibility framework, VoiceView is a completely distinct screen reader from TalkBack, as opposed to a modification of TalkBack.  This section discusses the differences between these two accessibility services:

*   **Focus behavior in new windows**: When a new window opens, VoiceView always places accessibility focus somewhere onscreen.  TalkBack does not place focus anywhere on a new window, and instead waits for the user to touch the screen and places focus at that location.
*   **Linear navigation across windows**: VoiceView allows linear navigation across window boundaries, while TalkBack does not.  Consider the bottom navigation bar on a tablet, which is actually a window that contains three buttons.
    *   In VoiceView, swiping left will move the cursor to the last item in the main content window.
    *   In TalkBack, swiping left from the **Back** button in the bottom navigation bar on TalkBack will not allow you to linearly navigate out of the bottom navigation bar and will produce an "end" earcon.
*   **Granular navigation across objects**: VoiceView allows granular navigation across objects, while TalkBack does not.  Consider a screen containing three objects with titles "Cat, "Dog", and "Monkey".
    *   When navigating by word, VoiceView moves seamlessly from "Cat" to "Dog" to "Monkey".
    *   Conversely, TalkBack will stop on "Dog" and not navigate to the next word.  
    *   Similarly, when navigating by character, if you swipe down to move to the next character after landing on the "g" of "Dog", VoiceView will move to the "C" of "Cat". TalkBack plays a sound indicating the end of text in this case.
*   **Text navigation**: When navigating through text, VoiceView announces the character or word after the caret, TalkBack announces the character or word that the caret has passed over.  Note that VoiceView's behavior is consistent with the behavior of screen readers on the Windows platform, which is the platform with which most blind users are familiar.  

*   **Sorting on-screen objects**: TalkBack typically sorts on-screen objects in a left to right, top to bottom order, based on the coordinates of the objects' top-left corners. VoiceView typically sorts on-screen objects in a left to right, top to bottom order, based on the coordinates of the objects' centers.
