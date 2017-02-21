---
title: Integrating Amazon Fling with an Existing Android App that Uses Google Cast
sidebar: fling
product: Fling SDK
permalink: integrating-amazon-fling-with-an-existing-android-cast-app.html
reviewers: jeffersd
github: true
---

Using our SDK, you can talk to Amazon Fire TV devices in a similar manner as you use Google Cast in your app to talk to Chromecast devices.  This page outlines steps to modify an existing app that uses the Google Cast Companion Library to also fling to Amazon Fire TV.

Before you start, make sure you follow the steps at [Setting Up your Amazon Fling Development Environment for Android][setting-up-your-amazon-fling-development-environment-for-android] to include the required libraries in your project.  

{% include note.html content="All the code in this document is available in the CastWithFlingExample in the samples folder of our SDK package." %}

## Modifying the Android Manifest

The API included in our SDK requires some permissions to be added to your Android manifest (`AndroidManifest.xml`). After the `<manifest>` element, add the `<uses-permission>` elements. Some of these permissions may already be present as they may also be used by applications that use Google Cast.

```java
manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="[YOUR PACKAGE NAME]"
    android:versioncode="1"
    android:versionname="1.0">
<!-- Android Network Access Permissions -->
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE"/>
...
</manifest>```

For Fire OS devices, also add the <uses-library> element so your application can communicate with the underlying framework on the device.

```java
application
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme" >
    <uses-library android:name="com.amazon.whisperplay.contracts" />
    ...
</application>
```

## Initializing DiscoveryController

Amazon Fling service provides similar functionality to Google Cast's `VideoCastManager` via different classes:

*  `DiscoveryController`, the interface used to discover remote Fire TV devices and players.
*  `RemoteMediaPlayer`, the interface that represents a player running on a remote Fire TV device. Use of this interface is described below in Converting Your Device Picker to use RemoteMediaPlayer.  

To use `DiscoveryController`, initialize it in your `onCreate()` method. Replace `YourReceiverServiceID` with your Amazon Fire TV player's system ID (SID).

```java
private DiscoveryController mController;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        mController = new DiscoveryController(this);
        ...
    }

    @Override
    protected void onResume() {
        ...
        mController.start("YourReceiverServiceID", mDiscovery);
        ...
    }

    @Override
    protected void onPause() {
        ...
        mController.stop();
        ...
    }
```

## Converting Your Device Picker to use RemoteMediaPlayer

The device picker is the part of your app's user interface that allows the user to view and pick available remote players. Your controller app uses the RemoteMediaPlayer interface for device discovery and selection through a custom device picker.

Cast applications that use the `MediaRouterButton` class, as in the code below, must be replaced by a custom device picker.  

```java
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        ...
        getMenuInflater().inflate(R.menu.main, menu);
        mCastManager.addMediaRouterButton(menu, R.id.media_route_menu_item);
        ...
    }
```

If your app implements a custom device picker like the code below you must modify that code to support `RemoteMediaPlayer`.  

```java
    private MediaRouter mMediaRouter;
    private List<CastDevice> mDeviceList = new LinkedList<CastDevice>();
    private CastDevice mCurrentDevice;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        mMediaRouter = MediaRouter.getInstance(this);
        ...
    }

    @Override
    protected void onResume() {
        ...
        mMediaRouter = MediaRouter.getInstance(this);
        mMediaRouter.addCallback(mCastManager.getMediaRouteSelector(), mCastCallback,
            MediaRouter.CALLBACK_FLAG_REQUEST_DISCOVERY);
        ...
    }

    @Override
    protected void onPause() {
        ...
        mMediaRouter.removeCallback(mCastCallback);
    }

    private MediaRouter.Callback mCastCallback = new MediaRouter.Callback() {
        public void onRouteAdded(android.support.v7.media.MediaRouter router,
            android.support.v7.media.MediaRouter.RouteInfo route) {
            CastDevice device = CastDevice.getFromBundle(route.getExtras());
            if (mDeviceList.contains(device)) {
                mDeviceList.remove(device);
                ...
            } else {
                ...
            }
            mDeviceList.add(device);
            triggerUpdate();
        }
        public void onRouteRemoved(android.support.v7.media.MediaRouter router,
            android.support.v7.media.MediaRouter.RouteInfo route) {
            CastDevice device = CastDevice.getFromBundle(route.getExtras());
            if( mDeviceList.contains(device) ) {
                ...
                mDeviceList.remove(device);
                triggerUpdate();
            }
        }
    };
```

This example uses a list of of `RemoteMediaPlayer` objects to keep track of the currently selected player device in a list view. The `IDiscoveryListener` interface is used to listen for when a new player device becomes available or an existing player device is no longer available. The listener is added when starting your controller app in the `onResume()` method.  

```java
    private List<RemoteMediaPlayer> mDeviceList = new LinkedList<RemoteMediaPlayer>();
    private RemoteMediaPlayer mCurrentDevice;

    private DiscoveryController.IDiscoveryListener mDiscovery = new DiscoveryController.IDiscoveryListener() {
        @Override
        public void playerDiscovered(RemoteMediaPlayer device) {
            if (mDeviceList.contains(device)) {
                mDeviceList.remove(device);
                ...
            } else {
                ...
            }
            mDeviceList.add(device);
            triggerUpdate();
        }

        @Override
        public void playerLost(RemoteMediaPlayer device) {
            if( mDeviceList.contains(device) ) {
                ...
                mDeviceList.remove(device);
                triggerUpdate();
                ...
            }
        }

        @Override
        public void discoveryFailure() {
            ...
        }
    }
```

The device picker list is updated in the following method that handles device objects from both Google Cast and this SDK.  

```java
    void triggerUpdate() {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                final ListView lv = (ListView) findViewById(R.id.listView1);
                ArrayAdapter<Object> ad = (ArrayAdapter<Object>) lv.getAdapter();
                ad.notifyDataSetChanged();
            }
        });
    }
```

## Monitoring the Status of the Player with StatusListener

Google Cast provides callbacks to manage application status changes. The Google Cast Companion Library also keeps track of some statuses such as the current media position through the `VideoCastConsumerImpl class`. Our API provides the `StatusListener` class to receive status updates based on a given interval. The `StatusListener` class provides the same functionality as `VideoCastConsumerImpl`. Add the status listener to your controller app to receive status updates, and set the interval for updates when your status listener is successfully added.

The `addStatusListener()` and `setPositionUpdateInterval()` methods, as with all remote methods in the API, are asynchronous calls. The API gives the result from these asynchronous calls by providing `FutureListener<T>`. Chain the `getAsync()` method with remote methods to get the result. Once the asynchronous task is done, the remote method call invokes the `futureIsNow()` method from the receiver app.

```java
    private StatusListener mListener;
    private Status mStatus = new Status();
    private static final long MONITOR_INTERVAL = 1000L;

    private DiscoveryController.IDiscoveryListener mDiscovery = new DiscoveryController.IDiscoveryListener() {
        @Override
        public void playerLost(RemoteMediaPlayer device) {
            ...
            device.removeStatusListener(mListener).getAsync(new FutureListener<Void> {
                @Override
                public void futureIsNow(Future<Void> voidFuture) {
                    //handle asynchronous task
                }
            });
        }
    };

    @Override
    protected void onResume() {
        ...
        mListener = new Monitor();
        ...
    }

    private static class Status {
        public long mPosition;
        public MediaState mState;
        public MediaCondition mCond;
        public synchronized void clear() {
            mPosition = -1L;
            mState = MediaState.NoSource;
        }
    }
    private class Monitor implements StatusListener {
        @SuppressLint("NewApi")
        @Override
        public void onStatusChange(AmazonMediaStatus status, long position) {
            if (mCurrentDevice != null) {
                synchronized (mStatus) {
                    mStatus.mState = status.getState();
                    mStatus.mCond = status.getCondition();
                    mStatus.mPosition = position;
                    ...
                }
                ...
            }
        }
    }
    ...
    private void fling(final Object target, final String name, final String title) {
        mCurrentDevice = target;
	mCurrentDevice.addStatusListener(mListener).getAsync(new ErrorResultHandler("Cannot set status listener"));
        mCurrentDevice.setPositionUpdateInterval(MONITOR_INTERVAL).getAsync(
            new ErrorResultHandler("Error attempting set update interval, ignoring”));
        mCurrentDevice.setMediaSource(name, title, true, false).getAsync(new ErrorResultHandler("Error attempting to Play”));
    }
    private class ErrorResultHandler implements FutureListener<Void>  {
        ErrorResultHandler(String msg) {
	    this(msg, false);
        }
        @Override
        public void futureIsNow(Future<Void> result) {
            try {
                result.get();
            } catch(ExecutionException e) {
              //handleFailure
            } catch(Exception e) {
              //handleFailure
            }
        }
    }
```

## Communicating With the Remote Media Player

The Google Cast Companion Library provides only the callbacks in VideoCastConsumerImpl to indicate when a remote call is finished.  Some actions your application performs do not provide a direct callback once the remote player has performed the specified action:

```java
mCastManager.loadMedia(selectedMedia, true, 0);
mCastManager.play();
mCastManager.pause();
mCastManager.stop();
mCastManager.seekAndPlay((int)position);
```

Our SDK provides the option to use listeners when communicating with the remote device, as in the previous section.  This allows you to monitor the result of the call and to handle failure and success situations differently, or with a common failure handler like the one in this example:

```java
private void handleFailure( Throwable throwable, final String msg, final boolean extend ) {
    Log.e(TAG, msg, throwable);
    final String exceptionMessage = throwable.getMessage();

    FlingActivity.this.runOnUiThread(new Runnable() {
        public void run() {
            Toast.makeText(FlingActivity.this, msg+(extend?exceptionMessage:""), Toast.LENGTH_LONG).show();
        }
    });
}

private class ErrorResultHandler  implements FutureListener<Void> {
    private String mMsg;
    private boolean mExtend;

    ErrorResultHandler(String msg) {
        this(msg, false);
    }
    ErrorResultHandler(String msg, Boolean extend) {
        mMsg = msg;
        mExtend = extend;
    }

    @Override
    public void futureIsNow(Future<Void> result) {
        try {
            result.get();
        } catch(ExecutionException e) {
            handleFailure(e.getCause(), mMsg, mExtend);
        } catch(Exception e) {
            handleFailure(e, mMsg, mExtend);
        }
    }
}
...
mCurrentDevice.stop(new ErrorResultHandler("Error Stopping"));
...
```

Or maybe not even handle the asynchronous result at all.

```java
mCurrentDevice.setPositionUpdateInterval(MONITOR_INTERVAL, null);
mCurrentDevice.setMediaSource(name, title, true, false, null);
mCurrentDevice.play(null);
mCurrentDevice.pause(null);
mCurrentDevice.stop(null);
mCurrentDevice.seek(null);
```

{% include links.html %}
