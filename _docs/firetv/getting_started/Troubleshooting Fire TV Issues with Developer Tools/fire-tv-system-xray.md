---
title: System X-Ray Overlay on Fire TV
permalink: fire-tv-system-xray.html
sidebar: firetv
product: Fire TV
reviewers: Dongjun Huang (primary), Andy Halverson (SA)
toc: false
github: true
---

System X-Ray is a tool that helps internal and external developers identify app or system problems on Fire TV devices. System X-Ray gathers instantaneous system metrics and displays on top of the screen as an overlay. When toggled on, the overlay will always visible on the screen, even when users run applications, such as playing video or games.

System X-Ray is available on all Amazon Fire TV and Fire TV Stick devices with Fire OS version 5.0.2 and higher.

* TOC
{:toc}

## Enable System X-Ray

{% include content/{{site.language}}/firetv-enabledevtools.md %}

After the Developer Tools Menu dialog box appears, toggle System X-Ray **On**. 

{% include image.html file="firetv/getting_started/images/xray-devtooloptions" type="png" max-width="500px" %}

(To close the dialog box, click your back button.) After you turn on System X-Ray, a long rectangular overlay appears on the screen showing different kinds of information:

{% include image.html file="firetv/getting_started/images/firetv_xrayall" type="png" %}

The System X-Ray overlay remains in place as you change apps or navigate around on Fire TV. The System X-Ray overlay is divided into four sections: 

*  [Display (DIS)](#dis)
*  [CPU (CPU)](#cpu)
*  [Memory (MEM)](#mem)
*  [Network (NET)](#net)

## Display (DIS) {#dis}

{% include image.html file="firetv/getting_started/images/firetv_xraydisplay" type="png" %}

The Display section shows the following:

* **HDMI Mode**: Shows the **physical height** of display in pixels and the **refresh rate** in frames per second. For example, if the Display shows "1080p 60," it means 1080 pixels is the physical height of the display, and 60 is the refresh rate in frames per second.
* **HDCP**: Shows the HDCP (High-bandwidth Digital Content Protection) version used by Fire TV to encrypt content that is sent through the HDMI cable to the television.

Note that Amazon Fire TV allows users to change their resolution by going to Settings > Display & Sounds > Display > Video Resolution. However, regardless of the resolution users select, an app can change the user's selected resolution due to network or system resource reasons to give users a better experience. For example, when the YouTube app plays video, if your network connection is slow, the app might lower the resolution to ensure the playback is still smooth.

## CPU {#cpu}

{% include image.html file="firetv/getting_started/images/firetv_xraycpu" type="png" %}

The CPU (Central Processing Unit) section shows the % CPU usage of each core of the device at real time with different colors. Each column represents a different core. The CPU usage is depicted as follows:

*  0% to 33% (low utilization) shows in <span style="color: green">green</span>
*  34% to 66% (moderate utilization) shows in <span style="color: orange">orange</span>
*  67% to 100% (high utilization) shows in <span style="color: red">red</span>

If an Amazon Fire TV device only has two cores (as with the Fire TV Stick), only two columns will appear.

CPU utilization can help identify CPU-intensive apps. A core that shows consistently heavy usage may indicate a need to make a process multi-threaded.

## Memory {#mem}

{% include image.html file="firetv/getting_started/images/firetv_xraymemory" type="png" %}

The Memory section has a bar with the labels **App** (<span style="color: blue">blue</span>), **Other** (<span style="color: gray">gray</span>), and **Available** (<span style="background-color: gray; color: white; padding: 1px;">white</span>):

*  **<span style="color: blue">Blue section</span>**: App &mdash; shows the memory usage (specifically, the [Proportional set size (PSS)](https://en.wikipedia.org/wiki/Proportional_set_size) of the foreground application, not the GPU memory) and the package name of the foreground app. The package name of the foreground app is displayed below the bar. If you're on the home screen, `com.amazon.tv.launcher` appears as the app name.
*  **<span style="color: gray">Gray section</span>**: Other &mdash; shows the memory usage by other applications.
*  **<span style="background-color: gray; color: white; padding: 1px;">White section</span>**: Available memory &mdash; shows the available (free) memory in the device.

In this example, 44.7 MB of memory is used by the Fire TV launcher, 744.8 MB is used by the whole system, and 849.3 MB is still available.

The Memory information can be used to identify issues such as:

* Memory leaks in an app
* Excessive memory consumption
* Low memory conditions on the device

## Network {#net}

{% include image.html file="firetv/getting_started/images/firetv_xraynetwork" type="png" %}

The Network section shows the strength of the WiFi signal along with the download rates across the entire device and for the visible app. The labels are as follows:

* **RSSI (Received Signal Strength Indication)**: Shows how strong the WiFi signal is, measured in dBm. The bar indicates the signal strength and is color-coded using the same color coding scheme as the CPU section to indicate the severity of a problem (<span style="color: green">green</span> is strong, <span style="color: orange">orange</span> is average, and <span style="color: red">red</span> is weak). The number is always negative -- with better signal strength, the number moves closer to 0. If the Amazon Fire TV has a wired connection and is not using WiFi, RSSI is not shown.
* **System**: Measures how many bits per second are being actively downloaded to the device (including both visible and background apps). This is not the available bandwidth. If 0 bps is displayed, no data is being downloaded at the moment.
* **Visible**: Measures how many bits per second are being actively downloaded by the visible (also called foreground) app. This number will never be higher than the system download speed.

The Network section can be used to diagnose issues such as:

* Connectivity issues
* Slow download speeds
* Lower quality streams (selected by the media player)


## See Also

For more details, see the following:

* [Developer Tool Options on System X-Ray][fire-tv-system-xray-developer-tools]
* [Customize System X-Ray Metrics][fire-tv-system-xray-customized-metrics]

{% include links.html %}
