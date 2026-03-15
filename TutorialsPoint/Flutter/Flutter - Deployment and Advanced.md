Flutter - Deployment
This chapter explains how to deploy Flutter application in both Android and iOS platforms.

Android Application
Change the application name using android:label entry in android manifest file. Android app manifest file, AndroidManifest.xml is located in <app dir>/android/app/src/main. It contains entire details about an android application. We can set the application name using android:label entry.

Change launcher icon using android:icon entry in manifest file.

Sign the app using standard option as necessary.

Enable Proguard and Obfuscation using standard option, if necessary.

Create a release APK file by running below command −

cd /path/to/my/application 
flutter build apk
You can see an output as shown below −

Initializing gradle...                                            8.6s 
Resolving dependencies...                                        19.9s 
Calling mockable JAR artifact transform to create file: 
/Users/.gradle/caches/transforms-1/files-1.1/android.jar/ 
c30932f130afbf3fd90c131ef9069a0b/android.jar with input 
/Users/Library/Android/sdk/platforms/android-28/android.jar 
Running Gradle task 'assembleRelease'... 
Running Gradle task 'assembleRelease'... 
Done                                                             85.7s 
Built build/app/outputs/apk/release/app-release.apk (4.8MB).
Install the APK on a device using the following command −

flutter install
Publish the application into Google Playstore by creating an appbundle and push it into playstore using standard methods.

flutter build appbundle
iOS Application
Register the iOS application in App Store Connect using standard method. Save the =Bundle ID used while registering the application.

Update Display name in the XCode project setting to set the application name.

Update Bundle Identifier in the XCode project setting to set the bundle id, which we used in step 1.

Code sign as necessary using standard method.

Add a new app icon as necessary using standard method.

Generate IPA file using the following command −

flutter build ios
Now, you can see the following output −

Building com.example.MyApp for device (ios-release)... 
Automatically signing iOS for device deployment 
using specified development team in Xcode project: 
Running Xcode build...                                   23.5s 
......................
Test the application by pushing the application, IPA file into TestFlight using standard method.

Finally, push the application into App Store using standard method.

Flutter - Development Tools
This chapter explains about Flutter development tools in detail. The first stable release of the cross-platform development toolkit was released on December 4th, 2018, Flutter 1.0. Well, Google is continuously working on the improvements and strengthening the Flutter framework with different development tools.

Widget Sets
Google updated for Material and Cupertino widget sets to provide pixel-perfect quality in the components design. The upcoming version of flutter 1.2 will be designed to support desktop keyboard events and mouse hover support.

Flutter Development with Visual Studio Code
Visual Studio Code supports flutter development and provides extensive shortcuts for fast and efficient development. Some of the key features provided by Visual Studio Code for flutter development are listed below −

Code assist - When you want to check for options, you can use Ctrl+Space to get a list of code completion options.

Quick fix - Ctrl+. is quick fix tool to help in fixing the code.

Shortcuts while Coding.

Provides detailed documentation in comments.

Debugging shortcuts.

Hot restarts.

Dart DevTools
We can use Android Studio or Visual Studio Code, or any other IDE to write our code and install plugins. Googles development team has been working on yet another development tool called Dart DevTools It is a web-based programming suite. It supports both Android and iOS platforms. It is based on time line view so developers can easily analyze their applications.

Install DevTools
To install DevTools run the following command in your console −

flutter packages pub global activate devtools
Now you can see the following output −

Resolving dependencies... 
+ args 1.5.1 
+ async 2.2.0
+ charcode 1.1.2 
+ codemirror 0.5.3+5.44.0 
+ collection 1.14.11 
+ convert 2.1.1 
+ devtools 0.0.16 
+ devtools_server 0.0.2 
+ http 0.12.0+2 
+ http_parser 3.1.3 
+ intl 0.15.8 
+ js 0.6.1+1 
+ meta 1.1.7 
+ mime 0.9.6+2 
.................. 
.................. 
Installed executable devtools. 
Activated devtools 0.0.16.
Run Server
You can run the DevTools server using the following command −

flutter packages pub global run devtools
Now, you will get a response similar to this,

Serving DevTools at http://127.0.0.1:9100
Start Your Application
Go to your application, open simulator and run using the following command −

flutter run --observatory-port=9200
Now, you are connected to DevTools.

Start DevTools in Browser
Now access the below url in the browser, to start DevTools −

http://localhost:9100/?port=9200
You will get a response as shown below −

Dart Dev Tools
Flutter SDK
To update Flutter SDK, use the following command −

flutter upgrade
You can see an output as shown below −

Flutter SDK
To upgrade Flutter packages, use the following command −

flutter packages upgrade
You could see the following response,

Running "flutter packages upgrade" in my_app... 7.4s
Flutter Inspector
It is used to explore flutter widget trees. To achieve this, run the below command in your console,

flutter run --track-widget-creation
You can see an output as shown below −

Launching lib/main.dart on iPhone X in debug mode... 
Assembling Flutter resources...                       3.6s 
Compiling, linking and signing...                      6.8s 
Xcode build done.                                     14.2s 
2,904ms (!)
To hot reload changes while running, press "r". To hot restart (and rebuild state), press "R". 
An Observatory debugger and profiler on iPhone X is available at: http://127.0.0.1:50399/ 
For a more detailed help message, press "h". To detach, press "d"; to quit, press "q".
Now go to the url, http://127.0.0.1:50399/ you could see the following result −

Result
Flutter - Writting Advanced Applications
In this chapter, we are going to learn how to write a full fledged mobile application, expense_calculator. The purpose of the expense_calculator is to store our expense information. The complete feature of the application is as follows −

Expense list.

Form to enter new expenses.

Option to edit / delete the existing expenses.

Total expenses at any instance.

We are going to program the expense_calculator application using below mentioned advanced features of Flutter framework.

Advanced use of ListView to show the expense list.

Form programming.

SQLite database programming to store our expenses.

scoped_model state management to simplify our programming.

Let us start programming the expense_calculator application.

Create a new Flutter application, expense_calculator in Android studio.

Open pubspec.yaml and add package dependencies.

dependencies: 
   flutter: 
      sdk: flutter 
   sqflite: ^1.1.0 
   path_provider: ^0.5.0+1 
   scoped_model: ^1.0.1 
   intl: any
Observe these points here −

sqflite is used for SQLite database programming.

path_provider is used to get system specific application path.

scoped_model is used for state management.

intl is used for date formatting.

Android studio will display the following alert that the pubspec.yaml is updated.

Alert Writing Advanced Applications
Click Get dependencies option. Android studio will get the package from Internet and properly configure it for the application.

Remove the existing code in main.dart.

Add new file, Expense.dart to create Expense class. Expense class will have the below properties and methods.

property: id − Unique id to represent an expense entry in SQLite database.

property: amount − Amount spent.

property: date − Date when the amount is spent.

property: category − Category represents the area in which the amount is spent. e.g Food, Travel, etc.,

formattedDate − Used to format the date property

fromMap − Used to map the field from database table to the property in the expense object and to create a new expense object.

factory Expense.fromMap(Map<String, dynamic> data) { 
   return Expense( 
      data['id'], 
      data['amount'], 
      DateTime.parse(data['date']),    
      data['category'] 
   ); 
}
toMap − Used to convert the expense object to Dart Map, which can be further used in database programming

Map<String, dynamic> toMap() => { 
   "id" : id, 
   "amount" : amount, 
   "date" : date.toString(), 
   "category" : category, 
};
columns − Static variable used to represent the database field.

Enter and save the following code into the Expense.dart file.

import 'package:intl/intl.dart'; class Expense {
   final int id; 
   final double amount; 
   final DateTime date; 
   final String category; 
   String get formattedDate { 
      var formatter = new DateFormat('yyyy-MM-dd'); 
      return formatter.format(this.date); 
   } 
   static final columns = ['id', 'amount', 'date', 'category'];
   Expense(this.id, this.amount, this.date, this.category); 
   factory Expense.fromMap(Map<String, dynamic> data) { 
      return Expense( 
         data['id'], 
         data['amount'], 
         DateTime.parse(data['date']), data['category'] 
      ); 
   }
   Map<String, dynamic> toMap() => {
      "id" : id, 
      "amount" : amount, 
      "date" : date.toString(), 
      "category" : category, 
   }; 
}
The above code is simple and self explanatory.

Add new file, Database.dart to create SQLiteDbProvider class. The purpose of the SQLiteDbProvider class is as follows −

Get all expenses available in the database using getAllExpenses method. It will be used to list all the users expense information.

Future<List<Expense>> getAllExpenses() async { 
   final db = await database; 
   
   List<Map> results = await db.query(
      "Expense", columns: Expense.columns, orderBy: "date DESC"
   );
   List<Expense> expenses = new List(); 
   results.forEach((result) {
      Expense expense = Expense.fromMap(result); 
      expenses.add(expense); 
   }); 
   return expenses; 
}
Get a specific expense information based on expense identity available in the database using getExpenseById method. It will be used to show the particular expense information to the user.

Future<Expense> getExpenseById(int id) async {
   final db = await database;
   var result = await db.query("Expense", where: "id = ", whereArgs: [id]);
   
   return result.isNotEmpty ? 
   Expense.fromMap(result.first) : Null; 
}
Get the total expenses of the user using getTotalExpense method. It will be used to show the current total expense to the user.

Future<double> getTotalExpense() async {
   final db = await database; 
   List<Map> list = await db.rawQuery(
      "Select SUM(amount) as amount from expense"
   );
   return list.isNotEmpty ? list[0]["amount"] : Null; 
}
Add new expense information into the database using insert method. It will be used to add new expense entry into the application by the user.

Future<Expense> insert(Expense expense) async { 
   final db = await database; 
   var maxIdResult = await db.rawQuery(
      "SELECT MAX(id)+1 as last_inserted_id FROM Expense"
   );
   var id = maxIdResult.first["last_inserted_id"]; 
   var result = await db.rawInsert(
      "INSERT Into Expense (id, amount, date, category)" 
      " VALUES (?, ?, ?, ?)", [
         id, expense.amount, expense.date.toString(), expense.category
      ]
   ); 
   return Expense(id, expense.amount, expense.date, expense.category); 
}
Update existing expense information using update method. It will be used to edit and update existing expense entry available in the system by the user.

update(Expense product) async {
   final db = await database; 
   
   var result = await db.update("Expense", product.toMap(), 
   where: "id = ?", whereArgs: [product.id]); 
   return result; 
}
Delete existing expense information using delete method. It will be used remove the existing expense entry available in the system by the user.

delete(int id) async {
   final db = await database;
   db.delete("Expense", where: "id = ?", whereArgs: [id]); 
}
The complete code of the SQLiteDbProvider class is as follows −

import 'dart:async'; 
import 'dart:io'; 
import 'package:path/path.dart'; 
import 'package:path_provider/path_provider.dart'; 
import 'package:sqflite/sqflite.dart'; 
import 'Expense.dart'; 

class SQLiteDbProvider {
   SQLiteDbProvider._(); 
   static final SQLiteDbProvider db = SQLiteDbProvider._(); 
   
   static Database _database; Future<Database> get database async { 
      if (_database != null) 
         return _database; 
      _database = await initDB(); 
      return _database; 
   } 
   initDB() async {
      Directory documentsDirectory = await getApplicationDocumentsDirectory(); 
      String path = join(documentsDirectory.path, "ExpenseDB2.db"); 
      return await openDatabase(
         path, version: 1, onOpen:(db){}, onCreate: (Database db, int version) async {
            await db.execute(
               "CREATE TABLE Expense (
                  ""id INTEGER PRIMARY KEY," "amount REAL," "date TEXT," "category TEXT""
               )
            "); 
            await db.execute(
               "INSERT INTO Expense ('id', 'amount', 'date', 'category') 
               values (?, ?, ?, ?)",[1, 1000, '2019-04-01 10:00:00', "Food"]
            );
            /*await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", [
                  2, "Pixel", "Pixel is the most feature phone ever", 800, "pixel.png"
               ]
            ); 
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", [
                  3, "Laptop", "Laptop is most productive development tool", 2000, "laptop.png"
               ]
            );
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", [
                  4, "Tablet", "Laptop is most productive development tool", 1500, "tablet.png"
               ]
            );
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", [
                  5, "Pendrive", "iPhone is the stylist phone ever", 100, "pendrive.png"
               ]
            ); 
            await db.execute(
               "INSERT INTO Product ('id', 'name', 'description', 'price', 'image') 
               values (?, ?, ?, ?, ?)", [
                  6, "Floppy Drive", "iPhone is the stylist phone ever", 20, "floppy.png"
               ]
            ); */ 
         }
      );
   }
   Future<List<Expense>> getAllExpenses() async {
      final db = await database; 
      List<Map> 
      results = await db.query(
         "Expense", columns: Expense.columns, orderBy: "date DESC"
      );
      List<Expense> expenses = new List(); 
      results.forEach((result) {
         Expense expense = Expense.fromMap(result);
         expenses.add(expense);
      }); 
      return expenses; 
   } 
   Future<Expense> getExpenseById(int id) async {
      final db = await database;
      var result = await db.query("Expense", where: "id = ", whereArgs: [id]); 
      return result.isNotEmpty ? Expense.fromMap(result.first) : Null; 
   }
   Future<double> getTotalExpense() async {
      final db = await database;
      List<Map> list = await db.rawQuery(
         "Select SUM(amount) as amount from expense"
      );
      return list.isNotEmpty ? list[0]["amount"] : Null; 
   }
   Future<Expense> insert(Expense expense) async {
      final db = await database; 
      var maxIdResult = await db.rawQuery(
         "SELECT MAX(id)+1 as last_inserted_id FROM Expense"
      );
      var id = maxIdResult.first["last_inserted_id"]; 
      var result = await db.rawInsert(
         "INSERT Into Expense (id, amount, date, category)" 
         " VALUES (?, ?, ?, ?)", [
            id, expense.amount, expense.date.toString(), expense.category
         ]
      );
      return Expense(id, expense.amount, expense.date, expense.category); 
   }
   update(Expense product) async {
      final db = await database; 
      var result = await db.update(
         "Expense", product.toMap(), where: "id = ?", whereArgs: [product.id]
      ); 
      return result; 
   }
   delete(int id) async {
      final db = await database;
      db.delete("Expense", where: "id = ?", whereArgs: [id]);
   }
}
Here,

database is the property to get the SQLiteDbProvider object.

initDB is a method used to select and open the SQLite database.

Create a new file, ExpenseListModel.dart to create ExpenseListModel. The purpose of the model is to hold the complete information of the user expenses in the memory and updating the user interface of the application whenever users expense changes in the memory. It is based on Model class from scoped_model package. It has the following properties and methods −

_items − private list of expenses.

items − getter for _items as UnmodifiableListView<Expense> to prevent unexpected or accidental changes to the list.

totalExpense − getter for Total expenses based on the items variable.

double get totalExpense {
   double amount = 0.0; 
   for(var i = 0; i < _items.length; i++) { 
      amount = amount + _items[i].amount; 
   } 
   return amount; 
}
load − Used to load the complete expenses from database and into the _items variable. It also calls notifyListeners to update the UI.

void load() {
   Future<List<Expense>> 
   list = SQLiteDbProvider.db.getAllExpenses(); 
   list.then( (dbItems) {
      for(var i = 0; i < dbItems.length; i++) { 
         _items.add(dbItems[i]); 
      } notifyListeners(); 
   });
}
byId − Used to get a particular expenses from _items variable.

Expense byId(int id) { 
   for(var i = 0; i < _items.length; i++) { 
      if(_items[i].id == id) { 
         return _items[i]; 
      } 
   }
   return null; 
}
add − Used to add a new expense item into the _items variable as well as into the database. It also calls notifyListeners to update the UI.

void add(Expense item) {
   SQLiteDbProvider.db.insert(item).then((val) { 
      _items.add(val); notifyListeners(); 
   }); 
}
Update − Used to Update expense item into the _items variable as well as into the database. It also calls notifyListeners to update the UI.

void update(Expense item) {
   bool found = false;
   for(var i = 0; i < _items.length; i++) {
      if(_items[i].id == item.id) {
         _items[i] = item; 
         found = true; 
         SQLiteDbProvider.db.update(item); break; 
      } 
   }
   if(found) notifyListeners(); 
}
delete − Used to remove an existing expense item in the _items variable as well as from the database. It also calls notifyListeners to update the UI.

void delete(Expense item) { 
   bool found = false; 
   for(var i = 0; i < _items.length; i++) {
      if(_items[i].id == item.id) {
         found = true; 
         SQLiteDbProvider.db.delete(item.id); 
         _items.removeAt(i); break; 
      }
   }
   if(found) notifyListeners(); 
}
The complete code of the ExpenseListModel class is as follows −

import 'dart:collection'; 
import 'package:scoped_model/scoped_model.dart'; 
import 'Expense.dart'; 
import 'Database.dart'; 

class ExpenseListModel extends Model { 
   ExpenseListModel() { 
      this.load(); 
   } 
   final List<Expense> _items = []; 
   UnmodifiableListView<Expense> get items => 
   UnmodifiableListView(_items); 
   
   /*Future<double> get totalExpense { 
      return SQLiteDbProvider.db.getTotalExpense(); 
   }*/ 
   
   double get totalExpense {
      double amount = 0.0;
      for(var i = 0; i < _items.length; i++) { 
         amount = amount + _items[i].amount; 
      } 
      return amount; 
   }
   void load() {
      Future<List<Expense>> list = SQLiteDbProvider.db.getAllExpenses(); 
      list.then( (dbItems) {
         for(var i = 0; i < dbItems.length; i++) {
            _items.add(dbItems[i]); 
         } 
         notifyListeners(); 
      }); 
   }
   Expense byId(int id) {
      for(var i = 0; i < _items.length; i++) { 
         if(_items[i].id == id) { 
            return _items[i]; 
         } 
      }
      return null; 
   }
   void add(Expense item) {
      SQLiteDbProvider.db.insert(item).then((val) {
         _items.add(val);
         notifyListeners();
      }); 
   }
   void update(Expense item) {
      bool found = false; 
      for(var i = 0; i < _items.length; i++) {
         if(_items[i].id == item.id) {
            _items[i] = item; 
            found = true; 
            SQLiteDbProvider.db.update(item); 
            break; 
         }
      }
      if(found) notifyListeners(); 
   }
   void delete(Expense item) {
      bool found = false; 
      for(var i = 0; i < _items.length; i++) {
         if(_items[i].id == item.id) {
            found = true; 
            SQLiteDbProvider.db.delete(item.id); 
            _items.removeAt(i); break; 
         }
      }
      if(found) notifyListeners(); 
   }
}
Open main.dart file. Import the classes as specified below −

import 'package:flutter/material.dart'; 
import 'package:scoped_model/scoped_model.dart'; 
import 'ExpenseListModel.dart'; 
import 'Expense.dart';
Add main function and call runApp by passing ScopedModel<ExpenseListModel> widget.

void main() { 
   final expenses = ExpenseListModel(); 
   runApp(
      ScopedModel<ExpenseListModel>(model: expenses, child: MyApp(),)
   );
}
Here,

expenses object loads all the user expenses information from the database. Also, when the application is opened for the first time, it will create the required database with proper tables.

ScopedModel provides the expense information during the whole life cycle of the application and ensures the maintenance of state of the application at any instance. It enables us to use StatelessWidget instead of StatefulWidget.

Create a simple MyApp using MaterialApp widget.

class MyApp extends StatelessWidget {
   // This widget is the root of your application. 
   @override 
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Expense',
         theme: ThemeData(
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(title: 'Expense calculator'), 
      );
   }
}
Create MyHomePage widget to display all the users expense information along with total expenses at the top. Floating button at the bottom right corner will be used to add new expenses.

class MyHomePage extends StatelessWidget { 
   MyHomePage({Key key, this.title}) : super(key: key); 
   final String title; 
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar( 
            title: Text(this.title), 
         ), 
         body: ScopedModelDescendant<ExpenseListModel>(
            builder: (context, child, expenses) {
               return ListView.separated(
                  itemCount: expenses.items == null ? 1 
                  : expenses.items.length + 1, 
                  itemBuilder: (context, index) { 
                     if (index == 0) { 
                        return ListTile(
                           title: Text("Total expenses: " 
                           + expenses.totalExpense.toString(), 
                           style: TextStyle(fontSize: 24,
                           fontWeight: FontWeight.bold),) 
                        );
                     } else {
                        index = index - 1; 
                        return Dismissible( 
                           key: Key(expenses.items[index].id.toString()), 
                              onDismissed: (direction) { 
                              expenses.delete(expenses.items[index]); 
                              Scaffold.of(context).showSnackBar(
                                 SnackBar(
                                    content: Text(
                                       "Item with id, " 
                                       + expenses.items[index].id.toString() + 
                                       " is dismissed"
                                    )
                                 )
                              ); 
                           },
                           child: ListTile( onTap: () { 
                              Navigator.push(
                                 context, MaterialPageRoute(
                                    builder: (context) => FormPage(
                                       id: expenses.items[index].id,
                                       expenses: expenses, 
                                    )
                                 )
                              );
                           }, 
                           leading: Icon(Icons.monetization_on), 
                           trailing: Icon(Icons.keyboard_arrow_right), 
                           title: Text(expenses.items[index].category + ": " + 
                           expenses.items[index].amount.toString() + 
                           " \nspent on " + expenses.items[index].formattedDate, 
                           style: TextStyle(fontSize: 18, fontStyle: FontStyle.italic),))
                        ); 
                     }
                  },
                  separatorBuilder: (context, index) { 
                     return Divider(); 
                  }, 
               );
            },
         ),
         floatingActionButton: ScopedModelDescendant<ExpenseListModel>(
            builder: (context, child, expenses) {
               return FloatingActionButton( onPressed: () {
                  Navigator.push( 
                     context, MaterialPageRoute(
                        builder: (context) => ScopedModelDescendant<ExpenseListModel>(
                           builder: (context, child, expenses) { 
                              return FormPage( id: 0, expenses: expenses, ); 
                           }
                        )
                     )
                  ); 
                  // expenses.add(new Expense( 
                     // 2, 1000, DateTime.parse('2019-04-01 11:00:00'), 'Food')
                  ); 
                  // print(expenses.items.length); 
               },
               tooltip: 'Increment', child: Icon(Icons.add), ); 
            }
         )
      );
   }
}
Here,

ScopedModelDescendant is used to pass the expense model into the ListView and FloatingActionButton widget.

ListView.separated and ListTile widget is used to list the expense information.

Dismissible widget is used to delete the expense entry using swipe gesture.

Navigator is used to open edit interface of an expense entry. It can be activated by tapping an expense entry.

Create a FormPage widget. The purpose of the FormPage widget is to add or update an expense entry. It handles expense entry validation as well.

class FormPage extends StatefulWidget { 
   FormPage({Key key, this.id, this.expenses}) : super(key: key); 
   final int id; 
   final ExpenseListModel expenses; 
   
   @override _FormPageState createState() => _FormPageState(id: id, expenses: expenses); 
}
class _FormPageState extends State<FormPage> {
   _FormPageState({Key key, this.id, this.expenses}); 
   
   final int id; 
   final ExpenseListModel expenses; 
   final scaffoldKey = GlobalKey<ScaffoldState>(); 
   final formKey = GlobalKey<FormState>(); 
   
   double _amount; 
   DateTime _date; 
   String _category; 
   
   void _submit() {
      final form = formKey.currentState; 
      if (form.validate()) {
         form.save(); 
         if (this.id == 0) expenses.add(Expense(0, _amount, _date, _category)); 
            else expenses.update(Expense(this.id, _amount, _date, _category)); 
         Navigator.pop(context); 
      }
   }
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         key: scaffoldKey, appBar: AppBar(
            title: Text('Enter expense details'),
         ), 
         body: Padding(
            padding: const EdgeInsets.all(16.0), 
            child: Form(
               key: formKey, child: Column(
                  children: [
                     TextFormField( 
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration( 
                           icon: const Icon(Icons.monetization_on), 
                           labelText: 'Amount', 
                           labelStyle: TextStyle(fontSize: 18)
                        ), 
                        validator: (val) {
                           Pattern pattern = r'^[1-9]\d*(\.\d+)?$'; 
                           RegExp regex = new RegExp(pattern); 
                           if (!regex.hasMatch(val)) 
                           return 'Enter a valid number'; else return null; 
                        }, 
                        initialValue: id == 0 
                        ? '' : expenses.byId(id).amount.toString(), 
                        onSaved: (val) => _amount = double.parse(val), 
                     ), 
                     TextFormField( 
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration( 
                           icon: const Icon(Icons.calendar_today),
                           hintText: 'Enter date', 
                           labelText: 'Date', 
                           labelStyle: TextStyle(fontSize: 18), 
                        ), 
                        validator: (val) {
                           Pattern pattern = r'^((?:19|20)\d\d)[- /.]
                              (0[1-9]|1[012])[- /.](0[1-9]|[12][0-9]|3[01])$'; 
                           RegExp regex = new RegExp(pattern); 
                           if (!regex.hasMatch(val)) 
                              return 'Enter a valid date'; 
                           else return null; 
                        },
                        onSaved: (val) => _date = DateTime.parse(val), 
                        initialValue: id == 0 
                        ? '' : expenses.byId(id).formattedDate, 
                        keyboardType: TextInputType.datetime, 
                     ),
                     TextFormField(
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration(
                           icon: const Icon(Icons.category),
                           labelText: 'Category', 
                           labelStyle: TextStyle(fontSize: 18)
                        ),
                        onSaved: (val) => _category = val, 
                        initialValue: id == 0 ? '' 
                        : expenses.byId(id).category.toString(),
                     ), 
                     RaisedButton( 
                        onPressed: _submit, 
                        child: new Text('Submit'), 
                     ), 
                  ],
               ),
            ),
         ),
      );
   }
}
Here,

TextFormField is used to create form entry.

validator property of TextFormField is used to validate the form element along with RegEx patterns.

_submit function is used along with expenses object to add or update the expenses into the database.

The complete code of the main.dart file is as follows −

import 'package:flutter/material.dart'; 
import 'package:scoped_model/scoped_model.dart'; 
import 'ExpenseListModel.dart'; 
import 'Expense.dart'; 

void main() { 
   final expenses = ExpenseListModel(); 
   runApp(
      ScopedModel<ExpenseListModel>(
         model: expenses, child: MyApp(), 
      )
   ); 
}
class MyApp extends StatelessWidget {
   // This widget is the root of your application. 
   @override
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Expense',
         theme: ThemeData(
            primarySwatch: Colors.blue, 
         ), 
         home: MyHomePage(title: 'Expense calculator'), 
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
         body: ScopedModelDescendant<ExpenseListModel>(
            builder: (context, child, expenses) { 
               return ListView.separated(
                  itemCount: expenses.items == null ? 1 
                  : expenses.items.length + 1, itemBuilder: (context, index) { 
                     if (index == 0) { 
                        return ListTile( title: Text("Total expenses: " 
                        + expenses.totalExpense.toString(), 
                        style: TextStyle(fontSize: 24,fontWeight: 
                        FontWeight.bold),) ); 
                     } else {
                        index = index - 1; return Dismissible(
                           key: Key(expenses.items[index].id.toString()), 
                           onDismissed: (direction) {
                              expenses.delete(expenses.items[index]); 
                              Scaffold.of(context).showSnackBar(
                                 SnackBar(
                                    content: Text(
                                       "Item with id, " + 
                                       expenses.items[index].id.toString() 
                                       + " is dismissed"
                                    )
                                 )
                              );
                           }, 
                           child: ListTile( onTap: () {
                              Navigator.push( context, MaterialPageRoute(
                                 builder: (context) => FormPage(
                                    id: expenses.items[index].id, expenses: expenses, 
                                 )
                              ));
                           }, 
                           leading: Icon(Icons.monetization_on), 
                           trailing: Icon(Icons.keyboard_arrow_right), 
                           title: Text(expenses.items[index].category + ": " + 
                           expenses.items[index].amount.toString() + " \nspent on " + 
                           expenses.items[index].formattedDate, 
                           style: TextStyle(fontSize: 18, fontStyle: FontStyle.italic),))
                        );
                     }
                  }, 
                  separatorBuilder: (context, index) {
                     return Divider(); 
                  },
               ); 
            },
         ),
         floatingActionButton: ScopedModelDescendant<ExpenseListModel>(
            builder: (context, child, expenses) {
               return FloatingActionButton(
                  onPressed: () {
                     Navigator.push(
                        context, MaterialPageRoute(
                           builder: (context)
                           => ScopedModelDescendant<ExpenseListModel>(
                              builder: (context, child, expenses) { 
                                 return FormPage( id: 0, expenses: expenses, ); 
                              }
                           )
                        )
                     );
                     // expenses.add(
                        new Expense(
                           // 2, 1000, DateTime.parse('2019-04-01 11:00:00'), 'Food'
                        )
                     );
                     // print(expenses.items.length); 
                  },
                  tooltip: 'Increment', child: Icon(Icons.add), 
               );
            }
         )
      );
   } 
}
class FormPage extends StatefulWidget {
   FormPage({Key key, this.id, this.expenses}) : super(key: key); 
   final int id; 
   final ExpenseListModel expenses; 
   
   @override 
   _FormPageState createState() => _FormPageState(id: id, expenses: expenses); 
}
class _FormPageState extends State<FormPage> {
   _FormPageState({Key key, this.id, this.expenses}); 
   final int id; 
   final ExpenseListModel expenses; 
   final scaffoldKey = GlobalKey<ScaffoldState>(); 
   final formKey = GlobalKey<FormState>(); 
   double _amount; DateTime _date; 
   String _category;
   void _submit() {
      final form = formKey.currentState; 
      if (form.validate()) {
         form.save(); 
         if (this.id == 0) expenses.add(Expense(0, _amount, _date, _category)); 
         else expenses.update(Expense(this.id, _amount, _date, _category)); 
         Navigator.pop(context); 
      } 
   } 
   @override 
   Widget build(BuildContext context) {
      return Scaffold(
         key: scaffoldKey, appBar: AppBar( 
            title: Text('Enter expense details'), 
         ), 
         body: Padding(
            padding: const EdgeInsets.all(16.0),
            child: Form(
               key: formKey, child: Column(
                  children: [
                     TextFormField(
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration( 
                           icon: const Icon(Icons.monetization_on), 
                           labelText: 'Amount', 
                           labelStyle: TextStyle(fontSize: 18)
                        ), 
                        validator: (val) {
                           Pattern pattern = r'^[1-9]\d*(\.\d+)?$'; 
                           RegExp regex = new RegExp(pattern); 
                           if (!regex.hasMatch(val)) return 'Enter a valid number'; 
                           else return null; 
                        },
                        initialValue: id == 0 ? '' 
                        : expenses.byId(id).amount.toString(), 
                        onSaved: (val) => _amount = double.parse(val), 
                     ),
                     TextFormField(
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration(
                           icon: const Icon(Icons.calendar_today), 
                           hintText: 'Enter date', 
                           labelText: 'Date', 
                           labelStyle: TextStyle(fontSize: 18), 
                        ),
                        validator: (val) {
                           Pattern pattern = r'^((?:19|20)\d\d)[- /.]
                           (0[1-9]|1[012])[- /.](0[1-9]|[12][0-9]|3[01])$'; 
                           RegExp regex = new RegExp(pattern); 
                           if (!regex.hasMatch(val)) return 'Enter a valid date'; 
                           else return null; 
                        },
                        onSaved: (val) => _date = DateTime.parse(val), 
                        initialValue: id == 0 ? '' : expenses.byId(id).formattedDate, 
                        keyboardType: TextInputType.datetime, 
                     ),
                     TextFormField(
                        style: TextStyle(fontSize: 22), 
                        decoration: const InputDecoration(
                           icon: const Icon(Icons.category), 
                           labelText: 'Category', 
                           labelStyle: TextStyle(fontSize: 18)
                        ), 
                        onSaved: (val) => _category = val, 
                        initialValue: id == 0 ? '' : expenses.byId(id).category.toString(), 
                     ),
                     RaisedButton(
                        onPressed: _submit, 
                        child: new Text('Submit'), 
                     ),
                  ],
               ),
            ),
         ),
      );
   }
}
Now, run the application.

Add new expenses using floating button.

Edit existing expenses by tapping the expense entry.

Delete the existing expenses by swiping the expense entry in either direction.

Some of the screen shots of the application are as follows −

Expense Calculator
Enter Expense Details
Total Expenses
Flutter - Conclusion
Flutter framework does a great job by providing an excellent framework to build mobile applications in a truly platform independent way. By providing simplicity in the development process, high performance in the resulting mobile application, rich and relevant user interface for both Android and iOS platform, Flutter framework will surely enable a lot of new developers to develop high performance and feature-full mobile application in the near future.
