Flutter - State Management
Managing state in an application is one of the most important and necessary process in the life cycle of an application.

Let us consider a simple shopping cart application.

User will login using their credentials into the application.

Once user is logged in, the application should persist the logged in user detail in all the screen.

Again, when the user selects a product and saved into a cart, the cart information should persist between the pages until the user checked out the cart.

User and their cart information at any instance is called the state of the application at that instance.

A state management can be divided into two categories based on the duration the particular state lasts in an application.

Ephemeral − Last for a few seconds like the current state of an animation or a single page like current rating of a product. Flutter supports its through StatefulWidget.

app state − Last for entire application like logged in user details, cart information, etc., Flutter supports its through scoped_model.

Navigation and Routing
In any application, navigating from one page / screen to another defines the work flow of the application. The way that the navigation of an application is handled is called Routing. Flutter provides a basic routing class MaterialPageRoute and two methods - Navigator.push and Navigator.pop, to define the work flow of an application.

MaterialPageRoute
MaterialPageRoute is a widget used to render its UI by replacing the entire screen with a platform specific animation.

MaterialPageRoute(builder: (context) => Widget())
Here, builder will accepts a function to build its content by suppling the current context of the application.

Navigation.push
Navigation.push is used to navigate to new screen using MaterialPageRoute widget.

Navigator.push( context, MaterialPageRoute(builder: (context) => Widget()), );
Navigation.pop
Navigation.pop is used to navigate to previous screen.

Navigator.pop(context);
Let us create a new application to better understand the navigation concept.

Create a new Flutter application in Android studio, product_nav_app

Copy the assets folder from product_nav_app to product_state_app and add assets inside the pubspec.yaml file.

flutter:
   assets: 
   - assets/appimages/floppy.png 
   - assets/appimages/iphone.png 
   - assets/appimages/laptop.png 
   - assets/appimages/pendrive.png 
   - assets/appimages/pixel.png 
   - assets/appimages/tablet.png
Replace the default startup code (main.dart) with our startup code.

import 'package:flutter/material.dart'; 
void main() => runApp(MyApp()); 

class MyApp extends StatelessWidget { 
   // This widget is the root of your application. 
   @override 
   Widget build(BuildContext context) { 
      return MaterialApp( 
         title: 'Flutter Demo', 
         theme: ThemeData( 
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(
            title: 'Product state demo home page'
         ),
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
            child: Text('Hello World',)
         ), 
      ); 
   } 
}
Let us create a Product class to organize the product information.

class Product { 
   final String name; 
   final String description; 
   final int price; 
   final String image; 
   Product(this.name, this.description, this.price, this.image); 
}
Let us write a method getProducts in the Product class to generate our dummy product records.

static List<Product> getProducts() {
   List<Product> items = <Product>[]; 
   
   items.add(
      Product( 
         "Pixel", 
         "Pixel is the most feature-full phone ever", 800, 
         "pixel.png"
      )
   ); 
   items.add(
      Product(
         "Laptop", 
         "Laptop is most productive development tool", 
         2000, "
         laptop.png"
      )
   ); 
   items.add(
      Product( 
         "Tablet", 
         "Tablet is the most useful device ever for meeting", 
         1500, 
         "tablet.png"
      )
   ); 
   items.add(
      Product( 
         "Pendrive", 
         "Pendrive is useful storage medium",
         100, 
         "pendrive.png"
      )
   ); 
   items.add(
      Product( 
         "Floppy Drive", 
         "Floppy drive is useful rescue storage medium", 
         20, 
         "floppy.png"
      )
   ); 
   return items; 
}
import product.dart in main.dart
import 'Product.dart';
Let us include our new widget, RatingBox.

class RatingBox extends StatefulWidget {
   @override 
   _RatingBoxState createState() =>_RatingBoxState(); 
} 
class _RatingBoxState extends State<RatingBox> {
   int _rating = 0; 
   void _setRatingAsOne() {
      setState(() {
         _rating = 1; 
      }); 
   } 
   void _setRatingAsTwo() {
      setState(() {
         _rating = 2; 
      }); 
   }
   void _setRatingAsThree() {
      setState(() {
         _rating = 3;
      });
   }
   Widget build(BuildContext context) {
      double _size = 20; 
      print(_rating); 
      return Row(
         mainAxisAlignment: MainAxisAlignment.end, 
         crossAxisAlignment: CrossAxisAlignment.end, 
         mainAxisSize: MainAxisSize.max, 
         children: <Widget>[
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton(
                  icon: (
                     _rating >= 1? 
                     Icon( 
                        Icons.star, 
                        size: _size, 
                     ) 
                     : Icon(
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsOne, 
                  iconSize: _size, 
               ), 
            ), 
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton(
                  icon: (
                     _rating >= 2? 
                     Icon(
                        Icons.star, 
                        size: _size, 
                     ) 
                     : Icon(
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsTwo, 
                  iconSize: _size, 
               ), 
            ), 
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton(
                  icon: (
                     _rating >= 3 ? 
                     Icon(
                        Icons.star, 
                        size: _size, 
                     ) 
                     : Icon( 
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsThree, 
                  iconSize: _size, 
               ), 
            ), 
         ], 
      ); 
   }
}
Let us modify our ProductBox widget to work with our new Product class.

class ProductBox extends StatelessWidget {    
   ProductBox({Key key, this.item}) : super(key: key); 
   final Product item; 
   
   Widget build(BuildContext context) {
      return Container(
         padding: EdgeInsets.all(2), 
         height: 140, 
         child: Card( 
            child: Row(
               mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
               children: <Widget>[ 
                  Image.asset("assets/appimages/" + this.item.image), 
                  Expanded(
                     child: Container(
                        padding: EdgeInsets.all(5), 
                        child: Column(
                           mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
                           children: <Widget>[
                              Text(this.item.name, 
                              style: TextStyle(fontWeight: FontWeight.bold)), 
                              Text(this.item.description), 
                              Text("Price: " + this.item.price.toString()), 
                              RatingBox(), 
                           ], 
                        )
                     )
                  )
               ]
            ), 
         )
      ); 
   }
}
Let us rewrite our MyHomePage widget to work with Product model and to list all products using ListView.

class MyHomePage extends StatelessWidget { 
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   final items = Product.getProducts(); 
   
   @override 
   Widget build(BuildContext context) { 
      return Scaffold( appBar: AppBar(title: Text("Product Navigation")), 
      body: ListView.builder( 
         itemCount: items.length, 
         itemBuilder: (context, index) {
            return GestureDetector( 
               child: ProductBox(item: items[index]), 
               onTap: () { 
                  Navigator.push( 
                     context, MaterialPageRoute( 
                        builder: (context) => ProductPage(item: items[index]), 
                     ), 
                  ); 
               }, 
            ); 
         }, 
      )); 
   } 
}
Here, we have used MaterialPageRoute to navigate to product details page.

Now, let us add ProductPage to show the product details.

class ProductPage extends StatelessWidget { 
   ProductPage({Key key, this.item}) : super(key: key); 
   final Product item; 
   
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar( 
            title: Text(this.item.name), 
         ), 
         body: Center(
            child: Container(
               padding: EdgeInsets.all(0), 
               child: Column(
                  mainAxisAlignment: MainAxisAlignment.start, 
                  crossAxisAlignment: CrossAxisAlignment.start, 
                  children: <Widget>[
                     Image.asset("assets/appimages/" + this.item.image), 
                     Expanded(
                        child: Container(
                           padding: EdgeInsets.all(5), 
                           child: Column(
                              mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
                              children: <Widget>[
                                 Text(
                                    this.item.name, style: TextStyle(
                                       fontWeight: FontWeight.bold
                                    )
                                 ), 
                                 Text(this.item.description), 
                                 Text("Price: " + this.item.price.toString()), 
                                 RatingBox(),
                              ], 
                           )
                        )
                     )
                  ]
               ), 
            ), 
         ), 
      ); 
   } 
}
The complete code of the application is as follows −

import 'package:flutter/material.dart'; 
void main() => runApp(MyApp()); 

class Product {
   final String name; 
   final String description; 
   final int price; 
   final String image; 
   Product(this.name, this.description, this.price, this.image); 
   
   static List<Product> getProducts() {
      List<Product> items = <Product>[]; 
      items.add(
         Product(
            "Pixel", 
            "Pixel is the most featureful phone ever", 
            800, 
            "pixel.png"
         )
      );
      items.add(
         Product(
            "Laptop", 
            "Laptop is most productive development tool", 
            2000, 
            "laptop.png"
         )
      ); 
      items.add(
         Product(
            "Tablet", 
            "Tablet is the most useful device ever for meeting", 
            1500, 
            "tablet.png"
         )
      ); 
      items.add(
         Product( 
            "Pendrive", 
            "iPhone is the stylist phone ever", 
            100, 
            "pendrive.png"
         )
      ); 
      items.add(
         Product(
            "Floppy Drive", 
            "iPhone is the stylist phone ever", 
            20, 
            "floppy.png"
         )
      ); 
      items.add(
         Product(
            "iPhone", 
            "iPhone is the stylist phone ever", 
            1000, 
            "iphone.png"
         )
      ); 
      return items; 
   }
}
class MyApp extends StatelessWidget {
   // This widget is the root of your application. 
   @override 
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Flutter Demo', 
         theme: ThemeData( 
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(title: 'Product Navigation demo home page'), 
      ); 
   }
}
class MyHomePage extends StatelessWidget {
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   final items = Product.getProducts(); 
   
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar(title: Text("Product Navigation")), 
         body: ListView.builder( 
            itemCount: items.length, 
            itemBuilder: (context, index) { 
               return GestureDetector( 
                  child: ProductBox(item: items[index]), 
                  onTap: () { 
                     Navigator.push( 
                        context, 
                        MaterialPageRoute( 
                           builder: (context) => ProductPage(item: items[index]), 
                        ), 
                     ); 
                  }, 
               ); 
            }, 
         )
      ); 
   }
} 
class ProductPage extends StatelessWidget {
   ProductPage({Key key, this.item}) : super(key: key); 
   final Product item; 
   
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar(
            title: Text(this.item.name), 
         ), 
         body: Center(
            child: Container( 
               padding: EdgeInsets.all(0), 
               child: Column( 
                  mainAxisAlignment: MainAxisAlignment.start, 
                  crossAxisAlignment: CrossAxisAlignment.start, 
                  children: <Widget>[ 
                     Image.asset("assets/appimages/" + this.item.image), 
                     Expanded( 
                        child: Container( 
                           padding: EdgeInsets.all(5), 
                           child: Column( 
                              mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
                              children: <Widget>[ 
                                 Text(this.item.name, style: TextStyle(fontWeight: FontWeight.bold)), 
                                 Text(this.item.description), 
                                 Text("Price: " + this.item.price.toString()), 
                                 RatingBox(), 
                              ], 
                           )
                        )
                     ) 
                  ]
               ), 
            ), 
         ), 
      ); 
   } 
}
class RatingBox extends StatefulWidget { 
   @override 
   _RatingBoxState createState() => _RatingBoxState(); 
} 
class _RatingBoxState extends State<RatingBox> { 
   int _rating = 0;
   void _setRatingAsOne() {
      setState(() {
         _rating = 1; 
      }); 
   }
   void _setRatingAsTwo() {
      setState(() {
         _rating = 2; 
      }); 
   } 
   void _setRatingAsThree() { 
      setState(() {
         _rating = 3; 
      }); 
   }
   Widget build(BuildContext context) {
      double _size = 20; 
      print(_rating); 
      return Row(
         mainAxisAlignment: MainAxisAlignment.end, 
         crossAxisAlignment: CrossAxisAlignment.end, 
         mainAxisSize: MainAxisSize.max, 
         children: <Widget>[
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton(
                  icon: (
                     _rating >= 1 ? Icon( 
                        Icons.star, 
                        size: _size, 
                     ) 
                     : Icon( 
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsOne, 
                  iconSize: _size, 
               ), 
            ), 
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton( 
                  icon: (
                     _rating >= 2 ? 
                     Icon( 
                        Icons.star, 
                        size: _size, 
                     ) 
                     : Icon( 
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsTwo, 
                  iconSize: _size, 
               ), 
            ), 
            Container(
               padding: EdgeInsets.all(0), 
               child: IconButton(
                  icon: (
                     _rating >= 3 ? 
                     Icon( 
                        Icons.star, 
                        size: _size, 
                     )
                     : Icon( 
                        Icons.star_border, 
                        size: _size, 
                     )
                  ), 
                  color: Colors.red[500], 
                  onPressed: _setRatingAsThree, 
                  iconSize: _size, 
               ), 
            ), 
         ], 
      ); 
   } 
} 
class ProductBox extends StatelessWidget {
   ProductBox({Key key, this.item}) : super(key: key); 
   final Product item; 
   
   Widget build(BuildContext context) {
      return Container(
         padding: EdgeInsets.all(2), 
         height: 140, 
         child: Card(
            child: Row(
               mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
               children: <Widget>[ 
                  Image.asset("assets/appimages/" + this.item.image), 
                  Expanded( 
                     child: Container( 
                        padding: EdgeInsets.all(5), 
                        child: Column( 
                           mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
                           children: <Widget>[ 
                              Text(this.item.name, style: TextStyle(fontWeight: FontWeight.bold)), Text(this.item.description), 
                              Text("Price: " + this.item.price.toString()), 
                              RatingBox(), 
                           ], 
                        )
                     )
                  ) 
               ]
            ), 
         )
      ); 
   } 
}
Run the application and click any one of the product item. It will show the relevant details page. We can move to home page by clicking back button. The product list page and product details page of the application are shown as follows −

Product Navigation
Pixel1
