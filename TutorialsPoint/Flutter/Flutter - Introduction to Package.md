Flutter - Introduction to Package
Darts way of organizing and sharing a set of functionality is through Package. Dart Package is simply sharable libraries or modules. In general, the Dart Package is same as that of Dart Application except Dart Package does not have application entry point, main.

The general structure of Package (consider a demo package, my_demo_package) is as below −

lib/src/* − Private Dart code files.

lib/my_demo_package.dart − Main Dart code file. It can be imported into an application as −

import 'package:my_demo_package/my_demo_package.dart'
Other private code file may be exported into the main code file (my_demo_package.dart), if necessary as shown below −

export src/my_private_code.dart
lib/* − Any number of Dart code files arranged in any custom folder structure. The code can be accessed as,

import 'package:my_demo_package/custom_folder/custom_file.dart'
pubspec.yaml − Project specification, same as that of application,

All Dart code files in the Package are simply Dart classes and it does not have any special requirement for a Dart code to include it in a Package.

Types of Packages
Since Dart Packages are basically a small collection of similar functionality, it can be categorized based on its functionality.

Dart Package
Generic Dart code, which can be used in both web and mobile environment. For example, english_words is one such package which contains around 5000 words and has basic utility functions like nouns (list nouns in the English), syllables (specify number of syllables in a word.

Flutter Package
Generic Dart code, which depends on Flutter framework and can be used only in mobile environment. For example, fluro is a custom router for flutter. It depends on the Flutter framework.

Flutter Plugin
Generic Dart code, which depends on Flutter framework as well as the underlying platform code (Android SDK or iOS SDK). For example, camera is a plugin to interact with device camera. It depends on the Flutter framework as well as the underlying framework to get access to camera.

Using a Dart Package
Dart Packages are hosted and published into the live server, https://pub.dartlang.org. Also, Flutter provides simple tool, pub to manage Dart Packages in the application. The steps needed to use as Package is as follows −

Include the package name and the version needed into the pubspec.yaml as shown below −

dependencies: english_words: ^3.1.5
The latest version number can be found by checking the online server.

Install the package into the application by using the following command −

flutter packages get
While developing in the Android studio, Android Studio detects any change in the pubspec.yaml and displays an Android studio package alert to the developer as shown below −

Package Alert
Dart Packages can be installed or updated in Android Studio using the menu options.

Import the necessary file using the command shown below and start working −

import 'package:english_words/english_words.dart';
Use any method available in the package,

nouns.take(50).forEach(print);
Here, we have used nouns function to get and print the top 50 words.

Develop a Flutter Plugin Package
Developing a Flutter Plugin is similar to developing a Dart application or Dart Package. The only exception is that the plugin is going to use System API (Android or iOS) to get the required platform specific functionality.

As we have already learned how to access platform code in the previous chapters, let us develop a simple plugin, my_browser to understand the plugin development process. The functionality of the my_browser plugin is to allow the application to open the given website in the platform specific browser.

Start Android Studio.

Click File → New Flutter Project and select Flutter Plugin option.

You can see a Flutter plugin selection window as shown here −

Flutter Plugin
Enter my_browser as project name and click Next.

Enter the plugin name and other details in the window as shown here −

Configure New Flutter Plugin
Enter company domain, flutterplugins.tutorialspoint.com in the window shown below and then click on Finish. It will generate a startup code to develop our new plugin.

Package Name
Open my_browser.dart file and write a method, openBrowser to invoke platform specific openBrowser method.

Future<void> openBrowser(String urlString) async { 
   try {
      final int result = await _channel.invokeMethod(
         'openBrowser', <String, String>{ 'url': urlString }
      );
   }
   on PlatformException catch (e) { 
      // Unable to open the browser print(e); 
   } 
}
Open MyBrowserPlugin.java file and import the following classes −

import android.app.Activity; 
import android.content.Intent; 
import android.net.Uri; 
import android.os.Bundle;
Here, we have to import library required for opening a browser from Android.

Add new private variable mRegistrar of type Registrar in MyBrowserPlugin class.

private final Registrar mRegistrar;
Here, Registrar is used to get context information of the invoking code.

Add a constructor to set Registrar in MyBrowserPlugin class.

private MyBrowserPlugin(Registrar registrar) { 
   this.mRegistrar = registrar; 
}
Change registerWith to include our new constructor in MyBrowserPlugin class.

public static void registerWith(Registrar registrar) { 
   final MethodChannel channel = new MethodChannel(registrar.messenger(), "my_browser"); 
   MyBrowserPlugin instance = new MyBrowserPlugin(registrar); 
   channel.setMethodCallHandler(instance); 
}
Change the onMethodCall to include openBrowser method in MyBrowserPlugin class.

@Override 
public void onMethodCall(MethodCall call, Result result) { 
   String url = call.argument("url");
   if (call.method.equals("getPlatformVersion")) { 
      result.success("Android " + android.os.Build.VERSION.RELEASE); 
   } 
   else if (call.method.equals("openBrowser")) { 
      openBrowser(call, result, url); 
   } else { 
      result.notImplemented(); 
   } 
}
Write the platform specific openBrowser method to access browser in MyBrowserPlugin class.

private void openBrowser(MethodCall call, Result result, String url) { 
   Activity activity = mRegistrar.activity(); 
   if (activity == null) {
      result.error("ACTIVITY_NOT_AVAILABLE", 
      "Browser cannot be opened without foreground activity", null); 
      return; 
   } 
   Intent intent = new Intent(Intent.ACTION_VIEW); 
   intent.setData(Uri.parse(url)); 
   activity.startActivity(intent); 
   result.success((Object) true); 
}
The complete source code of the my_browser plugin is as follows −

my_browser.dart

import 'dart:async'; 
import 'package:flutter/services.dart'; 

class MyBrowser {
   static const MethodChannel _channel = const MethodChannel('my_browser'); 
   static Future<String> get platformVersion async { 
      final String version = await _channel.invokeMethod('getPlatformVersion'); return version; 
   } 
   Future<void> openBrowser(String urlString) async { 
      try {
         final int result = await _channel.invokeMethod(
            'openBrowser', <String, String>{'url': urlString}); 
      } 
      on PlatformException catch (e) { 
         // Unable to open the browser print(e); 
      }
   }
}
MyBrowserPlugin.java

package com.tutorialspoint.flutterplugins.my_browser; 

import io.flutter.plugin.common.MethodCall; 
import io.flutter.plugin.common.MethodChannel; 
import io.flutter.plugin.common.MethodChannel.MethodCallHandler; 
import io.flutter.plugin.common.MethodChannel.Result; 
import io.flutter.plugin.common.PluginRegistry.Registrar; 
import android.app.Activity; 
import android.content.Intent; 
import android.net.Uri; 
import android.os.Bundle; 

/** MyBrowserPlugin */ 
public class MyBrowserPlugin implements MethodCallHandler {
   private final Registrar mRegistrar; 
   private MyBrowserPlugin(Registrar registrar) { 
      this.mRegistrar = registrar; 
   } 
   /** Plugin registration. */
   public static void registerWith(Registrar registrar) {
      final MethodChannel channel = new MethodChannel(
         registrar.messenger(), "my_browser"); 
      MyBrowserPlugin instance = new MyBrowserPlugin(registrar); 
      channel.setMethodCallHandler(instance); 
   } 
   @Override 
   public void onMethodCall(MethodCall call, Result result) { 
      String url = call.argument("url"); 
      if (call.method.equals("getPlatformVersion")) { 
         result.success("Android " + android.os.Build.VERSION.RELEASE); 
      } 
      else if (call.method.equals("openBrowser")) { 
         openBrowser(call, result, url); 
      } else { 
         result.notImplemented(); 
      } 
   } 
   private void openBrowser(MethodCall call, Result result, String url) { 
      Activity activity = mRegistrar.activity(); 
      if (activity == null) {
         result.error("ACTIVITY_NOT_AVAILABLE",
            "Browser cannot be opened without foreground activity", null); 
         return; 
      }
      Intent intent = new Intent(Intent.ACTION_VIEW); 
      intent.setData(Uri.parse(url)); 
      activity.startActivity(intent); 
      result.success((Object) true); 
   } 
}
Create a new project, my_browser_plugin_test to test our newly created plugin.

Open pubspec.yaml and set my_browser as a plugin dependency.

dependencies: 
   flutter: 
      sdk: flutter 
   my_browser: 
      path: ../my_browser
Android studio will alert that the pubspec.yaml is updated as shown in the Android studio package alert given below −

Android Studio Package Alert
Click Get dependencies option. Android studio will get the package from Internet and properly configure it for the application.

Open main.dart and include my_browser plugin as below −

import 'package:my_browser/my_browser.dart';
Call the openBrowser function from my_browser plugin as shown below −

onPressed: () => MyBrowser().openBrowser("https://flutter.dev"),
The complete code of the main.dart is as follows −

import 'package:flutter/material.dart'; 
import 'package:my_browser/my_browser.dart'; 

void main() => runApp(MyApp()); 

class MyApp extends StatelessWidget { 
   @override 
   Widget build(BuildContext context) {
      return MaterialApp( 
         title: 'Flutter Demo', 
         theme: ThemeData( 
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(
            title: 'Flutter Demo Home Page'
         ), 
      );,
   }
} 
class MyHomePage extends StatelessWidget { 
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar( 
            title: Text(this.title), 
         ), 
         body: Center(
            child: RaisedButton(
               child: Text('Open Browser'), 
               onPressed: () => MyBrowser().openBrowser("https://flutter.dev"), 
            ),
         ), 
      ); 
   }
}
Run the application and click the Open Browser button and see that the browser is launched. You can see a Browser app - Home page as shown in the screenshot shown below −

Open Browser
You can see a Browser app Browser screen as shown in the screenshot shown below −

Flutter Infrastructure
