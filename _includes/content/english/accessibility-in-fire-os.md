This page discusses the concepts and terminology related to implementing accessibility features for Fire OS.  

{% include note.html content="Before you begin, Amazon recommends reading through the Android documentation for accessibility to familiarize yourself with these terms and concepts. See [Making Applications Accessible](https://developer.android.com/guide/topics/ui/accessibility/apps.html)." %}

* TOC
{:toc}

## Overview of Fire OS Accessibility Objects and Events

This section contains an introduction to the terms and their definitions for the objects and events that you will use when implementing accessibility in your app.

### AccessibilityEvent

An `AccessibilityEvent` is a message from an application to an assistive technology indicating that something happened in the UI. Examples of UI changes that could trigger an accessibility event include focus changes, a window appearing or disappearing, or an on-screen object changing locations.

Also see the Android documentation for [`AccessibilityEvents`](https://developer.android.com/reference/android/view/accessibility/AccessibilityEvent.html).

### AccessibilityNodeInfo

An `AccessibilityNodeInfo` is a snapshot of the accessibility information related to an on-screen component and provides mostly one-way communication between your app and the assistive technology. The assistive technology also has very limited communication back to the app via accessibility actions.

After your application creates and returns an `AccessibilityNodeInfo`, the assistive technology and accessibility framework will not see any further changes to that node, so at that point you can discard the node. If the underlying object represented by the node changes, send an appropriate accessibility event indicating the change, reflecting the updated characteristics of the object in the node returned from the next call to either `createAccessibilityNodeInfo` or `onInitializeAccessibilityNodeInfo`.

In the app, on the server side, reference nodes using a combination of an Android view and a virtual descendant number. If the node represents the view itself, the virtual descendant number is `-1 (AccessibilityNodeProvider.HOST_VIEW_ID)`.

An app can add descriptive information about an `AccessibilityNodeInfo` object in that object's `getExtras` Bundle, which VoiceView can read by calling [AccessibilityNodeInfo.getExtras()](https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.html#getExtras%28%29).

Also see the Android documentation for [`AccessibilityNodeInfo`](https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.html).

### AccessibilityNodeProvider

An `AccessibilityNodeProvider` is an interface which creates an `AccessibilityNodeInfo` either for a view or for the virtually accessible descendant of a view.

Also see the Android documentation for [`AccessibilityNodeProvider`](https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeProvider.html).

### AccessibilityDelegate

An `AccessibilityDelegate`  is an interface that can initialize accessibility events, nodes for views, or virtual accessible descendants. Use `AccessibilityDelegates` to proxy or augment accessibility information in one view from information in another. For example, a parent list can provide accessibility information for a list item.

Also see the Android documentation for [`AccessibilityDelegate`](https://developer.android.com/reference/android/view/View.AccessibilityDelegate.html).

## Path of an Accessibility Event

 This section discusses the path of accessibility events for standard and custom views.  

### Standard Views  

The visual elements that make up the user interface of a Fire OS app are called Views. For example consider a simple app that has a single text element, a `TextView` with the text "hello world". In this example, the `TextView` is a child of a `RelativeLayout`. `RelativeLayout` is a child of `ViewRootImpl`, the class that is always the root of an app's View hierarchy. Since a `TextView` is a standard Android view, the default behavior of the `TextView` is to send appropriate accessibility events when VoiceView is running.

The following process describes the path of an accessibility event created within this simple app: 

1.  The app's code calls `textView.setText("new text")` a few seconds after the app opens.  
2.  The `TextView` creates a `TYPE_WINDOW_CONTENT_CHANGED` `AccessibilityEvent` and requests that its parent, the `RelativeLayout`, send the event.  
3.  The `RelativeLayout` then passes the event up to its parent, the `ViewRootImpl`, and requests that it send the event to the framework.  
4.  The `ViewRootImpl` sends the event to the Accessibility Framework by calling the `AccessibilityManager`'s `sendAccessibilityEvent()`.
5.  The `AccessibilityManager` passes the event to the `AccessibilityManagerService`, which then passes the event out of the Accessibility Framework and into VoiceView.
6.  When VoiceView processes the `TYPE_WINDOW_CONTENT_CHANGED AccessibilityEvent`, it calls the event object's `getSource()` method.
7.  This will trigger the Accessibility Framework to call the `TextView's createAccessibilityNodeInfo()` method, which returns an `AccessibilityNodeInfo` object containing the text "new text".
8.  The Accessibility Framework then passes the `AccessibilityNodeInfo` object to VoiceView.
9.  Finally, VoiceView becomes aware of the `TextView`'s change from "hello world" to "new text", and if a customer uses linear navigation to hear the text of the `TextView`, VoiceView will correctly speak "new text".

### Custom Views  

If your app uses a custom view instead of a standard view, you must uphold this pattern of informing VoiceView of the state of your widget. When doing so, do not try to send an `AccessibilityNodeInfo` object to VoiceView, or update a local copy of the `AccessibilityNodeInfo` in your app; these approaches will not work.

Remember, when you change the state of a custom widget:

**DO**

*   Send an `AccessibilityEvent` by calling `requestSendAccessibilityEvent()` on your custom view's parent.
*   Ensure the variables pointed to in your custom view's `createAccessibilityNodeInfo()` function contain updated values. VoiceView calls `createAccessibilityNodeInfo()` when VoiceView receives your `AccessibilityEvent`.

**DO NOT**

*   Try to send an `AccessibilityNodeInfo` to VoiceView.
*   Keep references to `AccessibilityNodeInfo` objects outside of your `createAccessibilityNodeInfo()` function and then update these objects. VoiceView does not have any way to reference to these objects and will never see your updates to them.

In the Android documentation, be sure to review a code example demonstrating correct use of the `AccessibilityDelegate` and AccessibilityNodeProvider classes where the `createAccessibilityNodeInfo()` function is implemented.

## Setting a view's importance

To ensure that accessibility events are handled properly, make sure to set the importance for accessibility for each Android view. 

Set the state of view's importance for accessibility either in an XML layout with the `android:importantForAccessibility` attribute or programmatically with the `View.setImportantForAccessibility` method. You can set importance to `yes, no, no_hide_descendants`, or `auto` (default). Views that are not tagged as important do not have corresponding accessibility nodes, and additionally, the framework drops any accessibility events sent by them.

If your view is sending appropriate accessibility events, but VoiceView doesn't seem to be responding correctly, verify that your view is tagged as important for accessibility.

## Managing an AccessibilityNodeInfo Object

One important aspect of Fire OS and Android accessibility is the management of `AccessibilityNodeInfo` objects. When the Android accessibility framework, which runs in the app's process, receives a request for a particular node, the framework first checks to see if the node is in its cache:

*   If the node exists in the cache, it is returned, and no call to create the node is made to the app. Because of this, your app must diligently notify the framework and assistive technologies whenever the accessibility information about a component changes. Otherwise, the framework returns stale information to the assistive technologies.
*   To indicate that a node has changed and is no longer valid, send an accessibility event of type `AccessibilityEvent.TYPE_WINDOW_CONTENT_CHANGED`.
*   If the node is not already in the cache depends, the node could be created, depending on how it was requested. Several possible actions by the assistive technology can trigger the creation of an `AccessibilityNodeInfo` object:
    *   Receiving an accessibility event, and requesting its source.
    *   Requesting the root node of the active window.
    *   Requesting the parent or child node of a particular node.

## Implementing Virtually Accessible Descendants

This section discusses how to implement virtually accessible descendant views.

### Creating Virtually Accessible Descendant Views  

Both `AccessibilityDelegate` and `AccessibilityNodeProvider` can provide accessibility for on-screen components that are not represented by backing views. An example of such a component is the AOSP on-screen keyboard whose keys are drawn on a canvas. Note that when implementing a virtual node tree, the `ExploreByTouchHelper` support library provided by Android might not work as expected.

Apps reference `AccessibilityNodeInfo` objects in by the view hosting the node and a virtual view ID. A virtual view ID of `-1 (AccessibilityNodeProvider.HOST_VIEW_ID)` references the host view itself. When creating nodes, you can add virtual children using the `AccessibilityNodeInfo.addChild()` method and specifying both the host view and the virtual view ID of the virtual child. Similarly, when creating child nodes, you can set a virtual parent by specifying the host view and the parent virtual view ID in the call to `AccessibilityNodeInfo.setParent()`.

### Management of a Virtually Accessible Descendant view

When you initially create virtual views, they are only referenced by their IDs and have no associated accessibility information (text, content description, etc.). When the accessibility framework requests that your app create the appropriate node, input the corresponding accessibility information into the `AccessibilityNodeInfo`. As a result, make sure that your app tracks which virtual view ID represents which portion of your custom widget.

Add the IDs of the child or parent nodes, and the framework later requests that your application actually create those nodes. Do not simply add child nodes to a container node.

## Best Practices for Fire OS Accessibility

To ensure the best user experience for apps with accessibility features, follow these guidelines:

*   **AccessibilityNodeInfo text**: The text of an `AccessibilityNodeInfo` should only return the on-screen text of an object.
*   **Labels for onscreen items**: All important onscreen items should have labels. Make sure that when testing with VoiceView, you touch all onscreen items and that you hear an appropriate description read. If a description is missing, add the appropriate text or content description to the item's `AccessibilityNodeInfo`.
*   **Content descriptions**: Use the content description for items that do not have on-screen text (for example, alt text for an image), or to augment the on-screen text to provide additional context to the user. However, avoid putting too much content into the content description to avoid overwhelming the user.  
*   **Accessibility event usage**: Avoid using the `AccessibilityEvent.TYPE_ANNOUNCEMENT` event type to convey on-screen activity. Preferably, use a combination of accessibility events and corresponding changes to the `AccessibilityNodeInfo` to communicate information about on-screen changes.
