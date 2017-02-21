---
title: Developer Tool Options
permalink: fire-tv-system-xray-developer-tools.html
sidebar: firetv
product: Fire TV
reviewers: Dongjun Huang (primary), Andy Halverson (SA)
wikipage: https://wiki.labcollab.net/confluence/display/DEVTECH/DevTech+System+X-Ray
toc: false
github: true
---

The Developer Tools Menu provides a number of options that provide real-time metrics and other information about your app. This information can assist you in troubleshooting, development, and testing.

* TOC
{:toc}

## Invoke the Developer Tools Menu

{% include content/{{site.language}}/firetv-enabledevtools.md %}

The following screenshot shows the options on the Developer Tools menu:

{% include image.html file="firetv/getting_started/images/xray-devtooloptions" type="png" max-width="500px" %}

Not all the tools shown above are available in Generation 1 Fire TV devices. However, an upcoming release will bring the Development Tools Menu options into parity with Gen 1 devices. The only exception is with Advanced Options (the multimedia overlay), which won't be available on Fire TV Stick Gen 1.


## System X-Ray

System X-Ray gathers instantaneous system metrics and displays on top of the screen as an overlay. When toggled on, the overlay will always visible on the screen. 

{% include image.html file="firetv/getting_started/images/firetv_xrayall" type="png" %}

The System X-Ray overlay contains details about the following:

*  Display 
*  CPU 
*  Memory 
*  Network

For deep-dive into System X-Ray, see [System X-Ray on Fire TV][fire-tv-system-xray].

## Advanced Options

Advanced Options enables multimedia information to appear when Android MediaCodec APIs are in use. When you switch this option on and then play media, an additional display (titled "MUL" for Multimedia) appears on the right.

{% include image.html file="firetv/getting_started/images/systemxray-multimedia" type="png" %}

{% include note.html content="Advanced Options is not available on Fire TV Stick Generation 1 (due to limited system resources). Even with future updates, Fire TV Stick Gen 1 devices won't display the multimedia overlay." %}

Information displayed in the Multimedia panel is divided into two sections: AUDIO and VIDEO.

**AUDIO:**

* Codec
* Hardware Accelerated
* Input Bitrate
* Secure

**VIDEO**: 

* Codec
* Hardware Accelerated
* Input Bitrate
* Secure
* Frames Dropped
* Resolution
* Frame Rate

## Snapshot

Snapshot provides a way for users to gather instantaneous all metrics information through `adb` command. Whenever you input the following command, metric information will display in the command line. 

```
adb shell dumpsys activity service com.amazon.ssm/.OverlayService
```

System X-Ray must be running for this command to function.

Here's a sample output: 

```
SERVICE com.amazon.ssm/.OverlayService 3dde6680 pid=10820
  Client:
    [com.amazon.ssm.timestamp]: [2017-02-07 15:11:53]
    [com.amazon.ssm.display.resolution]: [1080]
    [com.amazon.ssm.display.refreshrate]: [60]
    [com.amazon.ssm.display.hdcpversion]: [1.0]
    [com.amazon.ssm.cpu.core0]: [30]
    [com.amazon.ssm.cpu.core1]: [29]
    [com.amazon.ssm.cpu.core2]: [0]
    [com.amazon.ssm.cpu.core3]: [0]
    [com.amazon.ssm.memory.appname]: [tv.twitch.android.viewer]
    [com.amazon.ssm.memory.appmemory]: [56.8 MB]
    [com.amazon.ssm.memory.activememory]: [1.3 GB]
    [com.amazon.ssm.memory.availablememory]: [231.3 MB]
    [com.amazon.ssm.network.rssi]: [-56]
    [com.amazon.ssm.network.systemdownloadspeed]: [2.2 Mbps]
    [com.amazon.ssm.network.appdownloadspeed]: [2.1 Mbps]
```

## Record & Share

{% include note.html content="The Record & Share feature is in experimental beta, so be aware that this feature may have some issues. For example, if the memory is too large, the output may time out." %}

Record & Share stores instantaneous metrics about CPU, memory, network, and multimedia into a database as historical data. Although the same information is displayed graphically in real-time through the System X-Ray overlay, Record & Share takes this information and stores it into a history that you can dump to the command line.

To use Record & Share, first toggle the Record & Share setting in the Developer Tools Menu to **On**. You're then prompted to select the Record Settings:

{% include image.html file="firetv/getting_started/images/recordsettings" type="png" max-width= "400px" %}

These properties control the following:

* **Interval**: The time between two recordings: 2 seconds, 4 seconds, 8 seconds, 16 seconds, or 32 seconds.
* **Duration**: How long the data gets stored in the database: 1 hour, 2 hours, 4 hours, 8 hours, or 16 hours.

The default (2s interval, 1 hr duration) means that every 2 seconds, statistics will be recorded and stored in the database. The recording will be stored in the database for a total of 1 hour.

After playing media to gather some recorded information, you can dump all historical metrics to the command line using the following::

```
adb shell dumpsys activity service com.amazon.ssm/.OverlayService -all
```

The response includes the following information:

CPU:

* Timestamp
* cpu0
* cpu1
* cpu2
* cpu3

Memory:

* Timestamp
* Total_Memory
* Available_Memory
* Active_Memory
* Foreground_App_Memory
* Foreground_App_PackageName

Network:

* Timestamp
* RSSI
* Download_Speed
* Foreground_App_Download_Speed
* Foreground_App_PackageName

Multimedia:

* Timestamp
* AudioCodec
* AudioInputBitrate
* AudioAccelerated
* AudioSecure
* VideoCodec
* VideoInputBitrate
* VideoAccelerated
* VideoSecure
* VideoResolution
* VideoFramerate
* VideoFramedropped

Here's an example of the display on the command line:

```
 CPU
 Timestamp           cpu0 cpu1 cpu2 cpu3
 2016-10-31 11:40:22 19   16   13   18

 MEMORY
 Timestamp           Total_Memory Available_Memory Active_Memory Foreground_App_Memory Foreground_App_PackageName
 2016-10-31 11:40:23 919.3 MB     156.3 MB         731.8 MB      31.3 MB               com.amazon.ssm

 NETWORK
 Timestamp           RSSI Download_Speed Foreground_App_Download_Speed Foreground_App_PackageName
 2016-10-31 11:40:21 -41  14.4 kbps      0 bps                         com.amazon.ssm
 2016-10-31 11:40:23 -41  14.0 kbps      0 bps                         com.amazon.ssm
```

If you're interested in only part of the metrics, you can add different options in the command. For example, to dump memory and network historical metrics to command line:

<pre>
adb shell dumpsys activity service com.amazon.ssm/.OverlayService <span class="red">-memory -network</span>
</pre>

The following table shows all available options:

| Option | Description |
|-------|--------|
| blank <br/>(no option passed) | dump snapshot information |
| `-snapshot` | dump snapshot information |
| `-all` | dump all information from database |
| `-memory`| dump memory information from database |
| `-cpu` |  dump CPU information from database |
| `-network` | dump network information from database |
| `-multimedia` | dump multimedia information from database |


To check available options, pass the `-help` parameter:

```
adb shell dumpsys activity service com.amazon.ssm/.OverlayService -help
```

You can clear the recorded metrics stored in the database (before the duration time automatically clears the data). From the **Developer Options Tools** menu, select **Record & Share**, and then click the **menu** button on your remote.

{% include image.html file="firetv/getting_started/images/xraycleardb" type="png" max-width="500px" %}

## Safezone

Some TVs use overscan with their display. Overscan means the TV displays some information off the edges of the visible screen (to accommodate discrepancies in monitors). You should not display important information in the overscan areas.

To make the overscan areas visible, you can turn the **SafeZone** switch to **On**. This will make the overscan areas apparent so you can avoid displaying any information in these areas.

{% include image.html file="firetv/getting_started/images/xraysafezone" type="png" %}

Note that Fire TV Stick (Generation 2) does not include screen size calibration. If the screen display doesn't fit correctly on the TV screen, the overscan area may not show accurately.

## Developer Options

{% include note.html content="This feature is still in development. More information will be released shortly about this feature." %}

Currently some select third-party apps, such as Netflix and HBO Go, display recommendations on the Fire TV home screen in specific rows.

Developer Options allows you to turn on a row called "Recommended By Your Apps" (displayed below the Netlix and HBO Go rows). This "Recommended By Your Apps" row will show recommendations sent from third-party apps. 

Currently, turning this row on shows only recommendations that your own app sends. When the feature is fully released, it will show recommendations from all third-party apps the user has installed (excluding some apps such as Netflix and HBO Go, which display recommendations on their own rows).

## Launch Network Advisor

Launches a network analysis window that checks your network connection strength, channel, and other details. If there are problems, the Network Advisor provides recommendations to fix the issues.


## See Also

For more details, see the following:

* [System X-Ray on Fire TV][fire-tv-system-xray]
* [Customize System X-Ray Metrics][fire-tv-system-xray-customized-metrics]

{% include links.html %}