Flutter - Introduction to Gestures
Gestures are primarily a way for a user to interact with a mobile (or any touch based device) application. Gestures are generally defined as any physical action / movement of a user in the intention of activating a specific control of the mobile device. Gestures are as simple as tapping the screen of the mobile device to more complex actions used in gaming applications.

Some of the widely used gestures are mentioned here −

Tap − Touching the surface of the device with fingertip for a short period and then releasing the fingertip.

Double Tap − Tapping twice in a short time.

Drag − Touching the surface of the device with fingertip and then moving the fingertip in a steady manner and then finally releasing the fingertip.

Flick − Similar to dragging, but doing it in a speeder way.

Pinch − Pinching the surface of the device using two fingers.

Spread/Zoom − Opposite of pinching.

Panning − Touching the surface of the device with fingertip and moving it in any direction without releasing the fingertip.

Flutter provides an excellent support for all type of gestures through its exclusive widget, GestureDetector. GestureDetector is a non-visual widget primarily used for detecting the users gesture. To identify a gesture targeted on a widget, the widget can be placed inside GestureDetector widget. GestureDetector will capture the gesture and dispatch multiple events based on the gesture.

Some of the gestures and the corresponding events are given below −

Tap
onTapDown
onTapUp
onTap
onTapCancel
Double tap
onDoubleTap
Long press
onLongPress
Vertical drag
onVerticalDragStart
onVerticalDragUpdate
onVerticalDragEnd
Horizontal drag
onHorizontalDragStart
onHorizontalDragUpdate
onHorizontalDragEnd
Pan
onPanStart
onPanUpdate
onPanEnd
Now, let us modify the hello world application to include gesture detection feature and try to understand the concept.

Change the body content of the MyHomePage widget as shown below −

body: Center( 
   child: GestureDetector( 
      onTap: () { 
         _showDialog(context); 
      }, 
      child: Text( 'Hello World', ) 
   ) 
),
Observe that here we have placed the GestureDetector widget above the Text widget in the widget hierarchy, captured the onTap event and then finally shown a dialog window.

Implement the *_showDialog* function to present a dialog when user tabs the hello world message. It uses the generic showDialog and AlertDialog widget to create a new dialog widget. The code is shown below −

// user defined function void _showDialog(BuildContext context) { 
   // flutter defined function 
   showDialog( 
      context: context, builder: (BuildContext context) { 
         // return object of type Dialog
         return AlertDialog( 
            title: new Text("Message"), 
            content: new Text("Hello World"),   
            actions: <Widget>[ 
               new FlatButton( 
                  child: new Text("Close"),  
                  onPressed: () {   
                     Navigator.of(context).pop();  
                  }, 
               ), 
            ], 
         ); 
      }, 
   ); 
}
The application will reload in the device using Hot Reload feature. Now, simply click the message, Hello World and it will show the dialog as below −

Hot Reload Features
Now, close the dialog by clicking the close option in the dialog.

The complete code (main.dart) is as follows −

import 'package:flutter/material.dart'; 
void main() => runApp(MyApp()); 

class MyApp extends StatelessWidget { 
   // This widget is the root of your application.    
   @override 
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Hello World Demo Application', 
         theme: ThemeData( primarySwatch: Colors.blue,), 
         home: MyHomePage(title: 'Home page'), 
      ); 
   }
}
class MyHomePage extends StatelessWidget {
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   
   // user defined function 
   void _showDialog(BuildContext context) { 
      // flutter defined function showDialog( 
         context: context, builder: (BuildContext context) { 
            // return object of type Dialog return AlertDialog(
               title: new Text("Message"), 
               content: new Text("Hello World"),   
               actions: <Widget>[
                  new FlatButton(
                     child: new Text("Close"), 
                     onPressed: () {   
                        Navigator.of(context).pop();  
                     }, 
                  ), 
               ],
            );
         },
      );
   }
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar(title: Text(this.title),),
         body: Center(
            child: GestureDetector( 
               onTap: () {
                  _showDialog(context);
               },
            child: Text( 'Hello World', )
            )
         ),
      );
   }
}
Finally, Flutter also provides a low-level gesture detection mechanism through Listener widget. It will detect all user interactions and then dispatches the following events −

PointerDownEvent
PointerMoveEvent
PointerUpEvent
PointerCancelEvent
Flutter also provides a small set of widgets to do specific as well as advanced gestures. The widgets are listed below −

Dismissible − Supports flick gesture to dismiss the widget.

Draggable − Supports drag gesture to move the widget.

LongPressDraggable − Supports drag gesture to move a widget, when its parent widget is also draggable.

DragTarget − Accepts any Draggable widget

IgnorePointer − Hides the widget and its children from the gesture detection process.

AbsorbPointer − Stops the gesture detection process itself and so any overlapping widget also can not able to participate in the gesture detection process and hence, no event is raised.

Scrollable − Support scrolling of the content available inside the widget.

