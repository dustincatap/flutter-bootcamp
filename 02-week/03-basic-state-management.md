# Basic State Management in Flutter

State management is one of the most important concepts in Flutter development. Understanding how to manage state effectively will help you build interactive and responsive applications.

## Understanding State

**State** is information that can change over time and affects what is displayed on the screen. When state changes, Flutter rebuilds the affected parts of the UI to reflect those changes.

### Types of State

1. **Ephemeral State (Local State)**: State that only affects a single widget
   - Examples: Current page in a PageView, selected item in a list, animation state
   - Managed using `setState()`

2. **App State (Shared State)**: State that affects multiple parts of your app
   - Examples: User authentication status, shopping cart items, app preferences
   - Requires state management solutions (covered in later weeks)

In this guide, we'll focus on **ephemeral state** and how to manage it with `setState()`.

## Using setState()

The `setState()` method tells Flutter that the internal state of a widget has changed and the UI needs to be updated.

### How setState() Works

1. You call `setState()` with a function that modifies your state variables
2. Flutter marks the widget as "dirty" (needs rebuilding)
3. Flutter calls the `build()` method again
4. The widget tree is updated with the new state

### Basic setState() Example

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
      title: 'setState Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const CounterScreen(),
    );
  }
}

class CounterScreen extends StatefulWidget {
  const CounterScreen({super.key});

  @override
  State<CounterScreen> createState() => _CounterScreenState();
}

class _CounterScreenState extends State<CounterScreen> {
  // State variable
  int _counter = 0;

  // Method to update state
  void _incrementCounter() {
    setState(() {
      // Modify state inside setState
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    print('Building CounterScreen'); // Shows when widget rebuilds
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Counter Example'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'You have pushed the button this many times:',
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 8),
            Text(
              '$_counter',
              style: const TextStyle(
                fontSize: 48,
                fontWeight: FontWeight.bold,
              ),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

### setState() Rules

#### 1. Only Use setState() in StatefulWidgets

```dart
// ❌ WRONG - StatelessWidget cannot use setState
class MyWidget extends StatelessWidget {
  int counter = 0;
  
  void increment() {
    setState(() {  // ERROR: setState not available
      counter++;
    });
  }
}

// ✅ CORRECT - StatefulWidget can use setState
class MyWidget extends StatefulWidget {
  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  int counter = 0;
  
  void increment() {
    setState(() {
      counter++;
    });
  }
}
```

#### 2. Always Modify State Inside setState()

```dart
// ❌ WRONG - State modified outside setState
void _incrementCounter() {
  _counter++;  // Won't trigger rebuild
  setState(() {});
}

// ✅ CORRECT - State modified inside setState
void _incrementCounter() {
  setState(() {
    _counter++;  // Triggers rebuild
  });
}
```

#### 3. Don't Call Async Operations Directly in setState()

```dart
// ❌ WRONG - Async operation in setState
void _loadData() {
  setState(() async {  // Don't do this
    final data = await fetchData();
    _data = data;
  });
}

// ✅ CORRECT - Async operation outside setState
Future<void> _loadData() async {
  final data = await fetchData();
  setState(() {
    _data = data;  // Only update state in setState
  });
}
```

### Multiple State Variables

You can manage multiple pieces of state in a single widget:

```dart
class MultiStateExample extends StatefulWidget {
  const MultiStateExample({super.key});

  @override
  State<MultiStateExample> createState() => _MultiStateExampleState();
}

class _MultiStateExampleState extends State<MultiStateExample> {
  // Multiple state variables
  int _counter = 0;
  String _name = 'Guest';
  bool _isLiked = false;
  Color _backgroundColor = Colors.white;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  void _updateName(String newName) {
    setState(() {
      _name = newName;
    });
  }

  void _toggleLike() {
    setState(() {
      _isLiked = !_isLiked;
    });
  }

  void _changeColor() {
    setState(() {
      _backgroundColor = _backgroundColor == Colors.white
          ? Colors.blue[50]!
          : Colors.white;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: _backgroundColor,
      appBar: AppBar(
        title: const Text('Multiple State Example'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Hello, $_name!',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 16),
            Text(
              'Counter: $_counter',
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 16),
            IconButton(
              icon: Icon(
                _isLiked ? Icons.favorite : Icons.favorite_border,
                color: _isLiked ? Colors.red : Colors.grey,
                size: 40,
              ),
              onPressed: _toggleLike,
            ),
            const SizedBox(height: 32),
            ElevatedButton(
              onPressed: _incrementCounter,
              child: const Text('Increment Counter'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: () => _updateName('Flutter Developer'),
              child: const Text('Update Name'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _changeColor,
              child: const Text('Change Background'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Practical setState() Examples

#### Example 1: Todo List

```dart
class TodoListScreen extends StatefulWidget {
  const TodoListScreen({super.key});

  @override
  State<TodoListScreen> createState() => _TodoListScreenState();
}

class _TodoListScreenState extends State<TodoListScreen> {
  final List<String> _todos = [];
  final TextEditingController _controller = TextEditingController();

  void _addTodo() {
    if (_controller.text.isNotEmpty) {
      setState(() {
        _todos.add(_controller.text);
      });
      _controller.clear();
    }
  }

  void _removeTodo(int index) {
    setState(() {
      _todos.removeAt(index);
    });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todo List'),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _controller,
                    decoration: const InputDecoration(
                      labelText: 'New Todo',
                      border: OutlineInputBorder(),
                    ),
                    onSubmitted: (_) => _addTodo(),
                  ),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: _addTodo,
                  child: const Text('Add'),
                ),
              ],
            ),
          ),
          Expanded(
            child: _todos.isEmpty
                ? const Center(
                    child: Text('No todos yet!'),
                  )
                : ListView.builder(
                    itemCount: _todos.length,
                    itemBuilder: (context, index) {
                      return ListTile(
                        title: Text(_todos[index]),
                        trailing: IconButton(
                          icon: const Icon(Icons.delete),
                          onPressed: () => _removeTodo(index),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }
}
```

#### Example 2: Toggle Switch with Multiple Options

```dart
class SettingsScreen extends StatefulWidget {
  const SettingsScreen({super.key});

  @override
  State<SettingsScreen> createState() => _SettingsScreenState();
}

class _SettingsScreenState extends State<SettingsScreen> {
  bool _notificationsEnabled = true;
  bool _darkModeEnabled = false;
  double _fontSize = 14.0;
  String _selectedLanguage = 'English';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: ListView(
        children: [
          SwitchListTile(
            title: const Text('Enable Notifications'),
            subtitle: const Text('Receive push notifications'),
            value: _notificationsEnabled,
            onChanged: (value) {
              setState(() {
                _notificationsEnabled = value;
              });
            },
          ),
          SwitchListTile(
            title: const Text('Dark Mode'),
            subtitle: const Text('Use dark theme'),
            value: _darkModeEnabled,
            onChanged: (value) {
              setState(() {
                _darkModeEnabled = value;
              });
            },
          ),
          ListTile(
            title: const Text('Font Size'),
            subtitle: Text('${_fontSize.toInt()}'),
          ),
          Slider(
            value: _fontSize,
            min: 10.0,
            max: 24.0,
            divisions: 14,
            label: _fontSize.toInt().toString(),
            onChanged: (value) {
              setState(() {
                _fontSize = value;
              });
            },
          ),
          ListTile(
            title: const Text('Language'),
            subtitle: Text(_selectedLanguage),
            trailing: const Icon(Icons.arrow_forward_ios),
            onTap: () {
              // Show language selection dialog
            },
          ),
        ],
      ),
    );
  }
}
```

#### Example 3: Form with Dynamic Fields

```dart
class DynamicFormScreen extends StatefulWidget {
  const DynamicFormScreen({super.key});

  @override
  State<DynamicFormScreen> createState() => _DynamicFormScreenState();
}

class _DynamicFormScreenState extends State<DynamicFormScreen> {
  final List<TextEditingController> _controllers = [];
  
  void _addField() {
    setState(() {
      _controllers.add(TextEditingController());
    });
  }
  
  void _removeField(int index) {
    setState(() {
      _controllers[index].dispose();
      _controllers.removeAt(index);
    });
  }
  
  @override
  void dispose() {
    for (var controller in _controllers) {
      controller.dispose();
    }
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Dynamic Form'),
      ),
      body: Column(
        children: [
          Expanded(
            child: ListView.builder(
              itemCount: _controllers.length,
              padding: const EdgeInsets.all(16),
              itemBuilder: (context, index) {
                return Padding(
                  padding: const EdgeInsets.only(bottom: 16),
                  child: Row(
                    children: [
                      Expanded(
                        child: TextField(
                          controller: _controllers[index],
                          decoration: InputDecoration(
                            labelText: 'Field ${index + 1}',
                            border: const OutlineInputBorder(),
                          ),
                        ),
                      ),
                      IconButton(
                        icon: const Icon(Icons.remove_circle),
                        onPressed: () => _removeField(index),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: ElevatedButton.icon(
              onPressed: _addField,
              icon: const Icon(Icons.add),
              label: const Text('Add Field'),
            ),
          ),
        ],
      ),
    );
  }
}
```

## Lifting State Up

**Lifting state up** is a pattern where you move state from a child widget to a parent widget so that multiple children can access and modify the same state.

### When to Lift State Up

Lift state up when:
1. Multiple widgets need to access the same state
2. Sibling widgets need to share data
3. A parent needs to control child widget state
4. You want to synchronize state across multiple widgets

### Basic Example: Sharing State Between Siblings

```dart
// Parent widget holds the state
class ParentWidget extends StatefulWidget {
  const ParentWidget({super.key});

  @override
  State<ParentWidget> createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget> {
  // State is lifted up to parent
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  void _decrement() {
    setState(() {
      _counter--;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Lifting State Up'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Display widget shows the state
            CounterDisplay(counter: _counter),
            const SizedBox(height: 32),
            // Control widgets modify the state through callbacks
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                CounterButton(
                  label: '-',
                  onPressed: _decrement,
                ),
                const SizedBox(width: 16),
                CounterButton(
                  label: '+',
                  onPressed: _increment,
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

// Child widget that displays state (StatelessWidget)
class CounterDisplay extends StatelessWidget {
  final int counter;

  const CounterDisplay({
    super.key,
    required this.counter,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        const Text(
          'Counter Value:',
          style: TextStyle(fontSize: 20),
        ),
        const SizedBox(height: 8),
        Text(
          '$counter',
          style: const TextStyle(
            fontSize: 48,
            fontWeight: FontWeight.bold,
          ),
        ),
      ],
    );
  }
}

// Child widget that modifies state through callback (StatelessWidget)
class CounterButton extends StatelessWidget {
  final String label;
  final VoidCallback onPressed;

  const CounterButton({
    super.key,
    required this.label,
    required this.onPressed,
  });

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        padding: const EdgeInsets.symmetric(horizontal: 32, vertical: 16),
      ),
      child: Text(
        label,
        style: const TextStyle(fontSize: 24),
      ),
    );
  }
}
```

### Key Concepts of Lifting State Up

1. **State lives in the parent**: The parent widget manages the state
2. **Children receive data via parameters**: Pass state down as constructor parameters
3. **Children communicate via callbacks**: Pass functions down for children to call
4. **Children can be stateless**: Since they don't manage state themselves

### Practical Example: Shopping Cart

```dart
class Product {
  final String name;
  final double price;
  
  Product({required this.name, required this.price});
}

// Parent widget managing cart state
class ShoppingApp extends StatefulWidget {
  const ShoppingApp({super.key});

  @override
  State<ShoppingApp> createState() => _ShoppingAppState();
}

class _ShoppingAppState extends State<ShoppingApp> {
  final List<Product> _availableProducts = [
    Product(name: 'Laptop', price: 999.99),
    Product(name: 'Mouse', price: 29.99),
    Product(name: 'Keyboard', price: 79.99),
    Product(name: 'Monitor', price: 299.99),
  ];

  final List<Product> _cart = [];

  void _addToCart(Product product) {
    setState(() {
      _cart.add(product);
    });
  }

  void _removeFromCart(int index) {
    setState(() {
      _cart.removeAt(index);
    });
  }

  double get _totalPrice {
    return _cart.fold(0, (sum, product) => sum + product.price);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping Cart'),
      ),
      body: Row(
        children: [
          // Product list (left side)
          Expanded(
            child: ProductList(
              products: _availableProducts,
              onAddToCart: _addToCart,
            ),
          ),
          // Cart (right side)
          Expanded(
            child: CartView(
              cartItems: _cart,
              totalPrice: _totalPrice,
              onRemoveFromCart: _removeFromCart,
            ),
          ),
        ],
      ),
    );
  }
}

// Product list widget
class ProductList extends StatelessWidget {
  final List<Product> products;
  final Function(Product) onAddToCart;

  const ProductList({
    super.key,
    required this.products,
    required this.onAddToCart,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        const Padding(
          padding: EdgeInsets.all(16.0),
          child: Text(
            'Products',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
        ),
        Expanded(
          child: ListView.builder(
            itemCount: products.length,
            itemBuilder: (context, index) {
              final product = products[index];
              return ListTile(
                title: Text(product.name),
                subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
                trailing: ElevatedButton(
                  onPressed: () => onAddToCart(product),
                  child: const Text('Add'),
                ),
              );
            },
          ),
        ),
      ],
    );
  }
}

// Cart view widget
class CartView extends StatelessWidget {
  final List<Product> cartItems;
  final double totalPrice;
  final Function(int) onRemoveFromCart;

  const CartView({
    super.key,
    required this.cartItems,
    required this.totalPrice,
    required this.onRemoveFromCart,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        const Padding(
          padding: EdgeInsets.all(16.0),
          child: Text(
            'Cart',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
        ),
        Expanded(
          child: cartItems.isEmpty
              ? const Center(
                  child: Text('Cart is empty'),
                )
              : ListView.builder(
                  itemCount: cartItems.length,
                  itemBuilder: (context, index) {
                    final product = cartItems[index];
                    return ListTile(
                      title: Text(product.name),
                      subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
                      trailing: IconButton(
                        icon: const Icon(Icons.remove_circle),
                        onPressed: () => onRemoveFromCart(index),
                      ),
                    );
                  },
                ),
        ),
        Container(
          padding: const EdgeInsets.all(16.0),
          decoration: BoxDecoration(
            color: Colors.grey[200],
            border: Border(
              top: BorderSide(color: Colors.grey[400]!),
            ),
          ),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              const Text(
                'Total:',
                style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
              ),
              Text(
                '\$${totalPrice.toStringAsFixed(2)}',
                style: const TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                  color: Colors.green,
                ),
              ),
            ],
          ),
        ),
      ],
    );
  }
}
```

### Callbacks for Complex Data

When you need to pass more information back to the parent:

```dart
// Define a callback type
typedef OnTaskChanged = void Function(String taskId, bool isCompleted);

class TaskList extends StatelessWidget {
  final List<Task> tasks;
  final OnTaskChanged onTaskChanged;

  const TaskList({
    super.key,
    required this.tasks,
    required this.onTaskChanged,
  });

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: tasks.length,
      itemBuilder: (context, index) {
        final task = tasks[index];
        return CheckboxListTile(
          title: Text(task.title),
          value: task.isCompleted,
          onChanged: (value) {
            onTaskChanged(task.id, value ?? false);
          },
        );
      },
    );
  }
}

class Task {
  final String id;
  final String title;
  final bool isCompleted;

  Task({
    required this.id,
    required this.title,
    required this.isCompleted,
  });
}
```

## Best Practices

### 1. Keep State as Low as Possible

Only lift state up when necessary. If only one widget needs the state, keep it local.

```dart
// ✅ GOOD - State stays local
class LocalStateExample extends StatefulWidget {
  @override
  State<LocalStateExample> createState() => _LocalStateExampleState();
}

class _LocalStateExampleState extends State<LocalStateExample> {
  bool _isExpanded = false;  // Only this widget needs it
  
  @override
  Widget build(BuildContext context) {
    return ExpansionTile(
      title: Text('Item'),
      // State used only here
    );
  }
}
```

### 2. Use Callbacks for Upward Communication

Children should communicate with parents through callbacks, not by directly modifying parent state.

```dart
// ✅ GOOD - Using callbacks
class ChildWidget extends StatelessWidget {
  final VoidCallback onPressed;
  
  const ChildWidget({super.key, required this.onPressed});
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,  // Call the callback
      child: const Text('Press Me'),
    );
  }
}
```

### 3. Minimize setState() Scope

Only rebuild what needs to be rebuilt:

```dart
// ❌ AVOID - Rebuilding entire screen
class _MyScreenState extends State<MyScreen> {
  int _counter = 0;
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(  // Entire scaffold rebuilds on counter change
      body: Column(
        children: [
          ExpensiveWidget(),  // Rebuilds unnecessarily
          Text('$_counter'),
        ],
      ),
    );
  }
}

// ✅ BETTER - Extract stateful part
class _MyScreenState extends State<MyScreen> {
  int _counter = 0;
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          const ExpensiveWidget(),  // const, won't rebuild
          CounterDisplay(counter: _counter),  // Only this rebuilds
        ],
      ),
    );
  }
}
```

### 4. Use const Constructors

Mark widgets as `const` when they don't depend on state:

```dart
@override
Widget build(BuildContext context) {
  return Column(
    children: [
      const Text('Static text'),  // Won't rebuild
      Text('$_counter'),  // Will rebuild
    ],
  );
}
```

### 5. Avoid Deeply Nested Callbacks

If you're passing callbacks through many levels, consider using state management solutions (covered in Week 4).

```dart
// ❌ AVOID - Callback hell
Parent -> Child -> GrandChild -> GreatGrandChild
  (passing callbacks through each level)

// ✅ BETTER - Use state management (Provider, Bloc, etc.)
// Will be covered in Week 4
```

## setState() vs State Management Solutions

### When to Use setState()

- ✅ Simple local state (counters, toggles, form fields)
- ✅ State used by a single widget or small widget subtree
- ✅ Temporary UI state (animations, scroll position)
- ✅ State that doesn't need to persist

### When to Use State Management Solutions

- ✅ State shared across many widgets
- ✅ Complex state logic
- ✅ State that needs to persist
- ✅ App-wide state (user authentication, theme, etc.)

**Note:** Advanced state management (Provider, Bloc, Riverpod) will be covered in Week 4.

## Key Takeaways

1. **setState()** is used to update the UI when state changes
2. Always modify state **inside** the setState() callback
3. Only **StatefulWidgets** can use setState()
4. **Lift state up** when multiple widgets need to share state
5. Use **callbacks** to communicate from child to parent
6. Keep state **as low as possible** in the widget tree
7. Use **const** constructors for widgets that don't change
8. **Minimize setState() scope** to avoid unnecessary rebuilds
9. setState() is perfect for **local/ephemeral state**
10. Consider state management solutions for **complex/shared state**

---

## References and Further Reading

### State Management Basics
- [State Management](https://docs.flutter.dev/development/data-and-backend/state-mgmt/intro) - Official state management guide
- [setState Method](https://api.flutter.dev/flutter/widgets/State/setState.html) - setState API documentation
- [StatefulWidget Class](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html) - StatefulWidget documentation

### State Management Patterns
- [Ephemeral vs App State](https://docs.flutter.dev/development/data-and-backend/state-mgmt/ephemeral-vs-app) - Understanding different types of state
- [Simple App State Management](https://docs.flutter.dev/development/data-and-backend/state-mgmt/simple) - Basic state management approaches
- [List of State Management Approaches](https://docs.flutter.dev/development/data-and-backend/state-mgmt/options) - Overview of different solutions

### Best Practices
- [Performance Best Practices](https://docs.flutter.dev/perf/best-practices) - Optimizing Flutter apps
- [Widget Lifecycle](https://api.flutter.dev/flutter/widgets/State-class.html) - Understanding State lifecycle
