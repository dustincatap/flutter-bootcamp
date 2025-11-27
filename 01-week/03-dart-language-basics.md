# Dart Language Basics

Dart is the programming language that powers Flutter. It's a modern, object-oriented language with strong typing and built-in support for asynchronous programming.

## Variables, Types, and Null Safety

### Variables

Dart provides several ways to declare variables:

#### var keyword
The `var` keyword lets Dart infer the type from the assigned value:

```dart
var name = 'John';        // String type inferred
var age = 25;             // int type inferred
var height = 5.9;         // double type inferred
var isStudent = true;     // bool type inferred
```

#### Explicit Type Declaration
You can explicitly declare the type:

```dart
String name = 'John';
int age = 25;
double height = 5.9;
bool isStudent = true;
```

#### final and const
Use `final` for runtime constants and `const` for compile-time constants:

```dart
final String city = 'New York';     // Runtime constant
const double pi = 3.14159;          // Compile-time constant

final DateTime now = DateTime.now(); // OK: evaluated at runtime
// const DateTime now = DateTime.now(); // ERROR: not a compile-time constant
```

**Key Differences:**
- `final`: Value is set once at runtime and cannot be changed
- `const`: Value must be known at compile-time and is deeply immutable

### Data Types

Dart has several built-in types:

#### Numbers

```dart
int count = 42;                    // Integer
double price = 19.99;              // Double-precision floating point
num value = 100;                   // Can be int or double

// Number operations
int sum = 10 + 5;                  // 15
double average = 10 / 3;           // 3.3333...
int division = 10 ~/ 3;            // 3 (integer division)
int remainder = 10 % 3;            // 1
```

#### Strings

```dart
String firstName = 'John';
String lastName = "Doe";

// String interpolation
String fullName = '$firstName $lastName';           // 'John Doe'
String message = '${firstName.toUpperCase()} Doe';  // 'JOHN Doe'

// Multi-line strings
String multiLine = '''
This is a
multi-line
string
''';

// String concatenation
String greeting = 'Hello, ' + fullName;
```

#### Booleans

```dart
bool isActive = true;
bool isCompleted = false;

// Boolean operations
bool result = isActive && !isCompleted;  // true
bool check = isActive || isCompleted;    // true
```

#### Lists (Arrays)

```dart
// List with type inference
var numbers = [1, 2, 3, 4, 5];

// Explicitly typed list
List<String> fruits = ['Apple', 'Banana', 'Orange'];

// List operations
fruits.add('Mango');              // Add element
fruits.remove('Banana');          // Remove element
int length = fruits.length;       // Get length
String first = fruits[0];         // Access by index

// Spread operator
var moreFruits = ['Grape', ...fruits];

// Collection if
var cart = [
  'Apple',
  if (isStudent) 'Discount Coupon',
  'Banana'
];

// Collection for
var numbers2 = [
  for (var i = 1; i <= 5; i++) i * 2
]; // [2, 4, 6, 8, 10]
```

#### Maps (Objects)

```dart
// Map with type inference
var person = {
  'name': 'John',
  'age': 25,
  'city': 'New York'
};

// Explicitly typed map
Map<String, dynamic> user = {
  'id': 1,
  'username': 'johndoe',
  'isActive': true
};

// Map operations
user['email'] = 'john@example.com';  // Add/update
String? username = user['username'];  // Access value
user.remove('isActive');             // Remove entry
bool hasEmail = user.containsKey('email');
```

### Null Safety

Dart has sound null safety, meaning variables cannot contain `null` unless explicitly declared as nullable.

#### Non-nullable vs Nullable Types

```dart
// Non-nullable (default)
String name = 'John';
// name = null;  // ERROR: Can't assign null to non-nullable type

// Nullable (add ? after type)
String? nullableName = 'John';
nullableName = null;  // OK

int? age;  // Defaults to null
```

#### Null-aware Operators

```dart
String? name;

// Null-aware access (?.)
int? length = name?.length;  // null if name is null

// Null-aware assignment (??=)
name ??= 'Default Name';  // Assigns only if name is null

// Null coalescing operator (??)
String displayName = name ?? 'Guest';  // Use 'Guest' if name is null

// Null assertion operator (!)
// Use with caution - throws error if null
String definitelyNotNull = name!;  // Asserts name is not null
```

#### Late Variables

Use `late` for variables that will be initialized later but before they're used:

```dart
late String description;

void initialize() {
  description = 'Initialized later';
}

void display() {
  initialize();
  print(description);  // OK: initialized before use
}

// Also useful for lazy initialization
late String expensiveData = fetchDataFromServer();  // Only called when accessed
```

### Type Checking and Casting

```dart
dynamic value = 'Hello';

// Type checking
if (value is String) {
  print(value.length);  // Automatically promoted to String
}

// Type casting
String text = value as String;

// Safe casting
String? maybeText = value is String ? value : null;
```

## Functions and Classes

### Functions

#### Basic Function Declaration

```dart
// Function with return type
String greet(String name) {
  return 'Hello, $name!';
}

// Function with no return value
void printMessage(String message) {
  print(message);
}

// Call functions
String greeting = greet('John');
printMessage(greeting);
```

#### Arrow Functions (Fat Arrow)

```dart
// Single expression function
String greet(String name) => 'Hello, $name!';

// With multiple parameters
int add(int a, int b) => a + b;

// Use in higher-order functions
var numbers = [1, 2, 3, 4, 5];
var doubled = numbers.map((n) => n * 2).toList();
```

#### Optional Parameters

```dart
// Optional positional parameters (in [])
String greet(String name, [String? title]) {
  if (title != null) {
    return 'Hello, $title $name!';
  }
  return 'Hello, $name!';
}

greet('John');              // 'Hello, John!'
greet('John', 'Dr.');       // 'Hello, Dr. John!'

// Optional named parameters (in {})
void printUser({String? name, int? age, String? city}) {
  print('Name: $name, Age: $age, City: $city');
}

printUser(name: 'John', age: 25);
printUser(city: 'New York', name: 'Jane');

// Required named parameters
void createUser({required String name, required String email}) {
  print('User: $name, Email: $email');
}

createUser(name: 'John', email: 'john@example.com');

// Default parameter values
void greetUser({String name = 'Guest', int age = 0}) {
  print('Hello, $name! Age: $age');
}

greetUser();                      // Uses defaults
greetUser(name: 'John', age: 25);
```

#### Higher-Order Functions

```dart
// Function as parameter
void executeFunction(int a, int b, int Function(int, int) operation) {
  print(operation(a, b));
}

int add(int a, int b) => a + b;
int multiply(int a, int b) => a * b;

executeFunction(5, 3, add);       // 8
executeFunction(5, 3, multiply);  // 15

// Returning a function
Function makeMultiplier(int factor) {
  return (int value) => value * factor;
}

var doubler = makeMultiplier(2);
print(doubler(5));  // 10
```

### Classes

#### Basic Class Declaration

```dart
class Person {
  // Properties
  String name;
  int age;
  
  // Constructor
  Person(this.name, this.age);
  
  // Method
  void introduce() {
    print('Hi, I am $name and I am $age years old.');
  }
}

// Create instance
var person = Person('John', 25);
person.introduce();
```

#### Named Constructors

```dart
class Person {
  String name;
  int age;
  
  // Default constructor
  Person(this.name, this.age);
  
  // Named constructor
  Person.guest() : name = 'Guest', age = 0;
  
  // Named constructor from JSON
  Person.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        age = json['age'];
}

var person1 = Person('John', 25);
var person2 = Person.guest();
var person3 = Person.fromJson({'name': 'Jane', 'age': 30});
```

#### Getters and Setters

```dart
class Rectangle {
  double width;
  double height;
  
  Rectangle(this.width, this.height);
  
  // Getter
  double get area => width * height;
  
  // Setter
  set dimensions(List<double> dims) {
    width = dims[0];
    height = dims[1];
  }
}

var rect = Rectangle(10, 5);
print(rect.area);  // 50 (using getter)

rect.dimensions = [20, 10];  // Using setter
print(rect.area);  // 200
```

#### Private Members

In Dart, prefix with underscore `_` to make members private to the library:

```dart
class BankAccount {
  String _accountNumber;  // Private
  double _balance;        // Private
  
  BankAccount(this._accountNumber, this._balance);
  
  // Public getter
  double get balance => _balance;
  
  // Public method
  void deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
    }
  }
  
  // Private method
  void _logTransaction(String type, double amount) {
    print('$type: \$$amount');
  }
}
```

#### Inheritance

```dart
// Parent class
class Animal {
  String name;
  
  Animal(this.name);
  
  void makeSound() {
    print('Some sound');
  }
}

// Child class
class Dog extends Animal {
  String breed;
  
  Dog(String name, this.breed) : super(name);
  
  @override
  void makeSound() {
    print('Woof! Woof!');
  }
  
  void fetch() {
    print('$name is fetching the ball');
  }
}

var dog = Dog('Buddy', 'Golden Retriever');
dog.makeSound();  // 'Woof! Woof!'
dog.fetch();      // 'Buddy is fetching the ball'
```

#### Abstract Classes and Interfaces

```dart
// Abstract class
abstract class Shape {
  // Abstract method (must be implemented by subclasses)
  double calculateArea();
  
  // Concrete method
  void display() {
    print('Area: ${calculateArea()}');
  }
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius);
  
  @override
  double calculateArea() {
    return 3.14159 * radius * radius;
  }
}

// Implements (interface-like behavior)
class Printable {
  void printData() {}
}

class Document implements Printable {
  @override
  void printData() {
    print('Printing document...');
  }
}
```

#### Mixins

Mixins allow you to reuse code across multiple class hierarchies:

```dart
// Mixin
mixin Swimmer {
  void swim() {
    print('Swimming...');
  }
}

mixin Flyer {
  void fly() {
    print('Flying...');
  }
}

class Animal {
  String name;
  Animal(this.name);
}

// Use multiple mixins
class Duck extends Animal with Swimmer, Flyer {
  Duck(String name) : super(name);
}

var duck = Duck('Donald');
duck.swim();  // 'Swimming...'
duck.fly();   // 'Flying...'
```

## Async Programming and Futures

Asynchronous programming is essential for handling operations that take time, like network requests or file I/O.

### Future

A `Future` represents a value that will be available at some point in the future.

#### Basic Future Usage

```dart
// Function that returns a Future
Future<String> fetchUserData() {
  // Simulating network delay
  return Future.delayed(
    Duration(seconds: 2),
    () => 'User data loaded'
  );
}

// Using .then()
void getUserData() {
  print('Fetching user data...');
  fetchUserData().then((data) {
    print(data);  // 'User data loaded' (after 2 seconds)
  });
  print('Request sent');
}
```

### Async/Await

The `async` and `await` keywords make asynchronous code look and behave like synchronous code:

```dart
// async function returns a Future
Future<void> getUserData() async {
  print('Fetching user data...');
  
  // await pauses execution until Future completes
  String data = await fetchUserData();
  print(data);
  
  print('Data processed');
}

// Multiple async operations
Future<void> loadUserProfile() async {
  String userData = await fetchUserData();
  String userPosts = await fetchUserPosts();
  String userComments = await fetchUserComments();
  
  print('All data loaded');
}
```

### Error Handling

```dart
Future<String> fetchData() async {
  throw Exception('Network error');
}

// Using try-catch
Future<void> getData() async {
  try {
    String data = await fetchData();
    print(data);
  } catch (e) {
    print('Error: $e');
  } finally {
    print('Cleanup');
  }
}

// Using .catchError()
void getDataAlt() {
  fetchData()
    .then((data) => print(data))
    .catchError((error) => print('Error: $error'))
    .whenComplete(() => print('Cleanup'));
}
```

### Parallel Async Operations

```dart
// Wait for all futures to complete
Future<void> loadAllData() async {
  var results = await Future.wait([
    fetchUserData(),
    fetchUserPosts(),
    fetchUserComments(),
  ]);
  
  print('User: ${results[0]}');
  print('Posts: ${results[1]}');
  print('Comments: ${results[2]}');
}

// Execute with timeout
Future<void> loadWithTimeout() async {
  try {
    String data = await fetchUserData().timeout(Duration(seconds: 5));
    print(data);
  } catch (e) {
    print('Request timed out or failed');
  }
}
```

### Stream

Streams provide a sequence of asynchronous events:

```dart
// Creating a stream
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;  // Emit value
  }
}

// Listening to a stream
void listenToStream() {
  countStream(5).listen(
    (value) => print('Value: $value'),
    onError: (error) => print('Error: $error'),
    onDone: () => print('Stream completed'),
  );
}

// Using await for
Future<void> processStream() async {
  await for (int value in countStream(5)) {
    print('Processing: $value');
  }
  print('All values processed');
}

// Transform stream
Stream<int> doubledStream = countStream(5).map((value) => value * 2);
```

### Practical Async Example

```dart
// Simulating API calls
class ApiService {
  Future<Map<String, dynamic>> login(String email, String password) async {
    await Future.delayed(Duration(seconds: 2));
    
    if (email == 'user@example.com' && password == 'password') {
      return {'token': 'abc123', 'userId': 1};
    }
    throw Exception('Invalid credentials');
  }
  
  Future<Map<String, dynamic>> getUserProfile(String token) async {
    await Future.delayed(Duration(seconds: 1));
    return {
      'name': 'John Doe',
      'email': 'user@example.com',
      'age': 25
    };
  }
}

// Using the API
Future<void> userLogin() async {
  final api = ApiService();
  
  try {
    print('Logging in...');
    final authData = await api.login('user@example.com', 'password');
    print('Login successful! Token: ${authData['token']}');
    
    print('Fetching profile...');
    final profile = await api.getUserProfile(authData['token']);
    print('Welcome, ${profile['name']}!');
  } catch (e) {
    print('Error: $e');
  }
}
```

---

## References and Further Reading

### Dart Language Fundamentals
- [Dart Language Tour](https://dart.dev/guides/language/language-tour) - Comprehensive guide to Dart syntax and features
- [Dart Language Samples](https://dart.dev/samples) - Code examples for common Dart patterns
- [Effective Dart](https://dart.dev/guides/language/effective-dart) - Best practices for writing Dart code
- [Dart Type System](https://dart.dev/guides/language/type-system) - Understanding Dart's sound type system

### Variables and Types
- [Dart Variables](https://dart.dev/guides/language/language-tour#variables) - Variable declaration and usage
- [Built-in Types](https://dart.dev/guides/language/language-tour#built-in-types) - Overview of Dart's core types
- [Null Safety](https://dart.dev/null-safety) - Understanding null safety in Dart
- [Null Safety Codelab](https://dart.dev/codelabs/null-safety) - Interactive null safety tutorial

### Functions and Classes
- [Functions](https://dart.dev/guides/language/language-tour#functions) - Function syntax and features
- [Classes](https://dart.dev/guides/language/language-tour#classes) - Object-oriented programming in Dart
- [Constructors](https://dart.dev/guides/language/language-tour#constructors) - Different types of constructors
- [Mixins](https://dart.dev/guides/language/language-tour#adding-features-to-a-class-mixins) - Code reuse with mixins
- [Extension Methods](https://dart.dev/guides/language/extension-methods) - Adding functionality to existing classes

### Asynchronous Programming
- [Asynchronous Programming](https://dart.dev/codelabs/async-await) - Async/await codelab
- [Futures](https://dart.dev/guides/libraries/library-tour#future) - Working with Future objects
- [Streams](https://dart.dev/guides/libraries/library-tour#stream) - Understanding and using streams
- [Dart Asynchrony Support](https://dart.dev/guides/language/language-tour#asynchrony-support) - Language-level async features

### Collections and Generics
- [Collections](https://dart.dev/guides/libraries/library-tour#collections) - Lists, sets, and maps
- [Generics](https://dart.dev/guides/language/language-tour#generics) - Type-safe collections and classes
- [Iterable Collections](https://dart.dev/codelabs/iterables) - Working with iterable collections

### Practice and Tools
- [DartPad](https://dartpad.dev/) - Online Dart editor for practicing
- [Dart API Documentation](https://api.dart.dev/) - Complete API reference
- [Dart Cheatsheet](https://dart.dev/codelabs/dart-cheatsheet) - Quick reference for Dart syntax
