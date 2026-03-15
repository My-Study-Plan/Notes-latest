Flutter - Database Concepts
Flutter provides many advanced packages to work with databases. The most important packages are −

sqflite − Used to access and manipulate SQLite database, and

firebase_database − Used to access and manipulate cloud hosted NoSQL database from Google.

In this chapter, let us discuss each of them in detail.

SQLite
SQLite database is the de-facto and standard SQL based embedded database engine. It is small and time-tested database engine. sqflite package provides a lot of functionality to work efficiently with SQLite database. It provides standard methods to manipulate SQLite database engine. The core functionality provided by sqflite package is as follows −

Create / Open (openDatabase method) a SQLite database.

Execute SQL statement (execute method) against SQLite database.

Advanced query methods (query method) to reduce to code required to query and get information from SQLite database.

Let us create a product application to store and fetch product information from a standard SQLite database engine using sqflite package and understand the concept behind the SQLite database and sqflite package.

Create a new Flutter application in Android studio, product_sqlite_app.

Replace the default startup code (main.dart) with our product_rest_app code.

Copy the assets folder from product_nav_app to product_rest_app and add assets inside the *pubspec.yaml` file.

flutter: 
   assets: 
      - assets/appimages/floppy.png 
      - assets/appimages/iphone.png 
      - assets/appimages/laptop.png 
      - assets/appimages/pendrive.png 
      - assets/appimages/pixel.png 
      - assets/appimages/tablet.png
Configure sqflite package in the pubspec.yaml file as shown below −

dependencies: sqflite: any
Use the latest version number of sqflite in place of any

Configure path_provider package in the pubspec.yaml file as shown below −

dependencies: path_provider: any
Here, path_provider package is used to get temporary folder path of the system and path of the application. Use the latest version number of sqflite in place of any.

Android studio will alert that the pubspec.yaml is updated.

Updated
Click Get dependencies option. Android studio will get the package from Internet and properly configure it for the application.

In database, we need primary key, id as additional field along with Product properties like name, price, etc., So, add id property in the Product class. Also, add a new method, toMap to convert product object into Map object. fromMap and toMap are used to serialize and de- serialize the Product object and it is used in database manipulation methods.

class Product { 
   final int id; 
   final String name; 
   final String description; 
   final int price; 
   final String image; 
   static final columns = ["id", "name", "description", "price", "image"]; 
   Product(this.id, this.name, this.description, this.price, this.image); 
   factory Product.fromMap(Map<String, dynamic> data) {
      return Product( 
         data['id'], 
         data['name'], 
         data['description'], 
         data['price'], 
         data['image'], 
      ); 
   } 
   Map<String, dynamic> toMap() => {
      "id": id, 
      "name": name, 
      "description": description, 
      "price": price, 
      "image": image 
   }; 
}
Create a new file, Database.dart in the lib folder to write SQLite related functionality.

Import necessary import statement in Database.dart.

import 'dart:async'; 
import 'dart:io'; 
import 'package:path/path.dart'; 
import 'package:path_provider/path_provider.dart'; 
import 'package:sqflite/sqflite.dart'; 
import 'Product.dart';
Note the following points here −

async is used to write asynchronous methods.

io is used to access files and directories.

path is used to access dart core utility function related to file paths.

path_provider is used to get temporary and application path.

sqflite is used to manipulate SQLite database.

Create a new class SQLiteDbProvider

Declare a singleton based, static SQLiteDbProvider object as specified below −

class SQLiteDbProvider { 
   SQLiteDbProvider._(); 
   static final SQLiteDbProvider db = SQLiteDbProvider._(); 
   static Database _database; 
}
SQLiteDBProvoider object and its method can be accessed through the static db variable.

SQLiteDBProvoider.db.<emthod>
Create a method to get database (Future option) of type Future<Database>. Create product table and load initial data during the creation of the database itself.

Future<Database> get database async { 
   if (_database != null) 
   return _database; 
   _database = await initDB(); 
   return _database; 
}
initDB() async { 
   Directory documentsDirectory = await getApplicationDocumentsDirectory(); 
   String path = join(documentsDirectory.path, "ProductDB.db"); 
   return await openDatabase(
      path, 
      version: 1,
      onOpen: (db) {}, 
      onCreate: (Database db, int version) async {
         await db.execute(
            "CREATE TABLE Product ("
            "id INTEGER PRIMARY KEY,"
            "name TEXT,"
            "description TEXT,"
            "price INTEGER," 
            "image TEXT" ")"
         ); 
         await db.execute(
            "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [1, "iPhone", "iPhone is the stylist phone ever", 1000, "iphone.png"]
         ); 
         await db.execute(
            "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [2, "Pixel", "Pixel is the most feature phone ever", 800, "pixel.png"]
         ); 
         await db.execute(
            "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [3, "Laptop", "Laptop is most productive development tool", 2000, "laptop.png"]\
         ); 
         await db.execute( 
            "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [4, "Tablet", "Laptop is most productive development tool", 1500, "tablet.png"]
         );
         await db.execute( 
            "INSERT INTO Product 
            ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [5, "Pendrive", "Pendrive is useful storage medium", 100, "pendrive.png"]
         );
         await db.execute( 
            "INSERT INTO Product 
            ('id', 'name', 'description', 'price', 'image') 
            values (?, ?, ?, ?, ?)", 
            [6, "Floppy Drive", "Floppy drive is useful rescue storage medium", 20, "floppy.png"]
         ); 
      }
   ); 
}
Here, we have used the following methods −

getApplicationDocumentsDirectory − Returns application directory path

join − Used to create system specific path. We have used it to create database path.

openDatabase − Used to open a SQLite database

onOpen − Used to write code while opening a database

onCreate − Used to write code while a database is created for the first time

db.execute − Used to execute SQL queries. It accepts a query. If the query has placeholder (?), then it accepts values as list in the second argument.

Write a method to get all products in the database −

Future<List<Product>> getAllProducts() async { 
   final db = await database; 
   List<Map> 
   results = await db.query("Product", columns: Product.columns, orderBy: "id ASC"); 
   
   List<Product> products = new List(); 
   results.forEach((result) { 
      Product product = Product.fromMap(result); 
      products.add(product); 
   }); 
   return products; 
}
Here, we have done the following −

Used query method to fetch all the product information. query provides shortcut to query a table information without writing the entire query. query method will generate the proper query itself by using our input like columns, orderBy, etc.,

Used Products fromMap method to get product details by looping the results object, which holds all the rows in the table.

Write a method to get product specific to id

Future<Product> getProductById(int id) async {
   final db = await database; 
   var result = await db.query("Product", where: "id = ", whereArgs: [id]); 
   return result.isNotEmpty ? Product.fromMap(result.first) : Null; 
}
Here, we have used where and whereArgs to apply filters.

Create three methods - insert, update and delete method to insert, update and delete product from the database.

insert(Product product) async { 
   final db = await database; 
   var maxIdResult = await db.rawQuery(
      "SELECT MAX(id)+1 as last_inserted_id FROM Product");

   var id = maxIdResult.first["last_inserted_id"]; 
   var result = await db.rawInsert(
      "INSERT Into Product (id, name, description, price, image)" 
      " VALUES (?, ?, ?, ?, ?)", 
      [id, product.name, product.description, product.price, product.image] 
   ); 
   return result; 
}
update(Product product) async { 
   final db = await database; 
   var result = await db.update("Product", product.toMap(), 
   where: "id = ?", whereArgs: [product.id]); return result; 
} 
delete(int id) async { 
   final db = await database; 
   db.delete("Product", where: "id = ?", whereArgs: [id]); 
}
The final code of the Database.dart is as follows −

import 'dart:async'; 
import 'dart:io'; 
import 'package:path/path.dart'; 
import 'package:path_provider/path_provider.dart'; 
import 'package:sqflite/sqflite.dart'; 
import 'Product.dart'; 

class SQLiteDbProvider {
   SQLiteDbProvider._(); 
   static final SQLiteDbProvider db = SQLiteDbProvider._(); 
   static Database _database; 
   
   Future<Database> get database async {
      if (_database != null) 
      return _database; 
      _database = await initDB(); 
      return _database; 
   } 
   initDB() async {
      Directory documentsDirectory = await 
      getApplicationDocumentsDirectory(); 
      String path = join(documentsDirectory.path, "ProductDB.db"); 
      return await openDatabase(
         path, version: 1, 
         onOpen: (db) {}, 
         onCreate: (Database db, int version) async {
            await db.execute(
               "CREATE TABLE Product (" 
               "id INTEGER PRIMARY KEY," 
               "name TEXT," 
               "description TEXT," 
               "price INTEGER," 
               "image TEXT"")"
            ); 
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [1, "iPhone", "iPhone is the stylist phone ever", 1000, "iphone.png"]
            ); 
            await db.execute( 
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [2, "Pixel", "Pixel is the most feature phone ever", 800, "pixel.png"]
            );
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [3, "Laptop", "Laptop is most productive development tool", 2000, "laptop.png"]
            ); 
            await db.execute( 
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [4, "Tablet", "Laptop is most productive development tool", 1500, "tablet.png"]
            ); 
            await db.execute( 
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [5, "Pendrive", "Pendrive is useful storage medium", 100, "pendrive.png"]
            );
            await db.execute( 
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", 
               [6, "Floppy Drive", "Floppy drive is useful rescue storage medium", 20, "floppy.png"]
            ); 
         }
      ); 
   }
   Future<List<Product>> getAllProducts() async {
      final db = await database; 
      List<Map> results = await db.query(
         "Product", columns: Product.columns, orderBy: "id ASC"
      ); 
      List<Product> products = new List();   
      results.forEach((result) {
         Product product = Product.fromMap(result); 
         products.add(product); 
      }); 
      return products; 
   } 
   Future<Product> getProductById(int id) async {
      final db = await database; 
      var result = await db.query("Product", where: "id = ", whereArgs: [id]); 
      return result.isNotEmpty ? Product.fromMap(result.first) : Null; 
   } 
   insert(Product product) async { 
      final db = await database; 
      var maxIdResult = await db.rawQuery("SELECT MAX(id)+1 as last_inserted_id FROM Product"); 
      var id = maxIdResult.first["last_inserted_id"]; 
      var result = await db.rawInsert(
         "INSERT Into Product (id, name, description, price, image)" 
         " VALUES (?, ?, ?, ?, ?)", 
         [id, product.name, product.description, product.price, product.image] 
      ); 
      return result; 
   } 
   update(Product product) async { 
      final db = await database; 
      var result = await db.update(
         "Product", product.toMap(), where: "id = ?", whereArgs: [product.id]
      ); 
      return result; 
   } 
   delete(int id) async { 
      final db = await database; 
      db.delete("Product", where: "id = ?", whereArgs: [id]);
   } 
}
Change the main method to get the product information.

void main() {
   runApp(MyApp(products: SQLiteDbProvider.db.getAllProducts())); 
}
Here, we have used the getAllProducts method to fetch all products from the database.

Run the application and see the results. It will be similar to previous example, Accessing Product service API, except the product information is stored and fetched from the local SQLite database.

Cloud Firestore
Firebase is a BaaS app development platform. It provides many feature to speed up the mobile application development like authentication service, cloud storage, etc., One of the main feature of Firebase is Cloud Firestore, a cloud based real time NoSQL database.

Flutter provides a special package, cloud_firestore to program with Cloud Firestore. Let us create an online product store in the Cloud Firestore and create a application to access the product store.

Create a new Flutter application in Android studio, product_firebase_app.

Replace the default startup code (main.dart) with our product_rest_app code.

Copy Product.dart file from product_rest_app into the lib folder.

class Product { 
   final String name; 
   final String description; 
   final int price; 
   final String image; 
   
   Product(this.name, this.description, this.price, this.image); 
   factory Product.fromMap(Map<String, dynamic> json) {
      return Product( 
         json['name'], 
         json['description'], 
         json['price'], 
         json['image'], 
      ); 
   }
}
Copy the assets folder from product_rest_app to product_firebase_app and add assets inside the pubspec.yaml file.

flutter:
   assets: 
   - assets/appimages/floppy.png 
   - assets/appimages/iphone.png 
   - assets/appimages/laptop.png 
   - assets/appimages/pendrive.png 
   - assets/appimages/pixel.png 
   - assets/appimages/tablet.png
Configure cloud_firestore package in the pubspec.yaml file as shown below −

dependencies: cloud_firestore: ^0.9.13+1
Here, use the latest version of the cloud_firestore package.

Android studio will alert that the pubspec.yaml is updated as shown here −

Cloud Firestore Package
Click Get dependencies option. Android studio will get the package from Internet and properly configure it for the application.

Create a project in the Firebase using the following steps −

Create a Firebase account by selecting Free plan at https://firebase.google.com/pricing/.

Once Firebase account is created, it will redirect to the project overview page. It list all the Firebase based project and provides an option to create a new project.

Click Add project and it will open a project creation page.

Enter products app db as project name and click Create project option.

Go to *Firebase console.

Click Project overview. It opens the project overview page.

Click android icon. It will open project setting specific to Android development.

Enter Android Package name, com.tutorialspoint.flutterapp.product_firebase_app.

Click Register App. It generates a project configuration file, google_service.json.

Download google_service.json and then move it into the projects android/app directory. This file is the connection between our application and Firebase.

Open android/app/build.gradle and include the following code −

apply plugin: 'com.google.gms.google-services'
Open android/build.gradle and include the following configuration −

buildscript {
   repositories { 
      // ... 
   } 
   dependencies { 
      // ... 
      classpath 'com.google.gms:google-services:3.2.1' // new 
   } 
}
Here, the plugin and class path are used for the purpose of reading google_service.json file.

Open android/app/build.gradle and include the following code as well.

android {
   defaultConfig { 
      ... 
      multiDexEnabled true 
   } 
   ...
}
dependencies {
   ... 
   compile 'com.android.support: multidex:1.0.3' 
}
This dependency enables the android application to use multiple dex functionality.

Follow the remaining steps in the Firebase Console or just skip it.

Create a product store in the newly created project using the following steps −

Go to Firebase console.

Open the newly created project.

Click the Database option in the left menu.

Click Create database option.

Click Start in test mode and then Enable.

Click Add collection. Enter product as collection name and then click Next.

Enter the sample product information as shown in the image here −

Sample Product Information
Add addition product information using Add document options.

Open main.dart file and import Cloud Firestore plugin file and remove http package.

import 'package:cloud_firestore/cloud_firestore.dart';
Remove parseProducts and update fetchProducts to fetch products from Cloud Firestore instead of Product service API.

Stream<QuerySnapshot> fetchProducts() { 
   return Firestore.instance.collection('product').snapshots(); }
Here, Firestore.instance.collection method is used to access product collection available in the cloud store. Firestore.instance.collection provides many option to filter the collection to get the necessary documents. But, we have not applied any filter to get all product information.

Cloud Firestore provides the collection through Dart Stream concept and so modify the products type in MyApp and MyHomePage widget from Future<list<Product>> to Stream<QuerySnapshot>.

Change the build method of MyHomePage widget to use StreamBuilder instead of FutureBuilder.

@override 
Widget build(BuildContext context) {
   return Scaffold(
      appBar: AppBar(title: Text("Product Navigation")), 
      body: Center(
         child: StreamBuilder<QuerySnapshot>(
            stream: products, builder: (context, snapshot) {
               if (snapshot.hasError) print(snapshot.error); 
               if(snapshot.hasData) {
                  List<DocumentSnapshot> 
                  documents = snapshot.data.documents; 
                  
                  List<Product> 
                  items = List<Product>(); 
                  
                  for(var i = 0; i < documents.length; i++) { 
                     DocumentSnapshot document = documents[i]; 
                     items.add(Product.fromMap(document.data)); 
                  } 
                  return ProductBoxList(items: items);
               } else { 
                  return Center(child: CircularProgressIndicator()); 
               }
            }, 
         ), 
      )
   ); 
}
Here, we have fetched the product information as List<DocumentSnapshot> type. Since, our widget, ProductBoxList is not compatible with documents, we have converted the documents into List<Product> type and further used it.

Finally, run the application and see the result. Since, we have used the same product information as that of SQLite application and changed the storage medium only, the resulting application looks identical to SQLite application application.

