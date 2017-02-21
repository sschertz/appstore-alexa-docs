---
title: Using Amazon Fling with Android MediaRouter
sidebar: fling
product: Fling SDK
permalink: using-amazon-fling-with-android-mediarouter.html
reviewers: jeffersd
github: true
---

The [MediaRouter](https://developer.android.com/guide/topics/media/mediarouter.html) framework in Android enables your app to direct the output of media playback to a specific device. The API included in the SDK provides a FlingMediaRouteProvider class for controller apps, which makes it easy to integrate this functionality into your MediaRouter-based app.  

## Integrating FlingMediaRouteProvider

The `FlingMediaRouteProvider` class can be integrated into your app by adding the following lines to your existing activity:

```java
import com.amazon.whisperplay.fling.provider.FlingMediaRouteProvider;
...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        MediaRouter.getInstance(this).addProvider(new FlingMediaRouteProvider(this, "acme.fling.player"));
        ...
    }
```

MediaRouter provides routes to Amazon Fire TV apps implementing the service ID you provide. Change the service ID in the example above (acme.fling.player) to the service ID of your Amazon Fire TV application. The `FlingMediaRouteProvider` class provides all of the functionality accessible through [`RemotePlaybackClient`,](https://developer.android.com/reference/android/support/v7/media/RemotePlaybackClient.html) except queuing. Applications sending control intents directly to the route instead of using a `RemotePlaybackClient` support these standard [`MediaControlIntent`](https://developer.android.com/reference/android/support/v7/media/MediaControlIntent.html) intents:

*  `MediaControlIntent.ACTION_PLAY`
*  `MediaControlIntent.ACTION_SEEK`
*  `MediaControlIntent.ACTION_GET_STATUS`
*  `MediaControlIntent.ACTION_PAUSE`
*  `MediaControlIntent.ACTION_RESUME`
*  `MediaControlIntent.ACTION_STOP`
*  `MediaControlIntent.ACTION_START_SESSION`
*  `MediaControlIntent.ACTION_GET_SESSION_STATUS`
*  `MediaControlIntent.ACTION_END_SESSION`

In addition, these intents provide additional functionality not provided by the default intents. More information on required intent extras and the extras returned from these intents can be found in the API documentation provided with the SDK (Javadocs).

*  `FlingMediaControlIntent.ACTION_MUTE`
*  `FlingMediaControlIntent.ACTION_UNMUTE`
*  `FlingMediaControlIntent.ACTION_GET_IS_MUTE`
*  `FlingMediaControlIntent.ACTION_GET_MEDIA_INFO`
*  `FlingMediaControlIntent.ACTION_SEND_COMMAND`
*  `FlingMediaControlIntent.ACTION_SET_PLAYER_STYLE`
*  `FlingMediaControlIntent.ACTION_GET_IS_MIME_TYPE_SUPPORTED`

## Discovering Media Devices With MediaRouter

MediaRouter can be used to control device discovery on your controller app. Obtain a `MediaRouter` object with `getInstance()`:

```java
    private MediaRouter mMediaRouter;
    ...
        mMediaRouter = MediaRouter.getInstance(this);
```

After you get an instance of `MediaRouter`, add the provider, as documented in the previous section. Then, create a `MediaRouteSelector` and a `MediaRouter.Callback` to handle discovering devices, and then add them to `MediaRouter`.

{% include note.html content="The `FlingMediaRouteProvider` only provides routes if you are using the control category `MediaControlIntent.CATEGORY_REMOTE_PLAYBACK` when creating your `MediaRouteSelector`. The code sample below uses the flag `MediaRouter.CALLBACK_FLAG_PERMFORM_ACTIVE_SCAN` to continuously look for devices while the callback is registered." %}

```java
    private MediaRouter.Callback mMediaRouterCallback = new MediaRouter.Callback() {
        public void onRouteAdded(MediaRouter router, MediaRouter.RouteInfo route) {
            //your code for handling when a new route is discovered here
        }

        public void onRouteRemoved(MediaRouter router, MediaRouter.RouteInfo route) {
            //your code for handling when a route is no longer available
        }
    };
    ...
        mMediaRouter.addProvider(new FlingMediaRouteProvider(this, "acme.fling.player"));
        MediaRouteSelector mediaRouteSelector = new MediaRouteSelector.Builder()
                .addControlCategory(MediaControlIntent.CATEGORY_REMOTE_PLAYBACK)
                .build();
        mMediaRouter.addCallback(mediaRouteSelector, mMediaRouterCallback,
                MediaRouter.CALLBACK_FLAG_PERFORM_ACTIVE_SCAN);
```

When device routes have been discovered your app can interact with those routes with a RemotePlaybackClient object, or with playback control requests.  Volume control is covered in the next section.

## Using RemotePlaybackClient To Fling Content

Initialize a `RemotePlaybackClient` object with the route provided by `MediaRouter`. In this example, the `mCurrentRoute` variable is a `MediaRouter.RouteInfo` instance provided by the `onRouteAdded()` method in your `MediaRouter.Callback`.

```java
    private MediaRouter.RouteInfo mCurrentRoute;
    private RemotePlaybackClient mClient;
    ...
        mCurrentRoute.select();
        mClient = new RemotePlaybackClient(getApplicationContext(), mCurrentRoute);
```

Once the `RemotePlaybackClient` object is created your app can communicate with the remote device. Any calls to your remote device must include an asynchronous callback, which is called when the provider has received a response and has finished processing.

The callback provides some variables such as `sessionId`, and a `Bundle` with data. See the [`RemotePlaybackClient`](https://developer.android.com/reference/android/support/v7/media/RemotePlaybackClient.html) and [`MediaControlIntent`](https://developer.android.com/reference/android/support/v7/media/MediaControlIntent.html) classes for details on the data provided by each call and for additional methods.

```java
mClient.startSession(null, new RemotePlaybackClient.SessionActionCallback() {
    @Override
    public void onResult(Bundle data, String sessionId, MediaSessionStatus sessionStatus) {
        //code handling a successful result here
    }

    @Override
    public void onError(String error, int code, Bundle data) {
        //code handling failure here
    }
});

Uri mUri = "http://www.leanbackplayer.com/videos/360p/elephants_dream_640x360_2.30.mp4";

//build the metadata bundle that includes closed captioning information
Bundle metadataBundle = new Bundle();
metadataBundle.putString(MediaItemMetadata.KEY_TITLE, "Elephants Dream");
metadataBundle.putString(MediaItemMetadata.KEY_ARTWORK_URI, "http://en.wikipedia.org/wiki/Elephants_Dream#mediaviewer/File:Elephants_Dream_s5_both.jpg");
metadataBundle.putString(FireTVBuiltInReceiverMetadata.KEY_TYPE, "video/mp4");
metadataBundle.putString(FireTVBuiltInReceiverMetadata.KEY_DESCRIPTION, "Elephants Dream is the world?s first open movie, made entirely with open source graphics software such as Blender, and with all production files freely available to use however you please, under a Creative Commons license.");
metadataBundle.putString(FireTVBuiltInReceiverMetadata.KEY_NO_REPLAY, "false");
ArrayList<Bundle> ccTracks = new ArrayList<>();
Bundle track = new Bundle();
track.putString(FireTVBuiltInReceiverMetadata.KEY_TRACK_SOURCE, "http://www.leanbackplayer.com/subtitles/elephants_dream/english_en.vtt");
track.putString(FireTVBuiltInReceiverMetadata.KEY_TRACK_KIND, "subtitles");
track.putString(FireTVBuiltInReceiverMetadata.KEY_TRACK_SOURCE_LANGUAGE, "en-US");
track.putString(FireTVBuiltInReceiverMetadata.KEY_TRACK_LABEL, "English");
ccTracks.add(track);  metadataBundle.putParcelableArrayList(FireTVBuiltInReceiverMetadata.KEY_TRACKS, tracks);

String mItemID;
mClient.play(mUri, "video/mp4", metadataBundle, 0, null, new RemotePlaybackClient.ItemActionCallback() {
	@Override
	public void onResult(Bundle data, String sessionId, MediaSessionStatus sessionStatus, String itemId, MediaItemStatus itemStatus) {
		mItemID = itemId;
	}
});

mClient.pause(null, new RemotePlaybackClient.SessionActionCallback() {});
mClient.resume(null, new RemotePlaybackClient.SessionActionCallback() {});
mClient.stop(null, new RemotePlaybackClient.SessionActionCallback() {});
//seek to 10 seconds from the beginning
int mPosition = 10000;
mClient.seek(mItemID, mPosition, null, new RemotePlaybackClient.ItemActionCallback() {});
mClient.endSession(null, new RemotePlaybackClient.SessionActionCallback() {});
```

## Using sendControlRequest() To Fling Content

Once you obtain a `MediaRouter.RouteInfo` object from `MediaRouter.Callback` you can call the `sendControlRequest()` method directly. To use this method, build an `Intent` for the command you wish to send, and include a `MediaRouter.ControlRequestCallback` object.

The callback is called when your request has received a response and has finished processing, and returns a `Bundle` containing the data you requested. The bundle data and additional actions not listed here are described at [`MediaControlIntent`](https://developer.android.com/reference/android/support/v7/media/MediaControlIntent.html).

Implementing fling with the `sendControlRequest()` method requires more work than using `RemotePlaybackClient`, as it requires you to manage your session ID and create intents for every request.

```java
String mSessionID;
String mItemID;
//Start Session
Intent intent = new Intent(MediaControlIntent.ACTION_START_SESSION);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {
    @Override
    public void onResult(Bundle data) {
        mSessionID = data.getString(MediaControlIntent.EXTRA_SESSION_ID);
    }
});
//Play
Uri mUri = "http://distribution.bbb3d.renderfarming.net/video/mp4/bbb_sunflower_1080p_30fps_normal.mp4";
Intent intent = new Intent(MediaControlIntent.ACTION_PLAY).setDataAndType(mUri, "video/mp4");
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {
    @Override
    public void onResult(Bundle data) {
        mSessionID = data.getString(MediaControlIntent.EXTRA_SESSION_ID);
        mItemID = data.getString(MediaControlIntent.EXTRA_ITEM_ID);
    }
});
//Pause
Intent intent = new Intent(MediaControlIntent.ACTION_PAUSE);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
//Resume
Intent intent = new Intent(MediaControlIntent.ACTION_RESUME);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
//Stop
Intent intent = new Intent(MediaControlIntent.ACTION_STOP);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
//Seek
Intent intent = new Intent(MediaControlIntent.ACTION_SEEK);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
intent.putExtra(MediaControlIntent.EXTRA_ITEM_ID, mItemID);
intent.putExtra(MediaControlIntent.EXTRA_ITEM_CONTENT_POSITION, 10000); //10 seconds into the video
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
//End Session
Intent intent = new Intent(MediaControlIntent.ACTION_END_SESSION);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
```

## Getting Status Updates

Status updates from the `MediaRouter.RouteInfo` object can be obtained in two ways: from direct requests, or by setting up a listener. Direct requests work similarly to what was described in the previous section.

```java
mCurrentRoute.getStatus(mItemID, null, new RemotePlaybackClient.ItemActionCallback() {
    @Override
    public void onResult(final Bundle data, String sessionId, MediaSessionStatus sessionStatus, String itemId, MediaItemStatus itemStatus) {
        //handle status here
    }
});

Intent intent = new Intent(MediaControlIntent.ACTION_GET_STATUS);
intent.putExtra(MediaControlIntent.EXTRA_SESSION_ID, mSessionID);
intent.putExtra(MediaControlIntent.EXTRA_ITEM_ID, mItemID);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {
    @Override
    public void onResult(Bundle data) {
        //get status from bundle and use it here
    }
});
```

You can add a listener when starting the session or playing content when using `sendControlRequest()`. If you're using `RemotePlaybackClient` the listener can be added with the `setStatusCallback()` method. Set the listener before the session is created or any play commands are issued.

```java
//using RemotePlaybackClient
mClient.setStatusCallback(new RemotePlaybackClient.StatusCallback() {
    @Override
    public void onItemStatusChanged(Bundle data,
                String sessionId, MediaSessionStatus sessionStatus,
                String itemId, MediaItemStatus itemStatus) {
        //update status here
    }
    @Override
    public void onSessionStatusChanged(Bundle data,
                String sessionId, MediaSessionStatus sessionStatus) {
        //update status here
    }
    @Override
    public void onSessionChanged(String sessionId) {
        //update status here
    }
});

//using sendControlRequest
String ACTION_RECEIVE_MEDIA_STATUS_UPDATE = "acme.fling.player.ACTION_RECEIVE_MEDIA_STATUS_UPDATE";
mSessionStatusUpdateIntent = PendingIntent.getBroadcast(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);
mMediaStatusBroadcastReceiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
        //handle status update here
    }
};
mMediaStatusBroadcastIntentFilter = new IntentFilter(ACTION_RECEIVE_MEDIA_STATUS_UPDATE);
intent = new Intent(ACTION_RECEIVE_MEDIA_STATUS_UPDATE);
intent.setComponent(getCallingActivity());
mMediaStatusUpdateIntent = PendingIntent.getBroadcast(this, 0, intent,PendingIntent.FLAG_UPDATE_CURRENT);
...
Intent intent = new Intent(MediaControlIntent.ACTION_START_SESSION);
intent.putExtra(MediaControlIntent.EXTRA_ITEM_STATUS_UPDATE_RECEIVER, mMediaStatusUpdateIntent);
mCurrentRoute.sendControlRequest(intent, new MediaRouter.ControlRequestCallback() {});
```

{% include note.html content="There is a bug in the `MediaRouteProvider` object used by Cast devices that causes `onItemStatusChanged()` in `RemotePlaybackClient.StatusCallback` to never be called due to a missing session ID. This method receives updates properly for `FlingMediaRouteProvider`. `FlingMediaRouteProvider` also triggers the item status update every time the media position changes (maximum 1 call per second), although this may not be the case for all `MediaRouteProvider` instances." %}

{% include links.html %}
