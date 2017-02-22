---
title: Integrating Remote Install into Your iOS App
sidebar: fling
product: Fling SDK
permalink: ios-remote-install.html
reviewers: jeffersd
github: true
toc: false
---

This service provides an asynchronous discovery and control mechanism for your iOS app in the form of a framework. See [Understanding the Amazon Fling Service][understanding-the-amazon-fling-service] for a high-level overview of the features and functions this service provides. See also the remote install sample app, part of the SDK, for a sample implementation for iOS.  

Before you start, make sure you've set up your development environment. See [Setting Up your Amazon Fling Development Environment for iOS][setting-up-your-amazon-fling-development-environment-for-ios]  for details.

* TOC
{:toc}

## Integration Overview

There are three steps for creating or modifying your controller mobile app to support Remote Install:

1.  Initializing underlying framework and discovering available Fire TV devices.
2.  For a selected Fire TV device, add logic to check if your app is installed on it.
3.  Add UI flow to allow the user to initiate install of your Fire TV app. 

## Initializing and Suspending the App

When your iOS app loads, for example in the `ViewController::viewDidLoad` method, call `InstallDiscoveryController::searchInstallServiceWithListener` to initialize the underlying framework and start discovery. Call `InstallDiscoveryController::resume` every time the app comes to the foreground, that is, in the `applicationDidBecomeActive` lifecycle event of the `UIApplicationDelegate` implementation.

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
    mController = [[InstallDiscoveryController alloc] init];
    [mController searchInstallServiceWithListener : self];
}

- (void)resume {
    [mController resume];
}
```

The debug logging is disabled by default. To enable the debug logging add the parameter `andEnableLogs` set to `YES` when calling `searchInstallServiceWithListener`. Once the underlying framework is initialized, other operations such as install service discovery are possible.

When the controller app is to be suspended such as when the user locks the screen, your app must also suspend the underlying framework. Call `InstallDiscoveryController::close` in the `applicationWillResignActive` lifecycle event of the `UIApplicationDelegate` implementation to suspend the underlying framework.  

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
    [mController close];
}
```

## Implementing Discovery Callbacks

As part of your app's initialization, the component that creates the `InstallDiscoveryController` instance should also specify the callback receiver. This object implements the `InstallDiscoveryListener` interface. The object is passed to the underlying framework in the `InstallDiscoveryController::searchInstallServiceWithListener` call. Discovery is an ongoing process as long as the `InstallDiscoveryController` object is live.

```objective_c
@interface ViewController : UIViewController<InstallDiscoveryListener>
...
@implementation ViewController
...
- (void)installServiceDiscovered:(id<RemoteInstallService>)device {
    [mDevices addObject:device];
    // Update UI
}

- (void)installServiceLost:(id<RemoteInstallService>)device {
    [mDevices removeObject:device];
    // Update UI
}

- (void)discoveryFailure {
}
```

The remote install services are represented as `RemoteInstallService` objects. Through this interface, your app can install applications on the remote device.

## Presenting the Remote Install Service to the User

Once the players in the vicinity are discovered, the controller app should present the friendly names of to the user.

```objective_c
- (NSString *)pickerView:(UIPickerView *)pickerView
             titleForRow:(NSInteger)row
            forComponent:(NSInteger)component {
    if (pickerView == devicePickerView ) {
        if (row < [mDevices count]) {
            id<RemoteInstallService> dev = [mDevices objectAtIndex:row];
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

## Installing

Once the install service is discovered, your app can install applications on that device. The `RemoteInstallService` object provides local methods to check the version of an installed app or install an app. All the control actions are asynchronous and your app should provide success and failure handler callbacks to each of the control methods if desired. Please note that these callbacks are not on the UI thread and the app must dispatch any UI updates on the main thread.

Your app can check if the desired application is installed on the selected Amazon Fire TV by calling `getInstalledPackageVersion` API. If the app is not installed, `RIS_PACKAGE_NOT_INSTALLED` string will be returned. Once your app has called the `installByASIN` method the Fire TV will launch the Appstore page associated with that ASIN (Amazon Standard Identification Number).

The user will then need to finish the installation with the a Fire TV remote. If the ASIN is invalid the Fire TV will show an error popup. The ASIN of an application can be found on its product information page on [amazon.com](http://www.amazon.com "Amazon") under the Product Details section. Some applications could have different products for different regions and therefore could have multiple ASINs.

Examples of control actions are below:

```objective_c
- (void)logInstalledVersion : (NSString*)packageName {
    BFTask task = [mCurrentInstallService getInstalledPackageVersion : packageName];
    [task waitUntilFinished];
    if (task.error == nil) {
        if ([(NSString*)task.result compare : RIS_PACKAGE_NOT_INSTALLED] == 0) {
            NSLog(@"Package not installed");
        } else {
            NSLog(@"Received version is %@", (NSString*)task.result);
        }
    } else {
        // Indicate error
    }
}

- (void)installPackage : (NSString*)asin {
    BFTask* task = [mCurrentInstallService installByASIN : asin];
    [task waitUntilFinished];
    if (task.error != nil) {
        // Indicate error
    }
}
```

{% include links.html %}
