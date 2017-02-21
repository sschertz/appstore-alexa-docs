---
title: Integrating Amazon Fling with an Existing iOS App that uses Google Cast
sidebar: fling
product: Fling SDK
permalink: integrating-amazon-fling-with-an-existing-ios-cast-app.html
reviewers: jeffersd
github: true
---

Using our SDK, you can talk to Amazon Fire TV devices in a similar manner as you use Google Cast in your iOS apps to talk to Chromecast devices.  This page outlines steps to modify an existing iOS app that uses the Google Cast framework to also fling to Amazon Fire TV.

Before you start, make sure you follow the steps at [Setting Up your Amazon Fling Development Environment for iOS][setting-up-your-amazon-fling-development-environment-for-ios] to include the required frameworks in your project.  

{% include note.html content="All the code in this document is available in the `CastHelloVideo-ios-master` in the samples folder of the SDK package. This page calls out important code changes you need to make to an app that currently uses Google Cast." %}

## Setup and Initialization

To add this functionality in an app that currently uses Google Cast, add the required headers, and add listeners for player discovery and media status.  

```java
#import <UIKit/UIKit.h>
#import <GoogleCast/GoogleCast.h>
#import <AmazonFling/DiscoveryController.h>
#import <AmazonFling/RemoteMediaPlayer.h>
#import <AmazonFling/MediaPlayerStatus.h>

@interface HGCVViewController : UIViewController<GCKDeviceScannerListener,
                                                 GCKDeviceManagerDelegate,
                                                 GCKMediaControlChannelDelegate,
                                                 UIActionSheetDelegate,
                                                 DiscoveryListener,
                                                 MediaPlayerStatusListener>
```

Start the discovery process when the application or view is loaded for the first time.

```java
- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    // Initialize AmazonFling
    self.controller = [[DiscoveryController alloc] init];
    [self.controller searchPlayerWithId:@"amzn.thin.pl" andListener:self];
    self.devices = [[NSMutableArray alloc] init];
```

Your app receives the results of device discovery in overloaded callbacks.

```java
- (void)deviceDiscovered:(id<RemoteMediaPlayer>)device {
    NSLog(@"deviceDiscovered=%@", [device name]);
    [self.devices addObject:device];
    dispatch_async(dispatch_get_main_queue(), ^{
        [self updateButtonStates];
    });
}

- (void)deviceLost:(id<RemoteMediaPlayer>)device {
    NSLog(@"deviceLost=%@", [device name]);
    [self.devices removeObject:device];
    dispatch_async(dispatch_get_main_queue(), ^{
        [self updateButtonStates];
    });
}

- (void)discoveryFailure {
    NSLog(@"Discovery failure");
}
```

Your app gets media playback status in another callback:

```java
- (void)onStatusChange:(MediaPlayerStatus *)status positionChangedTo:(long long)position {
    NSLog(@"Applciation: onStatusChange Called");
}
```

## Lifecycle Event Handling

Your app should suspend device discovery when the app goes into the background, and resume when it comes to foreground:  

```java
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    NSLog(@"viewDidAppear");
    [self.controller resume];
}

- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    NSLog(@"viewWillDisappear");
    [self.devices removeAllObjects];
    [self.controller close];
}
```

## Presenting Amazon Fire TV Devices to the User

You can get a list of the names of the remote Amazon Fire TV devices and present those names to the user with the `RemoteMediaPlayer` proxy object:

```java
- (void)chooseDevice:(id)sender {
...
    UIActionSheet *sheet =
        [[UIActionSheet alloc] initWithTitle:NSLocalizedString(@"Connect to Device", nil)
                                    delegate:self
                           cancelButtonTitle:nil
                      destructiveButtonTitle:nil
                           otherButtonTitles:nil];

      for (id<RemoteMediaPlayer> device in self.devices) {
          [sheet addButtonWithTitle:[device name]];
      }
```

## Connect to the Player

To connect your app to a player, add a status listener.

```java
- (void)connectToFlingDevice {
    if (self.selectedFlingPlayer == nil)
        return;
    __weak HGCVViewController *selfWeak = self;
    [self.selectedFlingPlayer addStatusListener:self :^(NSException *failure) {
        NSLog(@"Failed to add status listener.  Error:%@", failure.reason);
    } : ^{
        dispatch_async(dispatch_get_main_queue(), ^{
            selfWeak.isConnectedToFlingPlayer = YES;
            [selfWeak updateButtonStates];
        });
    }];
}
```

## Get Media Playback Status on Remote Device

Once your app is connected to a remote player it can perform remote operations, such as fetch the media playback status. Communication with the Amazon Fire TV occurs through RemoteMediaPlayer proxy object. Since the SDK provides asynchronous methods for fluid UI, you may have to use serializing mechanisms in place of the Google Cast API calling paradigms you had previously been using.

```java
- (NSInteger)flingPlaybackStatus : (UIActionSheet*) sheet {
    NSString *friendlyName = [NSString stringWithFormat:NSLocalizedString(@"Casting to %@", nil),
                              [self.selectedFlingPlayer name]];
    sheet.title = friendlyName;

    __block NSString* mediaStatus = nil;
    NSCondition *waitHandle = [NSCondition new];
    [waitHandle lock];
    [self.selectedFlingPlayer getStatus:^(NSException *failure) {
        NSLog(@"Failed to get status.  Error:%@", failure.reason);
        [waitHandle signal];
    } :^(MediaPlayerStatus *result) {
        mediaStatus = [NSString stringWithString:[self mapState:[result getState]]];
        [waitHandle signal];
    }];
    [waitHandle wait];

    if (mediaStatus != nil) {
        [sheet addButtonWithTitle:mediaStatus];
    }
    return mediaStatus == nil ? 0 : 1;
}
```

## Flinging Media to the Player

If the selected device is a player, the controller application can set the media playback source and metadata:

```java
- (void)flingLoadMedia {
    [[self.selectedFlingPlayer setMediaSourceToURL:@"http://videos.acme.com/url_to_video"
                                         metaData:@"{\"title\" : \"Big Buck Bunny (2008)\"}" // meta data in JSON format
                                         autoPlay:YES  // auto-play
                              andPlayInBackground:NO   // play in background
    ] continueWithBlock:^id(BFTask *task) {
        if (task.error) {
            NSLog(@"Failure to set media source - %@", task.error);
        }
        return nil;
    }];
```

{% include links.html %}