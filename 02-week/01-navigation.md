# Navigation in Flutter

Navigation is a fundamental aspect of mobile app development. It allows users to move between different screens (pages) in your application. In this guide, you'll learn how to navigate between screens and pass data back and forth.

## Understanding Navigation in Flutter

Flutter uses a **Navigator** widget to manage a stack of routes (screens). Think of it like a stack of cards - you can push a new card on top or pop the top card off to reveal the one beneath.

### Key Concepts

- **Route**: Represents a screen or page in your app
- **Navigator**: Manages the stack of routes
- **Push**: Add a new route to the stack (navigate forward)
- **Pop**: Remove the current route from the stack (navigate back)
- **Context**: Required to access the Navigator

## Navigator Push and Pop

### Basic Navigation with Navigator.push()

The `Navigator.push()` method adds a new route to the navigation stack.

#### Syntax

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondScreen()),
);
```

#### Example: Simple Navigation

Let's create a simple two-screen app:

**main.dart**
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Navigation Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate to the second screen
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondScreen()),
            );
          },
          child: const Text('Go to Second Screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Screen'),
      ),
      body: Center(
        child: Text('This is the second screen'),
      ),
    );
  }
}
```

### Navigator.pop()

The `Navigator.pop()` method removes the current route from the stack, returning to the previous screen.

```dart
Navigator.pop(context);
```

#### Example: Manual Back Navigation

```dart
class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate back to the previous screen
            Navigator.pop(context);
          },
          child: const Text('Go Back'),
        ),
      ),
    );
  }
}
```

**Note:** The AppBar automatically includes a back button when there's a previous route in the stack. You don't need to add it manually.

### MaterialPageRoute

`MaterialPageRoute` is a modal route that replaces the entire screen with a platform-adaptive transition.

```dart
MaterialPageRoute(
  builder: (context) => const DestinationScreen(),
)
```

**Properties:**
- `builder`: A function that returns the widget to display
- `fullscreenDialog`: Set to `true` for modal presentation (default is `false`)

```dart
// Display as a fullscreen dialog
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => const SecondScreen(),
    fullscreenDialog: true,
  ),
);
```

## Sending Data Between Screens

### Passing Data to a New Screen

You can pass data to a new screen by providing it through the constructor.

#### Example: Passing a String

```dart
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const DetailScreen(
                  title: 'Hello from Home!',
                ),
              ),
            );
          },
          child: const Text('Go to Details'),
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  final String title;
  
  const DetailScreen({
    super.key,
    required this.title,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Text('Received: $title'),
      ),
    );
  }
}
```

#### Example: Passing Multiple Parameters

```dart
class User {
  final String name;
  final String email;
  final int age;

  User({
    required this.name,
    required this.email,
    required this.age,
  });
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final user = User(
      name: 'John Doe',
      email: 'john@example.com',
      age: 25,
    );

    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => UserProfileScreen(user: user),
              ),
            );
          },
          child: const Text('View Profile'),
        ),
      ),
    );
  }
}

class UserProfileScreen extends StatelessWidget {
  final User user;
  
  const UserProfileScreen({
    super.key,
    required this.user,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(user.name),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Name: ${user.name}',
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 8),
            Text(
              'Email: ${user.email}',
              style: const TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 8),
            Text(
              'Age: ${user.age}',
              style: const TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Returning Data from a Screen

You can return data from a screen when popping it from the navigation stack.

#### Using Navigator.pop() with a Result

```dart
// Pop with a result
Navigator.pop(context, 'Some result data');
```

#### Example: Selecting an Option

```dart
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  String selectedColor = 'None';

  Future<void> _navigateAndGetColor() async {
    // Navigate and wait for result
    final result = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => const SelectionScreen(),
      ),
    );

    // Update UI with the result
    if (result != null) {
      setState(() {
        selectedColor = result;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Selected Color: $selectedColor',
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _navigateAndGetColor,
              child: const Text('Select a Color'),
            ),
          ],
        ),
      ),
    );
  }
}

class SelectionScreen extends StatelessWidget {
  const SelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Select a Color'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
              onPressed: () {
                Navigator.pop(context, 'Red');
              },
              child: const Text('Red'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Colors.green),
              onPressed: () {
                Navigator.pop(context, 'Green');
              },
              child: const Text('Green'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Colors.blue),
              onPressed: () {
                Navigator.pop(context, 'Blue');
              },
              child: const Text('Blue'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Returning Complex Data

You can return any type of data, including custom objects:

```dart
class Product {
  final String name;
  final double price;

  Product({required this.name, required this.price});
}

class ProductSelectionScreen extends StatelessWidget {
  const ProductSelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Select a Product'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: const Text('Laptop'),
            subtitle: const Text('\$999'),
            onTap: () {
              Navigator.pop(
                context,
                Product(name: 'Laptop', price: 999),
              );
            },
          ),
          ListTile(
            title: const Text('Phone'),
            subtitle: const Text('\$699'),
            onTap: () {
              Navigator.pop(
                context,
                Product(name: 'Phone', price: 699),
              );
            },
          ),
        ],
      ),
    );
  }
}

// In the calling screen
Future<void> _selectProduct() async {
  final Product? selectedProduct = await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => const ProductSelectionScreen(),
    ),
  );

  if (selectedProduct != null) {
    print('Selected: ${selectedProduct.name} - \$${selectedProduct.price}');
  }
}
```

## Practical Examples

### Example 1: Todo Item Detail View

```dart
class TodoItem {
  final String title;
  final String description;
  final bool isCompleted;

  TodoItem({
    required this.title,
    required this.description,
    this.isCompleted = false,
  });
}

class TodoListScreen extends StatelessWidget {
  const TodoListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final todos = [
      TodoItem(
        title: 'Buy groceries',
        description: 'Milk, eggs, bread',
      ),
      TodoItem(
        title: 'Complete Flutter project',
        description: 'Finish navigation tutorial',
      ),
    ];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Todo List'),
      ),
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          final todo = todos[index];
          return ListTile(
            title: Text(todo.title),
            trailing: const Icon(Icons.chevron_right),
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => TodoDetailScreen(todo: todo),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class TodoDetailScreen extends StatelessWidget {
  final TodoItem todo;
  
  const TodoDetailScreen({
    super.key,
    required this.todo,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Description:',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            Text(
              todo.description,
              style: const TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 16),
            Text(
              'Status: ${todo.isCompleted ? "Completed" : "Pending"}',
              style: TextStyle(
                fontSize: 16,
                color: todo.isCompleted ? Colors.green : Colors.orange,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Example 2: Form with Result

```dart
class ContactFormScreen extends StatefulWidget {
  const ContactFormScreen({super.key});

  @override
  State<ContactFormScreen> createState() => _ContactFormScreenState();
}

class _ContactFormScreenState extends State<ContactFormScreen> {
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  void _submitForm() {
    final contact = {
      'name': _nameController.text,
      'email': _emailController.text,
    };
    
    // Return the contact data
    Navigator.pop(context, contact);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Add Contact'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _emailController,
              decoration: const InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 24),
            ElevatedButton(
              onPressed: _submitForm,
              child: const Text('Save Contact'),
            ),
          ],
        ),
      ),
    );
  }
}

// Usage in parent screen
Future<void> _addContact() async {
  final result = await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => const ContactFormScreen(),
    ),
  );

  if (result != null) {
    print('New contact: ${result['name']}, ${result['email']}');
    // Add contact to your list
  }
}
```

## Navigation Best Practices

### 1. Use const Constructors

When navigating to screens that don't take parameters:

```dart
// Good
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => const SettingsScreen()),
);

// Better - if SettingsScreen has a const constructor
const SettingsScreen()
```

### 2. Extract Routes to Separate Files

For better organization, create separate files for each screen:

```
lib/
├── screens/
│   ├── home_screen.dart
│   ├── detail_screen.dart
│   └── settings_screen.dart
└── main.dart
```

### 3. Handle Null Results

Always check if the result is null when popping with data:

```dart
final result = await Navigator.push(...);

if (result != null) {
  // Use the result
}
```

### 4. Use Meaningful Parameter Names

Make your code self-documenting:

```dart
// Good
class ProductDetailScreen extends StatelessWidget {
  final Product product;
  
  const ProductDetailScreen({
    super.key,
    required this.product,
  });
}

// Avoid
class ProductDetailScreen extends StatelessWidget {
  final Product p;  // Not descriptive
}
```

### 5. Consider Using Navigator 2.0 for Complex Apps

For apps with complex navigation requirements, consider using Navigator 2.0 (declarative navigation), but for most apps, the imperative approach shown here is sufficient.

## Common Navigation Patterns

### 1. Replace Current Screen

Remove the current route and add a new one:

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => const NewScreen()),
);
```

Use case: After login, replace the login screen with the home screen.

### 2. Clear All Routes and Navigate

Remove all routes and navigate to a new screen:

```dart
Navigator.pushAndRemoveUntil(
  context,
  MaterialPageRoute(builder: (context) => const HomeScreen()),
  (route) => false,  // Remove all routes
);
```

Use case: After logout, clear all screens and go to login.

### 3. Navigate Multiple Screens Back

```dart
Navigator.pop(context);
Navigator.pop(context);
// Or use Navigator.popUntil()
```

## Complete Working Example

Here's a complete example combining everything:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Navigation Complete Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  String message = 'No message yet';

  Future<void> _navigateToSecondScreen() async {
    final result = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => const SecondScreen(data: 'Hello from Home!'),
      ),
    );

    if (result != null) {
      setState(() {
        message = result;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Message: $message',
              style: const TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _navigateToSecondScreen,
              child: const Text('Go to Second Screen'),
            ),
          ],
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  final String data;
  
  const SecondScreen({
    super.key,
    required this.data,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Received: $data',
              style: const TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Hello from Second Screen!');
              },
              child: const Text('Send Data Back'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Key Takeaways

1. **Navigator.push()** adds a new screen to the navigation stack
2. **Navigator.pop()** removes the current screen and returns to the previous one
3. **Pass data forward** using constructor parameters
4. **Return data** using Navigator.pop() with a second argument
5. **Use async/await** when you need to receive data from a screen
6. **Always check for null** when receiving returned data
7. **MaterialPageRoute** is the standard route for Material Design apps

---

## References and Further Reading

### Navigation Basics
- [Navigation and Routing](https://docs.flutter.dev/development/ui/navigation) - Official Flutter navigation guide
- [Navigator Class](https://api.flutter.dev/flutter/widgets/Navigator-class.html) - Navigator API documentation
- [MaterialPageRoute](https://api.flutter.dev/flutter/material/MaterialPageRoute-class.html) - MaterialPageRoute API

### Advanced Navigation
- [Navigator 2.0](https://docs.flutter.dev/development/ui/navigation/deep-linking) - Declarative navigation
- [Named Routes](https://docs.flutter.dev/cookbook/navigation/named-routes) - Using named routes
- [Passing Arguments](https://docs.flutter.dev/cookbook/navigation/navigate-with-arguments) - Passing arguments to named routes

### Navigation Patterns
- [Navigation Cookbook](https://docs.flutter.dev/cookbook/navigation) - Common navigation patterns
- [Deep Linking](https://docs.flutter.dev/development/ui/navigation/deep-linking) - Handle deep links
