---
title: Design and User Experience Guidelines for TV Platforms
permalink: design-and-user-experience-guidelines.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Television user interface design differs significantly from the design for desktop computers, tablets, or phones. These guidelines will help you become familiar with the design principles for a 10-foot UI and help you integrate your application and its design into the Amazon Fire TV user interface.

* TOC
{:toc}

## General Principles

Use these principles to guide the design of your app as a whole.

### 10-Foot UI

TV interfaces are often referred to as 10-Foot User Interfaces (10-ft UI), because the user is viewing the screen from 10 or more feet away. Although the screen itself can be large, the screen resolution is lower, and distance from the screen means a smaller angle of view.

The design choices you would make for an application or web page running on a desktop computer, tablet, or phone are fundamentally different, as users typically view those screens from much closer distances.

In addition, as the television is used in a more relaxed fashion than a computer, a tablet or a phone, the UI on the TV should not require as much attention and precision. 10-ft UI may require you to wholly rethink the design, layout, and navigation of an existing app.

### Clear, Simple, and Visual

The design of a screen in a 10-ft UI requires simplicity and clarity, with low information density. Limit the number of design elements or UI components (menus, buttons, images) on the screen, and ensure that those elements are large enough and spaced far enough apart to be read from a distance. Present a clear set of actions or options for each screen.

Minimize the amount of text, as users do not read a lot of text on a television screen. Avoid requiring the user to input a lot of information and provide reasonable defaults where possible.

### Place Important Content First

Place the most important content or options first on the screen so they are easily viewable and navigable by the user.

### Focus on Consumption

Applications should have a clear focus on getting users to content quickly. Television interfaces are primarily about providing entertainment. When users sit down in front of their television, they don't want to do extra work. They need simple user interfaces that match their primary goal: "Give me something to watch or listen to or play with right now."

## Design Guidelines

Use these guidelines when designing individual screens and views in your app.

### Screen Size and Resolution

When you design an app for a tablet or phone screen, you're working with screens that have a fixed size and resolution. TV design differs in that the same app can appear in either 720p or 1080p resolutions, on a screen of any size.

We recommend for the best possible experience that you design your app and its resources for a full 1080p television screen. The Amazon Fire TV platform scales your resources to the appropriate TV output. For 1080p the screen size is 1920x1080px, the density is 320dpi ("xhdpi") and the output resolution is 960x540dp ("large").

The Amazon Fire TV platform also supports the standard Android configurations for enabling multiple resource directories for different output parameters, as described in the Android developer guide for [Supporting Multiple Screens][1].

When your app runs,  Fire TV loads your resources from the appropriate folder. See [Display and Layout][display-and-layout] for more information on the resource configurations available for Amazon Fire TV.

### Overscan and the Safe Zone

Regardless of its physical size, TV hardware manufacturers reserve space around the displayable area of the screen. This reserved space is known as _overscan_. The amount of space a TV uses as overscan varies across manufacturers. That real estate is not available to your app.

Although the Amazon Fire TV platform provides a way for the user to adjust for television overscan in the settings, for the safest possible behavior we recommend that you avoid placing any of your app's UI elements within the outer 5% of any edge on the screen. The focused item and on-screen text, especially, should be fully within the inner 90% (the safe zone) of your user interface.

{% include image.html alt="Overscan" file="firetv/getting_started/images/overscan" type="png" %}

You can display an overscan on your Fire TV through the Developer Options. See [System X-Ray][fire-tv-system-xray].

### Color

Television screens have a higher contrast than computer screens, which can make colors seem more saturated, brighter, and vibrant. The color gamut (the range of colors that can be displayed) is also less than that of a PC screen. In your app, use less saturated colors. Cool colors (blue, purple, gray) work better than warmer colors (red, orange).

{% include image.html alt="Color" file="firetv/getting_started/images/color" type="png" %}

### Typography

Because television screens must be read from across the room, use larger type sizes for body text (at least 14sp, which is approximately 19px on 720p, 28px on 1080p). Amazon uses Helvetica Neue Regular as the system font.

Keep item descriptions or other blocks of text as short as possible, both in content and in line width. Use greater line spacing than you would use on a desktop or tablet screen. Separate text into paragraphs or chunks and write in short, declarative sentences.

## Navigation and Input

Navigation and user input on the Fire TV user interface are both accomplished with one of the Fire TV remotes (Amazon Fire TV Remote or Voice Remote), or with a game controller (either the Amazon Fire Game Controller, or other Bluetooth game controllers) The use of a physical controller instead of mouse, keyboard or touch input makes input and control methods for Fire TV apps less flexible than on other devices.

### D-Pad Directional Navigation

The Left, Right, Up, and Down D-Pad buttons on one of the Fire TV remotes or on a game controller are used to navigate the user interface of your app. Clearly indicate how users should move through your app's user interface. A clear up-down and left-right orientation should be immediately apparent to the user and every actionable on-screen element should be reachable with the D-Pad.

### Focus and Selection

As the user navigates the user interface with the directional buttons on a remote or game controller, different UI elements highlight to indicate that an element has the focus. Your app should clearly indicate which on-screen element currently has the focus. When users glance away from the TV, upon return their gaze it should remain clear what options they have for navigation.

When the user presses Select (or the A button on a game controller) while a UI element has the focus, that element should momentarily change to the selected state.

### Text Entry

When the user navigates the focus to a text field, a system keyboard automatically appears. Users can then enter information by selecting letters and numbers with the directional buttons on the remote or game controller. Suggested completions appear and can be selected at any time.

## Screens, Views, and Flows

This section describes the patterns for the major screens and views in the Amazon Fire TV user interface, as well as descriptions of the controls used to build them. Use these patterns as a reference if you choose to optimize and integrate the design of your own apps with the system UI.

{% include image.html alt="Screens" file="firetv/getting_started/images/3_screens" type="png" %}

### Home Screen (Launcher)

{% include image.html alt="Screen" file="firetv/getting_started/images/home-1" type="png" %}

The home screen consists of a global navigation menu on the left and a set of content tiles on the right.

The global navigation menu is the primary system menu. It appears in a row on the left side of the screen. The global navigation menu allows the user to choose major content categories or other options including Search, Home, Movies, TV, Music, Games, Apps, and so on. Each item in the global navigation menu can be selected with the Up and Down directional buttons.

When the user focuses on any item in the global navigation menu, the home view for that node appears on the right side of the screen. Each node has its own home view with its own content. The overall system home view, sometimes called the launcher, is accessible with the Home key on the Fire TV remote or game controller, or by selecting Home from the global navigation menu.

{% include image.html alt="Content row" file="firetv/getting_started/images/home-1-3rd-row-focus" type="png" %}

Each home view contains multiple horizontal content rows. The title tile for the row indicates the type of content (for example, Most Popular, My Movies, Recommended for You). The remaining tiles show examples of that content. From these content rows, the user can:

* Navigate between rows with the Up and Down directional buttons.
* Move back to the navigation menu with the Left button.
* Choose Select or Right to select a row and view a 1D list for that row.

### 1D List Views

{% include image.html alt="1D list" file="firetv/getting_started/images/1d_outerspace-first-tile-focus" type="png" %}

1D list views appear when the user selects a content row from a home view. 1D lists contain a single row of items. The title bar indicates the title of the list (for example, Most Popular, My Movies, Recommended for You), and two numbers indicating the number of items in the list and the position of the item that has the focus.

The item with the focus in a browse list displays the mini details for that item below the list. Mini details can include basic item information (title, date, rating) as well as options such as Add to Watchlist.

From the 1D list, the user can:

* Navigate between items with the Left and Right directional buttons. Navigating past the end of the list wraps the focus around to the first item.
* Choose an item with the Select button, which shows the detail view for that item.

### Detail View

{% include image.html alt="Detail view" file="firetv/getting_started/images/detail-watch-now-focus" type="png" %}

The Detail view appears when the user selects an item from a browse list. This image shows the Detail view for a movie. The Detail may be different for TV, music, or other content. The Detail view displays information and actions related to a piece of content or other item.

The actions under the description provide possible actions for this item. The actions available vary depending on the user's subscription status (for example, whether they are an Amazon Prime member) as well as content availability.

The discovery menu on the left side of the screen provides additional related information about the item (for example, Cast, Trivia, or Reviews).

### Search

The Search menu item on the home screen opens the search screen, from which users can access voice search or text search. Search is also accessible with the Microphone button on the Amazon Fire TV Voice Remote, and appears in an overlay on the current app or content.

For text search, the user moves LEFT and RIGHT through the alphabet, pressing SELECT on each letter to type a search query. Possible results appear in a list below the query.

{% include image.html alt="Search" file="firetv/getting_started/images/search" type="png" %}

Global search is provided system-wide and is not customizable for individual apps. Developers may implement their own in-app search, but it is not included in the global search function.

### Additional Resources

For more information on UX best practices when designing for the ten-foot experience, see [Android TV](https://www.google.com/design/spec-tv/android-tv/introduction.html).


[1]: https://developer.android.com/guide/practices/screens_support.html

{% include links.html %}
