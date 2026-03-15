Flutter - Writing Android Specific Code
Flutter provides a general framework to access platform specific feature. This enables the developer to extend the functionality of the Flutter framework using platform specific code. Platform specific functionality like camera, battery level, browser, etc., can be accessed easily through the framework.

The general idea of accessing the platform specific code is through simple messaging protocol. Flutter code, Client and the platform code and Host binds to a common Message Channel. Client sends message to the Host through the Message Channel. Host listens on the Message Channel, receives the message and does the necessary functionality and finally, returns the result to the Client through Message Channel.

The platform specific code architecture is shown in the block diagram given below −

Specific Code Architecture
The messaging protocol uses a standard message codec (StandardMessageCodec class) that supports binary serialization of JSON-like values such as numbers, strings, boolean, etc., The serialization and de-serialization works transparently between the client and the host.

Let us write a simple application to open a browser using Android SDK and understand how

Create a new Flutter application in Android studio, flutter_browser_app

Replace main.dart code with below code −

import 'package:flutter/material.dart'; 
void main() => runApp(MyApp()); 
class MyApp extends StatelessWidget { 
   @override 
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Flutter Demo', 
         theme: ThemeData( 
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(title: 'Flutter Demo Home Page'),
      );
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
               onPressed: null, 
            ), 
         ), 
      ); 
   }
}
Here, we have created a new button to open the browser and set its onPressed method as null.

Now, import the following packages −

import 'dart:async'; 
import 'package:flutter/services.dart';
Here, services.dart include the functionality to invoke platform specific code.

Create a new message channel in the MyHomePage widget.

static const platform = const 
MethodChannel('flutterapp.tutorialspoint.com/browser');
Write a method, _openBrowser to invoke platform specific method, openBrowser method through message channel.

Future<void> _openBrowser() async { 
   try {
      final int result = await platform.invokeMethod(
         'openBrowser', <String, String>{ 
            'url': "https://flutter.dev" 
         }
      ); 
   } 
   on PlatformException catch (e) { 
      // Unable to open the browser 
      print(e); 
   }
}
Here, we have used platform.invokeMethod to invoke openBrowser (explained in coming steps). openBrowser has an argument, url to open a specific url.

Change the value of onPressed property of the RaisedButton from null to _openBrowser.

onPressed: _openBrowser,
Open MainActivity.java (inside the android folder) and import the required library −

import android.app.Activity; 
import android.content.Intent; 
import android.net.Uri; 
import android.os.Bundle; 

import io.flutter.app.FlutterActivity; 
import io.flutter.plugin.common.MethodCall; 
import io.flutter.plugin.common.MethodChannel; 
import io.flutter.plugin.common.MethodChannel.MethodCallHandler; 
import io.flutter.plugin.common.MethodChannel.Result; 
import io.flutter.plugins.GeneratedPluginRegistrant;
Write a method, openBrowser to open a browser

private void openBrowser(MethodCall call, Result result, String url) { 
   Activity activity = this; 
   if (activity == null) { 
      result.error("ACTIVITY_NOT_AVAILABLE", 
      "Browser cannot be opened without foreground 
      activity", null); 
      return; 
   } 
   Intent intent = new Intent(Intent.ACTION_VIEW); 
   intent.setData(Uri.parse(url)); 
   
   activity.startActivity(intent); 
   result.success((Object) true); 
}
Now, set channel name in the MainActivity class −

private static final String CHANNEL = "flutterapp.tutorialspoint.com/browser";
Write android specific code to set message handling in the onCreate method −

new MethodChannel(getFlutterView(), CHANNEL).setMethodCallHandler( 
   new MethodCallHandler() { 
   @Override 
   public void onMethodCall(MethodCall call, Result result) { 
      String url = call.argument("url"); 
      if (call.method.equals("openBrowser")) {
         openBrowser(call, result, url); 
      } else { 
         result.notImplemented(); 
      } 
   } 
});
Here, we have created a message channel using MethodChannel class and used MethodCallHandler class to handle the message. onMethodCall is the actual method responsible for calling the correct platform specific code by the checking the message. onMethodCall method extracts the url from message and then invokes the openBrowser only when the method call is openBrowser. Otherwise, it returns notImplemented method.

The complete source code of the application is as follows −

main.dart

MainActivity.java

package com.tutorialspoint.flutterapp.flutter_browser_app; 

import android.app.Activity; 
import android.content.Intent; 
import android.net.Uri; 
import android.os.Bundle; 
import io.flutter.app.FlutterActivity; 
import io.flutter.plugin.common.MethodCall; 
import io.flutter.plugin.common.MethodChannel.Result; 
import io.flutter.plugins.GeneratedPluginRegistrant; 

public class MainActivity extends FlutterActivity { 
   private static final String CHANNEL = "flutterapp.tutorialspoint.com/browser"; 
   @Override 
   protected void onCreate(Bundle savedInstanceState) { 
      super.onCreate(savedInstanceState); 
      GeneratedPluginRegistrant.registerWith(this); 
      new MethodChannel(getFlutterView(), CHANNEL).setMethodCallHandler(
         new MethodCallHandler() {
            @Override 
            public void onMethodCall(MethodCall call, Result result) {
               String url = call.argument("url"); 
               if (call.method.equals("openBrowser")) { 
                  openBrowser(call, result, url); 
               } else { 
                  result.notImplemented(); 
               }
            }
         }
      ); 
   }
   private void openBrowser(MethodCall call, Result result, String url) {
      Activity activity = this; if (activity == null) {
         result.error(
            "ACTIVITY_NOT_AVAILABLE", "Browser cannot be opened without foreground activity", null
         ); 
         return; 
      } 
      Intent intent = new Intent(Intent.ACTION_VIEW); 
      intent.setData(Uri.parse(url)); 
      activity.startActivity(intent); 
      result.success((Object) true); 
   }
}
main.dart

import 'package:flutter/material.dart'; 
import 'dart:async'; 
import 'package:flutter/services.dart'; 

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
      ); 
   }
}
class MyHomePage extends StatelessWidget {
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   static const platform = const MethodChannel('flutterapp.tutorialspoint.com/browser'); 
   Future<void> _openBrowser() async {
      try {
         final int result = await platform.invokeMethod('openBrowser', <String, String>{ 
            'url': "https://flutter.dev" 
         });
      }
      on PlatformException catch (e) { 
         // Unable to open the browser print(e); 
      } 
   }
   @override 
   Widget build(BuildContext context) {
      return Scaffold( 
         appBar: AppBar( 
            title: Text(this.title), 
         ), 
         body: Center(
            child: RaisedButton( 
               child: Text('Open Browser'), 
               onPressed: _openBrowser, 
            ), 
         ),
      );
   }
}
Run the application and click the Open Browser button and you can see that the browser is launched. The Browser app - Home page is as shown in the screenshot here −

Flutter Demo Home Page
Productively Build Apps
