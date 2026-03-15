Flutter - Writing IOS Specific Code
Accessing iOS specific code is similar to that on Android platform except that it uses iOS specific languages - Objective-C or Swift and iOS SDK. Otherwise, the concept is same as that of the Android platform.

Let us write the same application as in the previous chapter for iOS platform as well.

Let us create a new application in Android Studio (macOS), flutter_browser_ios_app

Follow steps 2 - 6 as in previous chapter.

Start XCode and click File → Open

Choose the xcode project under ios directory of our flutter project.

Open AppDelegate.m under Runner → Runner path. It contains the following code −

#include "AppDelegate.h" 
#include "GeneratedPluginRegistrant.h" 
@implementation AppDelegate 

- (BOOL)application:(UIApplication *)application
   didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      // [GeneratedPluginRegistrant registerWithRegistry:self];
      // Override point for customization after application launch.
      return [super application:application didFinishLaunchingWithOptions:launchOptions];
   } 
@end
We have added a method, openBrowser to open browser with specified url. It accepts single argument, url.

- (void)openBrowser:(NSString *)urlString { 
   NSURL *url = [NSURL URLWithString:urlString]; 
   UIApplication *application = [UIApplication sharedApplication]; 
   [application openURL:url]; 
}
In didFinishLaunchingWithOptions method, find the controller and set it in controller variable.

FlutterViewController* controller = (FlutterViewController*)self.window.rootViewController;
In didFinishLaunchingWithOptions method, set the browser channel as flutterapp.tutorialspoint.com/browse −

FlutterMethodChannel* browserChannel = [
   FlutterMethodChannel methodChannelWithName:
   @"flutterapp.tutorialspoint.com/browser" binaryMessenger:controller];
Create a variable, weakSelf and set current class −

__weak typeof(self) weakSelf = self;
Now, implement setMethodCallHandler. Call openBrowser by matching call.method. Get url by invoking call.arguments and pass it while calling openBrowser.

[browserChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
   if ([@"openBrowser" isEqualToString:call.method]) { 
      NSString *url = call.arguments[@"url"];   
      [weakSelf openBrowser:url]; 
   } else { result(FlutterMethodNotImplemented); } 
}];
The complete code is as follows −

#include "AppDelegate.h" 
#include "GeneratedPluginRegistrant.h" 
@implementation AppDelegate 

- (BOOL)application:(UIApplication *)application 
   didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
   
   // custom code starts 
   FlutterViewController* controller = (FlutterViewController*)self.window.rootViewController; 
   FlutterMethodChannel* browserChannel = [
      FlutterMethodChannel methodChannelWithName:
      @"flutterapp.tutorialspoint.com /browser" binaryMessenger:controller]; 
   
   __weak typeof(self) weakSelf = self; 
   [browserChannel setMethodCallHandler:^(
      FlutterMethodCall* call, FlutterResult result) { 
      
      if ([@"openBrowser" isEqualToString:call.method]) { 
         NSString *url = call.arguments[@"url"];
         [weakSelf openBrowser:url]; 
      } else { result(FlutterMethodNotImplemented); } 
   }]; 
   // custom code ends 
   [GeneratedPluginRegistrant registerWithRegistry:self]; 
   
   // Override point for customization after application launch. 
   return [super application:application didFinishLaunchingWithOptions:launchOptions]; 
}
- (void)openBrowser:(NSString *)urlString { 
   NSURL *url = [NSURL URLWithString:urlString]; 
   UIApplication *application = [UIApplication sharedApplication]; 
   [application openURL:url]; 
} 
@end
Open project setting.

Go to Capabilities and enable Background Modes.

Add *Background fetch and Remote Notification**.

Now, run the application. It works similar to Android version but the Safari browser will be opened instead of chrome.

