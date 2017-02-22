---
title: Integrating Amazon Fling into your iOS App
sidebar: fling
product: Fling SDK
permalink: integrating-amazon-fling-into-your-ios-app.html
reviewers: jeffersd
github: true
toc: false
---

This service provides an asynchronous discovery and control mechanism for your iOS app in the form of a framework. See [Understanding the Amazon Fling Service][understanding-the-amazon-fling-service] for a high-level overview of the features and functions this service provides. See also the "FlingSample" sample app, part of the SDK, for a sample controller implementation for iOS.  

Before you start, make sure you've set up your development environment. See [Setting Up your Amazon Fling Development Environment for iOS][setting-up-your-amazon-fling-development-environment-for-ios]  for details.

* TOC
{:toc} 

## Initializing and Suspending the Controller App

When your iOS controller app loads, for example in the `ViewController::viewDidLoad` method, call `DiscoveryController::``searchPlayerWithId` to initialize the underlying framework and start the discovery. Call `DiscoveryController::resume` every time the app comes to the foreground, that is, in the `applicationDidBecomeActive` lifecycle event of the `UIApplicationDelegate` implementation.

```objective_c
@implementation AppDelegate
...
- (void)applicationDidBecomeActive:(UIApplication *)application {
    id myViewController = [self findViewController];
    [myViewController resume];
}
...
@implementation ViewController
...
- (void)viewDidLoad {
...
    mController = [[DiscoveryController alloc] init];
    [mController searchPlayerWithId : @"amzn.thin.pl" andListener : self];
}

- (void)resume {
    [mController resume];
}
```

The debug logging is disabled by default. To enable the debug logging add the parameter `andEnableLogs` set to `YES` when calling `searchPlayerWithId`. Once the underlying framework is initialized, other operations such as player discovery and media playback and control are possible.

When the controller app is to be suspended such as when the user locks the screen, your app must also suspend the underlying framework. Call `DiscoveryController::close` in the `applicationWillResignActive` lifecycle event of the `UIApplicationDelegate` implementation to suspend the underlying framework.  

```objective_c
@implementation AppDelegate
...
- (void) applicationWillResignActive:(UIApplication *)application {
    id myViewController = [self findViewController];
    [myViewController suspend];
}
...
@implementation ViewController
...
- (void)suspend {
    // Make sure that MediaPlayerStatusListener is removed before suspending.
    if (mCurrentPlayer != nil) {
        [mCurrentPlayer removeStatusListener:self]
    }
    [mController close];
}
```

## Implementing Discovery Callbacks

As part of your app's initialization, the component that creates the `DiscoveryController` instance should also specify the callback receiver. This object implements the `DiscoveryListener` interface. The object is passed to the underlying framework in the `DiscoveryController::``searchPlayerWithId` call. Discovery is an ongoing process as long as the `DiscoveryController` object is live.

```objective_c
@interface ViewController : UIViewController<DiscoveryListener>
...
@implementation ViewController
...
- (void)deviceDiscovered:(id<RemoteMediaPlayer>)device {
    [mDevices addObject:device];
    // Update UI
}

- (void)deviceLost:(id<RemoteMediaPlayer>)device {
    [mDevices removeObject:device];
    // Update UI
}

- (void)discoveryFailure {
}
```

The remote players are represented as `RemoteMediaPlayer` objects. Through this interface, your controller app can control the media playback on the remote player.

## Presenting Remote Players to the User

Once the players in the vicinity are discovered, the controller app should present the friendly names of to the user.

```objective_c
- (NSString *)pickerView:(UIPickerView *)pickerView
             titleForRow:(NSInteger)row
            forComponent:(NSInteger)component {
    if (pickerView == devicePickerView ) {
        if (row < [mDevices count]) {
            id<RemoteMediaPlayer> dev = [mDevices objectAtIndex:row];
            return [dev name];
        }
    }
    return nil;
}
```

## Working with BFTask

`BFTask` is a class in the Bolts mobile development framework. Bolts makes it easier to work with asynchronous operations without blocking the UI thread. Our SDK for iOS uses Bolts to handle asynchronous tasks.

A `BFTask` object represents the result of an asynchronous method. Using `BFTask`, you can wait for an asynchronous method to return a value, and then do something with that value after it has returned. When you are working with our SDK for iOS, it is important to remember that methods that return `BFTask` are asynchronous.

Every `BFTask` has a method called `continueWithBlock:` that takes a continuation block. A continuation is a block that will be executed when the task is complete. In many cases, you only want to do more work if the previous task was successful, and propagate any errors to be dealt with later. To do this, use the `continueWithSuccessBlock:` method instead of `continueWithBlock:`. Note that you must return either `BFTask` or nil in every `continueWithBlock:` and `continueWithSucessBlock:`.

For complete documentation on Bolts, see [Bolts-iOS](https://github.com/BoltsFramework/Bolts-iOS "Bolts-iOS") on GitHub. Clicking on this link will send you outside the Amazon Developer Portal.

## Connecting to and Disconnecting from a Remote Player

After the user picks the player from the list of available players on Fire TV devices, your controller app is ready to interact with that remote player. To implement this interaction, the app needs to know the current player status and listen for changes to that status. Status updates are presented to the controller app in the form of callbacks.

Your callback receiver implements the `MediaPlayerStatusListener` interface. This callback provides the current status of the player in an `MediaPlayerStatus` object. The current state is returned by the `state` selector, and the `condition` selector returns extra information about the state.

```objective_c
@interface ViewController : UIViewController<MediaPlayerStatusListener>
...
@implementation ViewController
...
- (void)onStatusChange:(MediaPlayerStatus *)status positionChangedTo:(long long)position {
    // Update UI with status
}
```

To receive the callbacks the app must add a status update listener object to the remote player object. Depending on the logic, the app should disconnect from the last player.

```objective_c
-(void)connectToPlayer:(id<RemoteMediaPlayer>)device {
    if (mCurrentPlayer == nil) {
        mCurrentPlayer = device;
    } else if (device != mCurrentPlayer) {
        [[mCurrentPlayer removeStatusListener:self] continueWithBlock:^id(BFTask *task) {
            if (task.error) {
                // Indicate error
            }
            mCurrentPlayer = device;
            return nil;
        }];
    }
    [[mCurrentPlayer addStatusListener:self] continueWithBlock:^id(BFTask *task) {
        if (task.error) {
            // Indicate error
        } else {
            // UI update to show connection establishment
        }
        return nil;
    }];
}
```

You should disconnect from the current player when the status updates are not needed.

```objective_c
-(void)disconnectFromCurrentPlayer {
    [[mCurrentPlayer removeStatusListener : self] continueWithBlock:^id(BFTask *task) {
        if (task.error) {
            // Indicate error
        }
        return nil;
    }];
}
```

## Controlling Playback

Once the player connection is established, your controller app can play and control media playback on that player. The `RemoteMediaPlayer` object provides local methods to control the playback and volume. All the control actions are asynchronous and your controller app should provide success and failure handler callbacks to each of the control methods if desired. Please note that these callbacks are not on the UI thread and the app must dispatch any UI updates on the main thread.

Examples of control actions are below:

```objective_c
- (void)playNewMedia : (NSString*)metadata : (NSString*)url {
     [[mCurrentPlayer setMediaSourceToURL : url
                                 metaData : metadata
                                 autoPlay : YES // start playback immediately
                      andPlayInBackground : NO  // play in foreground
    ] continueWithBlock:^id(BFTask *task) {
        if (task.error) {
            // Indicate error
        }
        return nil;
    }];
}
- (IBAction)goBack10s : (id)sender {
    [[[mCurrentPlayer getPosition] continuteWithSuccessBlock:^id(BFTask *task) {
        long long toPos = [task.result longlongValue];
        toPos -= 10000;
        if( toPos < 0 ) {
            toPos = 0;
        }
        return [mCurrentPlayer seekToPosition:toPos andMode:ABSOLUTE];
    }] continueWithBlock:^id(BFTask *task) {
        if (task.error) {
            // Indicate error
        }
        return nil;
    }];
}
```

## Next Steps

To create a custom player for Fire TV, see [Integrating Amazon Fling into your Fire TV Application][integrating-amazon-fling-into-your-fire-tv-app].

If you are modifying an existing Google Cast app on iOS to support flinging to Amazon Fire TV, see [Integrating Amazon Fling with an Existing iOS Cast App][integrating-amazon-fling-with-an-existing-ios-cast-app].

Please refer to [Designing Amazon Fling UX][designing-amazon-fling-ux] for guidelines on how to design the user interface of your app.

{% include links.html %}
