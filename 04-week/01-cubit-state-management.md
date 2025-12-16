# State Management with Cubit

Welcome to Week 4! In Week 2, you learned about basic state management using `setState()` for local widget state. Now, we'll explore **Cubit**, a powerful and simple state management solution for managing complex, app-wide state in Flutter applications.

**Note:** Cubit is part of the `flutter_bloc` package and provides a simpler, more straightforward approach to state management compared to the full Bloc pattern. We'll focus primarily on Cubit in this guide.

## Review: Why Move Beyond setState()?

In Week 2, you learned that `setState()` is perfect for:
- Simple local state (counters, toggles)
- State used by a single widget
- Temporary UI state

However, as apps grow, you'll encounter scenarios where `setState()` becomes challenging:
- ❌ State shared across many widgets (passing data through multiple levels)
- ❌ Complex state logic mixed with UI code
- ❌ Difficult to test business logic
- ❌ Callback hell (passing functions through many widget layers)
- ❌ Hard to track state changes and debug

This is where **Cubit** comes in!

## What is Cubit?

**Cubit** is a lightweight state management solution that separates business logic from UI, making your code more:
- **Testable**: Business logic is isolated and easy to test
- **Reusable**: Logic can be shared across multiple screens
- **Predictable**: Clear flow from method calls to states
- **Maintainable**: Separation of concerns makes code easier to understand
- **Simple**: Direct method calls without complex event handling

### Cubit Architecture

```
┌─────────────┐
│     UI      │  (Widgets)
│  (Screen)   │
└──────┬──────┘
       │ 1. User Action (Button Press)
       ▼
┌─────────────┐
│   Method    │  increment()
│    Call     │
└──────┬──────┘
       │ 2. Method called on Cubit
       ▼
┌─────────────┐
│   Cubit     │  (Business Logic)
│  (Counter   │
│   Cubit)    │
└──────┬──────┘
       │ 3. Cubit emits new state
       ▼
┌─────────────┐
│    State    │  (CounterUpdated)
└──────┬──────┘
       │ 4. New state emitted
       ▼
┌─────────────┐
│     UI      │  (Rebuilds with new state)
│  (Screen)   │
└─────────────┘
```

### Key Concepts

1. **State**: Represents the current state of your application
2. **Cubit**: Contains business logic and emits states
3. **Methods**: Direct function calls to change state (e.g., `increment()`, `addTodo()`)
4. **Streams**: Cubits use streams to emit states over time
5. **Emit**: The function used to output new states

### Why Cubit?

Cubit is **simpler than full Bloc** because:
- ✅ No need to define separate event classes
- ✅ Direct method calls instead of event dispatching
- ✅ Less boilerplate code
- ✅ Easier to understand and learn
- ✅ Perfect for most real-world applications

**For this bootcamp, we'll focus on Cubit** as it provides the right balance of simplicity and power for building production-ready Flutter applications.

---

## Getting Started with Cubit

Let's recreate the counter example from Week 2, but with Cubit!

### Step 1: Add Dependencies

Add to `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_bloc: ^8.1.3
```

Run:
```bash
flutter pub get
```

### Step 2: Create State Class

```dart
// counter_state.dart
abstract class CounterState {
  final int count;
  
  const CounterState(this.count);
}

class CounterInitial extends CounterState {
  const CounterInitial() : super(0);
}

class CounterUpdated extends CounterState {
  const CounterUpdated(super.count);
}
```

### Step 3: Create Cubit

```dart
// counter_cubit.dart
import 'package:flutter_bloc/flutter_bloc.dart';
import 'counter_state.dart';

class CounterCubit extends Cubit<CounterState> {
  // Start with initial state
  CounterCubit() : super(const CounterInitial());

  // Methods to change state
  void increment() {
    emit(CounterUpdated(state.count + 1));
  }

  void decrement() {
    emit(CounterUpdated(state.count - 1));
  }

  void reset() {
    emit(const CounterInitial());
  }
}
```

### Step 4: Provide Cubit to Widget Tree

```dart
// main.dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'counter_cubit.dart';
import 'counter_screen.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Cubit Counter',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: BlocProvider(
        create: (context) => CounterCubit(),
        child: const CounterScreen(),
      ),
    );
  }
}
```

### Step 5: Use Cubit in UI

```dart
// counter_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'counter_cubit.dart';
import 'counter_state.dart';

class CounterScreen extends StatelessWidget {
  const CounterScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Cubit Counter'),
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
            // BlocBuilder rebuilds when state changes
            BlocBuilder<CounterCubit, CounterState>(
              builder: (context, state) {
                return Text(
                  '${state.count}',
                  style: const TextStyle(
                    fontSize: 48,
                    fontWeight: FontWeight.bold,
                  ),
                );
              },
            ),
            const SizedBox(height: 32),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                FloatingActionButton(
                  onPressed: () {
                    // Access cubit and call method
                    context.read<CounterCubit>().decrement();
                  },
                  heroTag: 'decrement',
                  child: const Icon(Icons.remove),
                ),
                const SizedBox(width: 16),
                FloatingActionButton(
                  onPressed: () {
                    context.read<CounterCubit>().reset();
                  },
                  heroTag: 'reset',
                  child: const Icon(Icons.refresh),
                ),
                const SizedBox(width: 16),
                FloatingActionButton(
                  onPressed: () {
                    context.read<CounterCubit>().increment();
                  },
                  heroTag: 'increment',
                  child: const Icon(Icons.add),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

### Comparison: setState() vs Cubit

**Week 2 with setState():**
```dart
class _CounterScreenState extends State<CounterScreen> {
  int _counter = 0;  // State mixed with UI

  void _incrementCounter() {
    setState(() {
      _counter++;  // Logic in UI layer
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Text('$_counter');  // Direct access
  }
}
```

**Week 4 with Cubit:**
```dart
// Separated state class
class CounterState {
  final int count;
  const CounterState(this.count);
}

// Separated business logic
class CounterCubit extends Cubit<CounterState> {
  void increment() => emit(CounterState(state.count + 1));
}

// Clean UI layer
BlocBuilder<CounterCubit, CounterState>(
  builder: (context, state) => Text('${state.count}'),
)
```

**Benefits:**
- ✅ Business logic separated from UI
- ✅ Easy to test CounterCubit independently
- ✅ State can be shared across multiple screens
- ✅ Clear state flow

---

## Core Cubit Concepts

### 1. BlocProvider

Provides a Cubit to the widget tree.

```dart
// Single provider
BlocProvider(
  create: (context) => CounterCubit(),
  child: MyScreen(),
)

// Multiple providers
MultiBlocProvider(
  providers: [
    BlocProvider(create: (context) => CounterCubit()),
    BlocProvider(create: (context) => ThemeCubit()),
    BlocProvider(create: (context) => CartCubit()),
  ],
  child: MyApp(),
)
```

### 2. BlocBuilder

Rebuilds UI when state changes.

```dart
BlocBuilder<CounterCubit, CounterState>(
  builder: (context, state) {
    // This rebuilds when state changes
    return Text('Count: ${state.count}');
  },
)
```

### 3. BlocListener

Executes code in response to state changes (side effects).

```dart
BlocListener<CartCubit, CartState>(
  listener: (context, state) {
    if (state is CartItemAdded) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Item added to cart!')),
      );
    } else if (state is CartError) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(state.message)),
      );
    }
  },
  child: MyWidget(),
)
```

### 4. BlocConsumer

Combines BlocBuilder and BlocListener.

```dart
BlocConsumer<CounterCubit, CounterState>(
  listener: (context, state) {
    // Side effects (navigation, snackbars)
    if (state.count == 10) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('You reached 10!')),
      );
    }
  },
  builder: (context, state) {
    // UI building
    return Text('Count: ${state.count}');
  },
)
```

### 5. context.read() vs context.watch()

```dart
// context.read() - Get cubit, doesn't listen to changes
ElevatedButton(
  onPressed: () {
    context.read<CounterCubit>().increment();  // Call method
  },
  child: const Text('Increment'),
)

// context.watch() - Get cubit AND rebuild when state changes
Widget build(BuildContext context) {
  final count = context.watch<CounterCubit>().state.count;
  return Text('$count');  // Rebuilds on state change
}
```

---

## Cubit Examples

### Example 1: Todo List with Cubit

```dart
// Models
class Todo {
  final String id;
  final String title;
  final bool isCompleted;

  Todo({
    required this.id,
    required this.title,
    this.isCompleted = false,
  });

  Todo copyWith({
    String? id,
    String? title,
    bool? isCompleted,
  }) {
    return Todo(
      id: id ?? this.id,
      title: title ?? this.title,
      isCompleted: isCompleted ?? this.isCompleted,
    );
  }
}

// State
class TodoState {
  final List<Todo> todos;
  final TodoFilter filter;

  const TodoState({
    this.todos = const [],
    this.filter = TodoFilter.all,
  });

  TodoState copyWith({
    List<Todo>? todos,
    TodoFilter? filter,
  }) {
    return TodoState(
      todos: todos ?? this.todos,
      filter: filter ?? this.filter,
    );
  }

  List<Todo> get filteredTodos {
    switch (filter) {
      case TodoFilter.completed:
        return todos.where((todo) => todo.isCompleted).toList();
      case TodoFilter.pending:
        return todos.where((todo) => !todo.isCompleted).toList();
      case TodoFilter.all:
      default:
        return todos;
    }
  }
}

enum TodoFilter { all, completed, pending }

// Cubit
import 'package:flutter_bloc/flutter_bloc.dart';

class TodoCubit extends Cubit<TodoState> {
  TodoCubit() : super(const TodoState());

  void addTodo(String title) {
    final todo = Todo(
      id: DateTime.now().toString(),
      title: title,
    );
    emit(state.copyWith(
      todos: [...state.todos, todo],
    ));
  }

  void toggleTodo(String id) {
    final updatedTodos = state.todos.map((todo) {
      if (todo.id == id) {
        return todo.copyWith(isCompleted: !todo.isCompleted);
      }
      return todo;
    }).toList();

    emit(state.copyWith(todos: updatedTodos));
  }

  void deleteTodo(String id) {
    final updatedTodos = state.todos.where((todo) => todo.id != id).toList();
    emit(state.copyWith(todos: updatedTodos));
  }

  void setFilter(TodoFilter filter) {
    emit(state.copyWith(filter: filter));
  }
}

// UI
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class TodoScreen extends StatelessWidget {
  const TodoScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todo List'),
        actions: [
          PopupMenuButton<TodoFilter>(
            onSelected: (filter) {
              context.read<TodoCubit>().setFilter(filter);
            },
            itemBuilder: (context) => [
              const PopupMenuItem(
                value: TodoFilter.all,
                child: Text('All'),
              ),
              const PopupMenuItem(
                value: TodoFilter.completed,
                child: Text('Completed'),
              ),
              const PopupMenuItem(
                value: TodoFilter.pending,
                child: Text('Pending'),
              ),
            ],
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: TodoInput(),
          ),
          Expanded(
            child: BlocBuilder<TodoCubit, TodoState>(
              builder: (context, state) {
                final todos = state.filteredTodos;

                if (todos.isEmpty) {
                  return const Center(
                    child: Text('No todos yet!'),
                  );
                }

                return ListView.builder(
                  itemCount: todos.length,
                  itemBuilder: (context, index) {
                    final todo = todos[index];
                    return ListTile(
                      leading: Checkbox(
                        value: todo.isCompleted,
                        onChanged: (_) {
                          context.read<TodoCubit>().toggleTodo(todo.id);
                        },
                      ),
                      title: Text(
                        todo.title,
                        style: TextStyle(
                          decoration: todo.isCompleted
                              ? TextDecoration.lineThrough
                              : null,
                        ),
                      ),
                      trailing: IconButton(
                        icon: const Icon(Icons.delete),
                        onPressed: () {
                          context.read<TodoCubit>().deleteTodo(todo.id);
                        },
                      ),
                    );
                  },
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}

class TodoInput extends StatefulWidget {
  const TodoInput({super.key});

  @override
  State<TodoInput> createState() => _TodoInputState();
}

class _TodoInputState extends State<TodoInput> {
  final _controller = TextEditingController();

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _addTodo() {
    if (_controller.text.isNotEmpty) {
      context.read<TodoCubit>().addTodo(_controller.text);
      _controller.clear();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Row(
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
    );
  }
}
```

### Example 2: Theme Cubit

```dart
// State
enum ThemeMode { light, dark }

class ThemeState {
  final ThemeMode mode;
  
  const ThemeState(this.mode);
  
  bool get isDark => mode == ThemeMode.dark;
}

// Cubit
import 'package:flutter_bloc/flutter_bloc.dart';

class ThemeCubit extends Cubit<ThemeState> {
  ThemeCubit() : super(const ThemeState(ThemeMode.light));

  void toggleTheme() {
    final newMode = state.isDark ? ThemeMode.light : ThemeMode.dark;
    emit(ThemeState(newMode));
  }

  void setLightTheme() {
    emit(const ThemeState(ThemeMode.light));
  }

  void setDarkTheme() {
    emit(const ThemeState(ThemeMode.dark));
  }
}

// Usage in MaterialApp
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => ThemeCubit(),
      child: BlocBuilder<ThemeCubit, ThemeState>(
        builder: (context, state) {
          return MaterialApp(
            title: 'Theme Demo',
            theme: ThemeData.light(),
            darkTheme: ThemeData.dark(),
            themeMode: state.isDark 
                ? MaterialThemeMode.dark 
                : MaterialThemeMode.light,
            home: const HomeScreen(),
          );
        },
      ),
    );
  }
}

// Settings screen with toggle
class SettingsScreen extends StatelessWidget {
  const SettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: BlocBuilder<ThemeCubit, ThemeState>(
        builder: (context, state) {
          return SwitchListTile(
            title: const Text('Dark Mode'),
            subtitle: const Text('Toggle dark theme'),
            value: state.isDark,
            onChanged: (_) {
              context.read<ThemeCubit>().toggleTheme();
            },
          );
        },
      ),
    );
  }
}
```

---

## Cubit with API Calls

Real-world example: Fetching data from an API using Cubit.

```dart
// Models
class Product {
  final int id;
  final String title;
  final double price;
  final String image;

  Product({
    required this.id,
    required this.title,
    required this.price,
    required this.image,
  });

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'],
      title: json['title'],
      price: (json['price'] as num).toDouble(),
      image: json['image'],
    );
  }
}

// States
abstract class ProductState {}

class ProductInitial extends ProductState {}

class ProductLoading extends ProductState {}

class ProductLoaded extends ProductState {
  final List<Product> products;
  ProductLoaded(this.products);
}

class ProductError extends ProductState {
  final String message;
  ProductError(this.message);
}

// API Service
import 'dart:convert';
import 'package:http/http.dart' as http;

class ApiService {
  static const String baseUrl = 'https://fakestoreapi.com';

  Future<List<Product>> fetchProducts() async {
    final response = await http.get(
      Uri.parse('$baseUrl/products'),
    ).timeout(const Duration(seconds: 10));

    if (response.statusCode == 200) {
      final List<dynamic> data = json.decode(response.body);
      return data.map((json) => Product.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load products');
    }
  }
}

// Cubit
import 'package:flutter_bloc/flutter_bloc.dart';

class ProductCubit extends Cubit<ProductState> {
  final ApiService apiService;

  ProductCubit({required this.apiService}) : super(ProductInitial());

  Future<void> loadProducts() async {
    emit(ProductLoading());
    try {
      final products = await apiService.fetchProducts();
      emit(ProductLoaded(products));
    } catch (e) {
      emit(ProductError(e.toString()));
    }
  }

  Future<void> refreshProducts() async {
    // Don't show loading for refresh
    try {
      final products = await apiService.fetchProducts();
      emit(ProductLoaded(products));
    } catch (e) {
      emit(ProductError(e.toString()));
    }
  }
}

// UI
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class ProductScreen extends StatelessWidget {
  const ProductScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Products'),
      ),
      body: BlocBuilder<ProductCubit, ProductState>(
        builder: (context, state) {
          if (state is ProductInitial) {
            // Load products on initial state
            context.read<ProductCubit>().loadProducts();
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (state is ProductLoading) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (state is ProductError) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(
                    'Error: ${state.message}',
                    style: const TextStyle(color: Colors.red),
                  ),
                  const SizedBox(height: 16),
                  ElevatedButton(
                    onPressed: () {
                      context.read<ProductCubit>().loadProducts();
                    },
                    child: const Text('Retry'),
                  ),
                ],
              ),
            );
          }

          if (state is ProductLoaded) {
            return RefreshIndicator(
              onRefresh: () async {
                context.read<ProductCubit>().refreshProducts();
                // Wait for refresh to complete
                await Future.delayed(const Duration(milliseconds: 500));
              },
              child: ListView.builder(
                itemCount: state.products.length,
                itemBuilder: (context, index) {
                  final product = state.products[index];
                  return ListTile(
                    leading: Image.network(
                      product.image,
                      width: 50,
                      height: 50,
                      fit: BoxFit.cover,
                    ),
                    title: Text(product.title),
                    subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
                  );
                },
              ),
            );
          }

          return const SizedBox.shrink();
        },
      ),
    );
  }
}

// Provide Cubit in main.dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Product App',
      home: BlocProvider(
        create: (context) => ProductCubit(
          apiService: ApiService(),
        ),
        child: const ProductScreen(),
      ),
    );
  }
}
```

**Key Differences from Week 2:**
- Business logic (API calls, state management) is in `ProductCubit`
- UI only handles display and user interactions
- Easy to test `ProductCubit` independently
- Can share `ProductCubit` across multiple screens

---

## Shopping Cart Example with Cubit

A practical e-commerce shopping cart implementation:

```dart
// Models
class CartItem {
  final int productId;
  final String title;
  final double price;
  final int quantity;

  CartItem({
    required this.productId,
    required this.title,
    required this.price,
    required this.quantity,
  });

  CartItem copyWith({
    int? productId,
    String? title,
    double? price,
    int? quantity,
  }) {
    return CartItem(
      productId: productId ?? this.productId,
      title: title ?? this.title,
      price: price ?? this.price,
      quantity: quantity ?? this.quantity,
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'productId': productId,
      'title': title,
      'price': price,
      'quantity': quantity,
    };
  }

  factory CartItem.fromJson(Map<String, dynamic> json) {
    return CartItem(
      productId: json['productId'],
      title: json['title'],
      price: json['price'],
      quantity: json['quantity'],
    );
  }
}

// State
class CartState {
  final List<CartItem> items;
  final double totalPrice;
  final int itemCount;

  CartState({
    required this.items,
    required this.totalPrice,
    required this.itemCount,
  });

  CartState.initial()
      : items = [],
        totalPrice = 0.0,
        itemCount = 0;

  CartState copyWith({
    List<CartItem>? items,
  }) {
    final updatedItems = items ?? this.items;
    final total = updatedItems.fold<double>(
      0,
      (sum, item) => sum + (item.price * item.quantity),
    );
    final count = updatedItems.fold<int>(
      0,
      (sum, item) => sum + item.quantity,
    );

    return CartState(
      items: updatedItems,
      totalPrice: total,
      itemCount: count,
    );
  }
}

// Cubit
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

class CartCubit extends Cubit<CartState> {
  CartCubit() : super(CartState.initial()) {
    _loadCart();
  }

  // Load cart from local storage
  Future<void> _loadCart() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final cartJson = prefs.getString('cart');
      if (cartJson != null) {
        final List<dynamic> itemsList = json.decode(cartJson);
        final items = itemsList.map((json) => CartItem.fromJson(json)).toList();
        emit(state.copyWith(items: items));
      }
    } catch (e) {
      // Handle error
      print('Error loading cart: $e');
    }
  }

  // Save cart to local storage
  Future<void> _saveCart() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final itemsList = state.items.map((item) => item.toJson()).toList();
      await prefs.setString('cart', json.encode(itemsList));
    } catch (e) {
      print('Error saving cart: $e');
    }
  }

  void addItem(int productId, String title, double price) {
    final existingIndex = state.items.indexWhere(
      (item) => item.productId == productId,
    );

    List<CartItem> updatedItems;
    if (existingIndex >= 0) {
      // Item exists, increase quantity
      updatedItems = [...state.items];
      updatedItems[existingIndex] = updatedItems[existingIndex].copyWith(
        quantity: updatedItems[existingIndex].quantity + 1,
      );
    } else {
      // New item, add to cart
      updatedItems = [
        ...state.items,
        CartItem(
          productId: productId,
          title: title,
          price: price,
          quantity: 1,
        ),
      ];
    }

    emit(state.copyWith(items: updatedItems));
    _saveCart();
  }

  void removeItem(int productId) {
    final updatedItems = state.items.where(
      (item) => item.productId != productId,
    ).toList();

    emit(state.copyWith(items: updatedItems));
    _saveCart();
  }

  void updateQuantity(int productId, int quantity) {
    if (quantity <= 0) {
      removeItem(productId);
      return;
    }

    final updatedItems = state.items.map((item) {
      if (item.productId == productId) {
        return item.copyWith(quantity: quantity);
      }
      return item;
    }).toList();

    emit(state.copyWith(items: updatedItems));
    _saveCart();
  }

  void clearCart() {
    emit(CartState.initial());
    _saveCart();
  }
}

// UI - Cart Screen
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class CartScreen extends StatelessWidget {
  const CartScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping Cart'),
      ),
      body: BlocBuilder<CartCubit, CartState>(
        builder: (context, state) {
          if (state.items.isEmpty) {
            return const Center(
              child: Text('Your cart is empty'),
            );
          }

          return Column(
            children: [
              Expanded(
                child: ListView.builder(
                  itemCount: state.items.length,
                  itemBuilder: (context, index) {
                    final item = state.items[index];
                    return ListTile(
                      title: Text(item.title),
                      subtitle: Text('\$${item.price.toStringAsFixed(2)}'),
                      trailing: Row(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          IconButton(
                            icon: const Icon(Icons.remove),
                            onPressed: () {
                              context.read<CartCubit>().updateQuantity(
                                item.productId,
                                item.quantity - 1,
                              );
                            },
                          ),
                          Text('${item.quantity}'),
                          IconButton(
                            icon: const Icon(Icons.add),
                            onPressed: () {
                              context.read<CartCubit>().updateQuantity(
                                item.productId,
                                item.quantity + 1,
                              );
                            },
                          ),
                          IconButton(
                            icon: const Icon(Icons.delete),
                            onPressed: () {
                              context.read<CartCubit>().removeItem(
                                item.productId,
                              );
                            },
                          ),
                        ],
                      ),
                    );
                  },
                ),
              ),
              Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.grey[200],
                  border: Border(
                    top: BorderSide(color: Colors.grey[400]!),
                  ),
                ),
                child: Column(
                  children: [
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Text(
                          'Items: ${state.itemCount}',
                          style: const TextStyle(fontSize: 16),
                        ),
                        Text(
                          'Total: \$${state.totalPrice.toStringAsFixed(2)}',
                          style: const TextStyle(
                            fontSize: 20,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 16),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: () {
                          // Proceed to checkout
                        },
                        child: const Text('Checkout'),
                      ),
                    ),
                  ],
                ),
              ),
            ],
          );
        },
      ),
    );
  }
}
```

---

## Best Practices

### 1. Keep State Immutable

Always create new state objects, never mutate existing ones.

```dart
// ❌ WRONG - Mutating state
void addTodo(Todo todo) {
  state.todos.add(todo);  // Mutating existing list
  emit(state);  // Won't trigger rebuild!
}

// ✅ CORRECT - Creating new state
void addTodo(Todo todo) {
  emit(state.copyWith(
    todos: [...state.todos, todo],  // New list
  ));
}
```

### 2. Use copyWith for State Updates

```dart
class TodoState {
  final List<Todo> todos;
  final bool isLoading;

  const TodoState({
    required this.todos,
    required this.isLoading,
  });

  // copyWith method
  TodoState copyWith({
    List<Todo>? todos,
    bool? isLoading,
  }) {
    return TodoState(
      todos: todos ?? this.todos,
      isLoading: isLoading ?? this.isLoading,
    );
  }
}
```

### 3. Handle All State Cases in UI

```dart
BlocBuilder<MyBloc, MyState>(
  builder: (context, state) {
    if (state is Loading) {
      return const CircularProgressIndicator();
    }
    if (state is Success) {
      return MySuccessWidget(data: state.data);
    }
    if (state is Error) {
      return ErrorWidget(message: state.message);
    }
    return const SizedBox.shrink();  // Handle unknown states
  },
)
```

### 4. Use BlocListener for Side Effects

```dart
BlocListener<CartCubit, CartState>(
  listener: (context, state) {
    // Show snackbar when item added
    if (state.items.isNotEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('${state.itemCount} items in cart'),
          duration: const Duration(seconds: 1),
        ),
      );
    }
  },
  child: MyWidget(),
)
```

### 5. Close Cubits Properly

Cubits are automatically closed when using BlocProvider, but if you create them manually:

```dart
class _MyScreenState extends State<MyScreen> {
  late final MyCubit _cubit;

  @override
  void initState() {
    super.initState();
    _cubit = MyCubit();
  }

  @override
  void dispose() {
    _cubit.close();  // Important!
    super.dispose();
  }
}
```

**Best Practice**: Always use BlocProvider to avoid manual cleanup.

### 6. Use Equatable for State Comparison

Add `equatable` package to compare states efficiently:

```yaml
dependencies:
  equatable: ^2.0.5
```

```dart
import 'package:equatable/equatable.dart';

class TodoState extends Equatable {
  final List<Todo> todos;
  final bool isLoading;

  const TodoState({
    required this.todos,
    required this.isLoading,
  });

  @override
  List<Object?> get props => [todos, isLoading];

  TodoState copyWith({
    List<Todo>? todos,
    bool? isLoading,
  }) {
    return TodoState(
      todos: todos ?? this.todos,
      isLoading: isLoading ?? this.isLoading,
    );
  }
}
```

### 7. Organize Cubit Files

```
lib/
├── cubits/
│   ├── counter/
│   │   ├── counter_cubit.dart
│   │   └── counter_state.dart
│   ├── todo/
│   │   ├── todo_cubit.dart
│   │   └── todo_state.dart
│   ├── cart/
│   │   ├── cart_cubit.dart
│   │   └── cart_state.dart
│   ├── product/
│   │   ├── product_cubit.dart
│   │   └── product_state.dart
│   └── theme/
│       ├── theme_cubit.dart
│       └── theme_state.dart
```

---

## Testing Cubits

One of the biggest advantages of Cubit is testability! You can test business logic independently from UI.

### Add Testing Dependencies

```yaml
dev_dependencies:
  flutter_test:
    sdk: flutter
  bloc_test: ^9.1.5
```

### Example: Testing CounterCubit

```dart
// counter_cubit_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:bloc_test/bloc_test.dart';

void main() {
  group('CounterCubit', () {
    test('initial state is 0', () {
      final cubit = CounterCubit();
      expect(cubit.state.count, 0);
    });

    blocTest<CounterCubit, CounterState>(
      'emits [1] when increment is called',
      build: () => CounterCubit(),
      act: (cubit) => cubit.increment(),
      expect: () => [const CounterUpdated(1)],
    );

    blocTest<CounterCubit, CounterState>(
      'emits [1, 2, 3] when increment is called 3 times',
      build: () => CounterCubit(),
      act: (cubit) {
        cubit.increment();
        cubit.increment();
        cubit.increment();
      },
      expect: () => [
        const CounterUpdated(1),
        const CounterUpdated(2),
        const CounterUpdated(3),
      ],
    );

    blocTest<CounterCubit, CounterState>(
      'emits [0] when reset is called',
      build: () => CounterCubit(),
      seed: () => const CounterUpdated(5),
      act: (cubit) => cubit.reset(),
      expect: () => [const CounterInitial()],
    );
  });
}
```

---

## Advanced: When to Consider Full Bloc

While Cubit is perfect for most use cases, you might consider full Bloc (with events) when:

- ✅ You need to track and log all user actions
- ✅ Multiple different events should trigger the same state change
- ✅ You need event transformation (debounce, throttle, etc.)
- ✅ You require more sophisticated debugging capabilities
- ✅ Your team prefers explicit event-driven architecture

**For this bootcamp's final project, Cubit is recommended** as it provides the right balance of simplicity and power.

---

## Key Takeaways

1. **Cubit** separates business logic from UI
2. **Direct method calls** make Cubit simpler than full Bloc
3. Use **BlocProvider** to provide Cubit to widget tree
4. Use **BlocBuilder** to rebuild UI on state changes
5. Use **BlocListener** for side effects (navigation, snackbars)
6. Use **context.read()** to call methods without listening
7. Use **context.watch()** to access state and listen to changes
8. Always emit **new state objects** (immutability)
9. Use **copyWith** for partial state updates
10. **Test your Cubits** independently from UI
11. Handle **all state cases** in your UI
12. **Cubits are automatically closed** with BlocProvider

---

## References and Further Reading

### Official Documentation
- [flutter_bloc Package](https://pub.dev/packages/flutter_bloc) - Official package documentation
- [Bloc Library](https://bloclibrary.dev/) - Comprehensive Bloc documentation
- [Bloc Concepts](https://bloclibrary.dev/#/coreconcepts) - Core concepts explained

### Tutorials and Guides
- [Bloc Tutorial](https://bloclibrary.dev/#/fluttercountertutorial) - Official counter tutorial
- [Bloc Architecture](https://bloclibrary.dev/#/architecture) - Recommended architecture
- [Flutter Bloc Examples](https://github.com/felangel/bloc/tree/master/examples) - Official example apps

### Testing
- [bloc_test Package](https://pub.dev/packages/bloc_test) - Testing utilities for Cubit/Bloc
- [Testing Blocs](https://bloclibrary.dev/#/testing) - How to test Cubits and Blocs

### Best Practices
- [Bloc Best Practices](https://bloclibrary.dev/#/blocnamingconventions) - Naming conventions and patterns
- [Cubit vs Bloc](https://bloclibrary.dev/#/coreconcepts?id=cubit-vs-bloc) - When to use each
- [State Management Options](https://docs.flutter.dev/development/data-and-backend/state-mgmt/options) - Flutter state management overview

---

## Week 4 Project Tips

For your E-Commerce Shopping App final project:

1. **Use Cubit for everything** - It's simpler and sufficient for the project requirements
2. **Create Cubits for**:
   -  Manage product list from API
   -  Manage shopping cart items
   -  Manage dark mode setting
3. **Don't overthink it** - Direct method calls are easier than events
4. **Test as you go** - Cubit makes testing easy
5. **Keep states simple** - Use `copyWith` for updates
6. **Use MultiBlocProvider** - Provide all cubits at app level

---

**Next Steps**: Apply these Cubit concepts in your Week 4 final project! Use Cubit for all state management - it's simple, powerful, and perfect for the e-commerce app. Good luck! 🚀
