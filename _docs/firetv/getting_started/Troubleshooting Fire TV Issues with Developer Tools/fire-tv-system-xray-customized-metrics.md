---
title: Customize System X-Ray Metrics
permalink: fire-tv-system-xray-customized-metrics.html
sidebar: firetv
product: Fire TV
reviewers: Dongjun Huang (primary), Andy Halverson (SA)
toc: false
github: true
---

You can customize the display of metrics on System X-Ray by sending the information you want to be displayed. You can use this feature to display anything you find useful, such as static information, when a metric crosses different threshold boundaries, or when an event occurs.

{% include note.html content="See [System X-Ray][fire-tv-system-xray] for an overview of System X-Ray, including how to show the Developer Tool Options menu and read the metrics displayed on the overlay." %}

* TOC
{:toc}

## Create Custom Metrics

System X-Ray displays the App section when it receives an [Intent](https://developer.android.com/reference/android/content/Intent.html), broadcast by your app, with metrics that you define. The following code shows how to create an intent for System X-Ray:

```java
private void updateMetrics(Context context) {

// Initialize Intent
Intent intent = new Intent("com.amazon.ssm.METRICS_UPDATE");
intent.putExtra("com.amazon.ssm.PACKAGENAME", context.getPackageName());

// Add Metrics
intent.putExtra("Metrics1", "First metric");
intent.putExtra("Color1", "green");

intent.putExtra("Metrics2", "Second metric");
intent.putExtra("Color2", "yellow");

intent.putExtra("Metrics3", "Third metric");
intent.putExtra("Color3", "red");

// Send
context.sendBroadcast(intent);
}
```

This sample uses the [`Context`](https://developer.android.com/reference/android/content/Context.html) class to get the necessary resources and classes for the environment.

Initialize the Intent with the action `com.amazon.ssm.METRICS_UPDATE` and add the package name of your app as an extra. The package name is required because System X-Ray will only display the App section when your app is in the foreground. If you put this code in an Activity, you can call the [`getPackageName()`](https://developer.android.com/reference/android/content/Context.html#getPackageName()) helper method.

The metric name must be `Metrics1`, `Metrics2`, or `Metrics3`. Any other name will be ignored. You can set a metric’s value to any String you want, but keep in mind that it may be truncated. System X-Ray displays each metric in the following format: `[name]:[value]`.

A metric’s default color is gray. If you choose to change the color, you will need to add an extra to the Intent. The name of the extra must be `Color1`, `Color2`, or `Color3`. The value can be `red`, `yellow`, `green`, or `blue`. The metric name and color name must have the same number in order for the color to apply. For example, `Metrics1` will get `Color1`. The color values are independent of each other &mdash; you can have more than one metric with the same color.

Now that the Intent is set up, you can call the `sendBroadcast(Intent)` method. If System X-Ray is enabled, it will add a section called `App` and display the metrics defined in the Intent. If you want to change the value or color of a metric, you must recreate the Intent with the new value or color and send it again.

If you are tracking multiple metrics in System X-Ray, you must resend the status of all of them, even if they don’t all change, as System X-Ray does not cache a metric’s state. Otherwise, the metrics you don’t send will be removed from System X-Ray.

## Examples of Custom Metrics

Let’s walk through a few examples of ways you can use this feature.

### Static Information

If you test your app on multiple Fire TV devices or use different WiFi networks, you might want to see which Fire TV model you are testing, or the SSID of the WiFi network the Fire TV is using. As your app starts up, you could get this information and send it to System X-Ray. The following code shows a sample of how to show static information:

```java
private void updateMetrics(Context context, String buildModel, String ssid) {
    // Initialize Intent
    Intent intent = new Intent("com.amazon.ssm.METRICS_UPDATE");
    intent.putExtra("com.amazon.ssm.PACKAGENAME", context.getPackageName());

    // Add metrics
    intent.putExtra("Metrics1", buildModel);
    intent.putExtra("Metrics2", ssid);

    // Send
    context.sendBroadcast(intent);
}
```

The following image is an example display from the above input. In this image, System X-Ray shows that the Fire TV device model is AFTS, which is the 2nd generation Fire TV box. It also shows that Fire TV is connected to the Guest network.

{% include image.html file="firetv/getting_started/images/systemxray_custom1" type="png" max-width="150px" %}

### Thresholds

You might want to track a metric that can cross different thresholds. For example, let’s say your app has video content and you want to track dropped frames during video playback. You might consider less than 5 drops to be green, 5-9 to be yellow, and 10 and higher to be red.

As you update the number of dropped frames, you change the color to match the threshold. The following code sample shows how show a threshold:

```java
private void updateMetrics(Context context, int numFrameDrops, String frameDropStatus) {
    // Initialize Intent
    Intent intent = new Intent("com.amazon.ssm.METRICS_UPDATE");
    intent.putExtra("com.amazon.ssm.PACKAGENAME", context.getPackageName());

    // Add metrics
    intent.putExtra("Metrics1", "FrameDrops:"+numFrameDrops);
    intent.putExtra("Color1", frameDropStatus);

    // Send
    context.sendBroadcast(intent);
}
```

Here are screenshots of what the different thresholds look like:

{% include image.html file="firetv/getting_started/images/systemxray_custom2" type="png" max-width="500px" %}

### Events

Event logging is useful, but you might want a visual way to track when an event last occurred. For example, perhaps testing reveals that an intermittent Exception is thrown after 3 hours of video playback. Here's a sample of how to configure an event:

```java
private void updateMetrics(Context context, String message, String time) {
    // Initialize Intent
    Intent intent = new Intent("com.amazon.ssm.METRICS_UPDATE");
    intent.putExtra("com.amazon.ssm.PACKAGENAME", context.getPackageName());

    // Add metrics
    intent.putExtra("Metrics1", message);
    intent.putExtra("Color1", "Red");

    intent.putExtra("Metrics2", "Time:"+time);
    intent.putExtra("Color2", "Red");

    // Send
    context.sendBroadcast(intent);
}
```

In the following screenshot, System X-Ray displays the Exception, along with the time it occurred.

{% include image.html file="firetv/getting_started/images/systemxray_custom3" type="png"  max-width="150px" %}

## See Also

For more details, see the following:

* [System X-Ray on Fire TV][fire-tv-system-xray]
* [Developer Tool Options on System X-Ray][fire-tv-system-xray-developer-tools]

{% include links.html %}
