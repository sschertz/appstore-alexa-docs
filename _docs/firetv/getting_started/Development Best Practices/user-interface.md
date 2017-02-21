---
title: Android Menu and ActionBar widgets
permalink: user-interface.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

This page describes how to modify the Android Menu and ActionBar widgets to work for your Fire TV app.

The Amazon Fire TV platform supports the majority of the existing Android UI framework (`android.widget.*`) with no changes. The two exceptions are [Menu][1] and [ActionBar][2]. All other Android widgets work without modifications, although those widgets may have a different appearance in the Fire TV user interface.

## Menus and Action Bar

When your app uses Android's [ActionBar][3], it is important to note that, in order to avoid cluttering the user interface, the action bar items do not appear on the screen. Instead, items from the action bar are displayed in a modal dialog box when the user presses the Menu button on one of the Fire TV remotes or the Fire Game Controller. The user can then pick an Action Item or a Navigation Tab from the dialog.

Currently, only the action items, tabs, options menu and sub menus on the action bar are handled. On the launch of any application or activity, the action bar does not show up by default. Pressing the menu button brings up a dialog window containing two listviews arranged side by side, at a max. The left list view contains all the tabs and the one on the right shall contain the action items.

Users can navigate between and within the lists with the directional navigation buttons on the remote or game controller. If for an item, a sub-menu exists, then clicking on the item causes its sub-menu appear as a list in the place of its parent. This implementation does not keep track of the menu hierarchy and the navigation state. Pressing Back at any point dismisses the dialog window.

You can get a handle to the action bar using the [`getActionBar()`][4] method in the [`onCreate(Bundle)`][5] method for your activity. Access to its complete [API][6] is possible in your activity.

Note that Drop-down lists and action Views are not supported by the Amazon Fire TV platform.

[1]: http://developer.android.com/reference/android/view/Menu.html
[2]: http://developer.android.com/reference/android/app/ActionBar.html
[3]: http://developer.android.com/guide/topics/ui/actionbar.html
[4]: http://developer.android.com/reference/android/app/Activity.html#getActionBar%28%29
[5]: http://developer.android.com/reference/android/app/Activity.html#onCreate%28android.os.Bundle%29
[6]: http://developer.android.com/reference/android/app/Activity.html

{% include links.html %}
