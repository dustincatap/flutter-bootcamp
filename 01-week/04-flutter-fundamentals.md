# Flutter Fundamentals

Now that you understand Dart and have your environment set up, it's time to dive into Flutter development. This guide covers the essential concepts you need to build your first Flutter applications.

## Project Structure

Understanding the Flutter project structure is crucial for organizing your code and resources effectively.

### Creating a New Flutter Project

To create a new Flutter project, run:

```bash
flutter create my_app
cd my_app
```

### Default Project Structure

When you create a new Flutter project, you'll see the following structure:

```
my_app/
├── android/              # Android-specific configuration
├── ios/                  # iOS-specific configuration
├── lib/                  # Main application code (Dart files)
│   └── main.dart         # Entry point of the application
├── test/                 # Unit and widget tests
├── web/                  # Web-specific configuration
├── windows/              # Windows-specific configuration
├── linux/                # Linux-specific configuration
├── macos/                # macOS-specific configuration
├── .gitignore            # Git ignore file
├── pubspec.yaml          # Project configuration and dependencies
└── README.md             # Project documentation
```

### Key Directories and Files

#### lib/ Directory
The `lib/` directory contains all your Dart code. This is where you'll spend most of your time.

#### pubspec.yaml
The `pubspec.yaml` file is the project configuration file where you:
- Define project metadata (name, description, version)
- Declare dependencies (packages from pub.dev)
- Specify assets (images, fonts, etc.)

**Example pubspec.yaml:**
```yaml
name: my_app
description: A new Flutter project
version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  http: ^1.1.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^2.0.0

flutter:
  uses-material-design: true
  
  # Assets
  assets:
    - assets/images/
    - assets/icons/
  
  # Fonts
  fonts:
    - family: Roboto
      fonts:
        - asset: fonts/Roboto-Regular.ttf
        - asset: fonts/Roboto-Bold.ttf
          weight: 700
```

#### main.dart
The `main.dart` file is the entry point of your Flutter application:

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
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

**Key components:**
- `main()`: The entry point function that calls `runApp()`
- `runApp()`: Takes the root widget and makes it the root of the widget tree
- `MaterialApp`: A convenience widget that wraps commonly required widgets for Material Design apps
- `home`: The default route of the app (the first screen)

### Adding Dependencies

To add a package to your project:

1. Find the package on [pub.dev](https://pub.dev)
2. Add it to `pubspec.yaml` under `dependencies`:
   ```yaml
   dependencies:
     http: ^1.1.0
   ```
3. Run `flutter pub get` to install the package
4. Import it in your Dart file:
   ```dart
   import 'package:http/http.dart' as http;
   ```

### Managing Assets

To use images, fonts, or other assets:

1. Create an `assets/` folder in your project root
2. Add your files to the folder
3. Declare them in `pubspec.yaml`:
   ```yaml
   flutter:
     assets:
       - assets/images/logo.png
       - assets/images/
   ```
4. Use them in your code:
   ```dart
   Image.asset('assets/images/logo.png')
   ```

## Stateless vs Stateful Widgets

In Flutter, everything is a widget. Widgets are divided into two main categories: Stateless and Stateful.

### Stateless Widgets

**Stateless widgets** are immutable. Once created, they cannot change their properties. They are perfect for UI elements that don't need to change dynamically.

#### Characteristics
- Immutable (properties cannot change)
- No internal state to manage
- Rebuild only when parent widget changes
- Lightweight and efficient
- Use `StatelessWidget` class

#### When to Use
- Displaying static content (text, icons, images)
- UI elements that depend only on configuration data
- Widgets that don't respond to user interaction
- Performance-critical widgets

#### Example: Simple Text Display

```dart
import 'package:flutter/material.dart';

class WelcomeText extends StatelessWidget {
  final String name;
  
  const WelcomeText({
    super.key,
    required this.name,
  });

  @override
  Widget build(BuildContext context) {
    return Text(
      'Welcome, $name!',
      style: const TextStyle(
        fontSize: 24,
        fontWeight: FontWeight.bold,
      ),
    );
  }
}

// Usage
WelcomeText(name: 'John')
```

#### Example: Profile Card

```dart
class ProfileCard extends StatelessWidget {
  final String name;
  final String email;
  final String avatarUrl;
  
  const ProfileCard({
    super.key,
    required this.name,
    required this.email,
    required this.avatarUrl,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Row(
          children: [
            CircleAvatar(
              radius: 30,
              backgroundImage: NetworkImage(avatarUrl),
            ),
            const SizedBox(width: 16),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  name,
                  style: const TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                Text(
                  email,
                  style: const TextStyle(
                    color: Colors.grey,
                  ),
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

### Stateful Widgets

**Stateful widgets** are mutable. They can maintain state that might change during the widget's lifetime and can rebuild themselves when their state changes.

#### Characteristics
- Have mutable state
- Can change appearance in response to events
- Use `StatefulWidget` class with a separate `State` class
- Call `setState()` to trigger rebuild
- More complex than stateless widgets

#### When to Use
- Widgets that change based on user interaction (buttons, forms)
- Animated widgets
- Widgets that fetch and display dynamic data
- Widgets that need to maintain state across rebuilds

#### Anatomy of a Stateful Widget

A stateful widget consists of two classes:

1. **StatefulWidget class**: Immutable, creates the State object
2. **State class**: Mutable, holds the state and build logic

```dart
// 1. StatefulWidget class
class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

// 2. State class
class _CounterState extends State<Counter> {
  // State variable
  int _count = 0;

  // Method to modify state
  void _incrementCounter() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Text(
          'Count: $_count',
          style: const TextStyle(fontSize: 24),
        ),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

#### setState() Method

The `setState()` method is crucial for stateful widgets:

```dart
void _incrementCounter() {
  setState(() {
    // Modify state variables here
    _count++;
  });
  // Flutter rebuilds the widget after setState completes
}
```

**Important rules:**
- Always modify state inside `setState()`
- `setState()` triggers a rebuild of the widget
- Only the widget calling `setState()` and its children rebuild
- Keep `setState()` calls minimal for performance

#### Lifecycle Methods

Stateful widgets have lifecycle methods:

```dart
class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
    // Called once when the widget is inserted into the tree
    // Initialize state, start timers, subscribe to streams
    print('initState called');
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    // Called when dependencies change (e.g., InheritedWidget)
    print('didChangeDependencies called');
  }

  @override
  Widget build(BuildContext context) {
    // Called every time the widget needs to render
    print('build called');
    return Container();
  }

  @override
  void didUpdateWidget(MyWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    // Called when the widget configuration changes
    print('didUpdateWidget called');
  }

  @override
  void dispose() {
    // Called when the widget is removed from the tree permanently
    // Clean up: cancel timers, unsubscribe from streams
    print('dispose called');
    super.dispose();
  }
}
```

#### Example: Todo List

```dart
class TodoList extends StatefulWidget {
  const TodoList({super.key});

  @override
  State<TodoList> createState() => _TodoListState();
}

class _TodoListState extends State<TodoList> {
  final List<String> _todos = [];
  final TextEditingController _controller = TextEditingController();

  void _addTodo() {
    if (_controller.text.isNotEmpty) {
      setState(() {
        _todos.add(_controller.text);
        _controller.clear();
      });
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
    return Column(
      children: [
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _controller,
                  decoration: const InputDecoration(
                    hintText: 'Enter a todo',
                  ),
                ),
              ),
              IconButton(
                icon: const Icon(Icons.add),
                onPressed: _addTodo,
              ),
            ],
          ),
        ),
        Expanded(
          child: ListView.builder(
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
    );
  }
}
```

### Comparison: Stateless vs Stateful

| Feature | Stateless Widget | Stateful Widget |
|---------|------------------|-----------------|
| **State** | No internal state | Has mutable state |
| **Rebuilds** | When parent changes | When `setState()` is called |
| **Complexity** | Simple | More complex |
| **Performance** | More efficient | Slightly less efficient |
| **Use Case** | Static content | Dynamic content |
| **Classes** | One class | Two classes |
| **Example** | Text, Icon, Image | Form, Animation, Counter |

### Best Practices

1. **Start with Stateless**: Use stateless widgets by default and only use stateful when needed
2. **Keep State Minimal**: Store only what you need in the state
3. **State Placement**: Place state as low as possible in the widget tree
4. **Dispose Resources**: Always dispose controllers, timers, and subscriptions in `dispose()`
5. **const Constructors**: Use `const` constructors when possible for better performance

## Basic Widgets

Flutter provides a rich set of pre-built widgets. Here are the most commonly used ones.

### Layout Widgets

#### Container

A convenience widget that combines common painting, positioning, and sizing widgets.

```dart
Container(
  width: 200,
  height: 100,
  padding: const EdgeInsets.all(16),
  margin: const EdgeInsets.all(8),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(8),
    boxShadow: [
      BoxShadow(
        color: Colors.grey.withOpacity(0.5),
        spreadRadius: 2,
        blurRadius: 5,
      ),
    ],
  ),
  child: const Text('Hello, Flutter!'),
)
```

**Properties:**
- `width`, `height`: Set dimensions
- `padding`: Inner spacing
- `margin`: Outer spacing
- `decoration`: Background color, borders, shadows
- `child`: Single child widget

#### Row

Arranges children horizontally.

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Icon(Icons.star),
    Text('Rating'),
    Text('4.5'),
  ],
)
```

**Properties:**
- `mainAxisAlignment`: Horizontal alignment (start, end, center, spaceBetween, spaceEvenly, spaceAround)
- `crossAxisAlignment`: Vertical alignment (start, end, center, stretch, baseline)
- `children`: List of widgets

#### Column

Arranges children vertically.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [
    Text('Title'),
    Text('Subtitle'),
    ElevatedButton(
      onPressed: () {},
      child: Text('Click Me'),
    ),
  ],
)
```

#### Stack

Stacks widgets on top of each other.

```dart
Stack(
  children: [
    Container(
      width: 200,
      height: 200,
      color: Colors.blue,
    ),
    Positioned(
      top: 10,
      right: 10,
      child: Icon(Icons.favorite, color: Colors.red),
    ),
  ],
)
```

#### Expanded & Flexible

Control how children flex within Row, Column, or Flex.

```dart
Row(
  children: [
    Expanded(
      flex: 2,
      child: Container(color: Colors.red, height: 50),
    ),
    Expanded(
      flex: 1,
      child: Container(color: Colors.blue, height: 50),
    ),
  ],
)
```

#### Padding

Adds padding around a widget.

```dart
Padding(
  padding: const EdgeInsets.all(16.0),
  child: Text('Padded Text'),
)

// Different padding for each side
Padding(
  padding: const EdgeInsets.only(
    left: 16,
    top: 8,
    right: 16,
    bottom: 8,
  ),
  child: Text('Custom Padding'),
)
```

#### Center

Centers its child within itself.

```dart
Center(
  child: Text('Centered Text'),
)
```

### Display Widgets

#### Text

Displays a string of text.

```dart
Text(
  'Hello, Flutter!',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
    fontStyle: FontStyle.italic,
  ),
  textAlign: TextAlign.center,
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
)
```

#### RichText

Displays text with multiple styles.

```dart
RichText(
  text: TextSpan(
    text: 'Hello ',
    style: DefaultTextStyle.of(context).style,
    children: [
      TextSpan(
        text: 'Flutter',
        style: TextStyle(
          fontWeight: FontWeight.bold,
          color: Colors.blue,
        ),
      ),
      TextSpan(text: '!'),
    ],
  ),
)
```

#### Image

Displays an image.

```dart
// From assets
Image.asset('assets/images/logo.png')

// From network
Image.network(
  'https://example.com/image.jpg',
  width: 200,
  height: 200,
  fit: BoxFit.cover,
  loadingBuilder: (context, child, loadingProgress) {
    if (loadingProgress == null) return child;
    return CircularProgressIndicator();
  },
)

// From file
Image.file(File('/path/to/image.jpg'))
```

#### Icon

Displays an icon from the Material Icons set.

```dart
Icon(
  Icons.favorite,
  color: Colors.red,
  size: 30,
)

// With IconButton
IconButton(
  icon: Icon(Icons.add),
  onPressed: () {
    print('Add button pressed');
  },
)
```

### Input Widgets

#### TextField

Allows users to enter text.

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Enter your name',
    hintText: 'John Doe',
    prefixIcon: Icon(Icons.person),
    border: OutlineInputBorder(),
  ),
  onChanged: (value) {
    print('Current value: $value');
  },
)

// With controller
final controller = TextEditingController();

TextField(
  controller: controller,
  decoration: InputDecoration(labelText: 'Email'),
)

// Get value
String email = controller.text;

// Don't forget to dispose
@override
void dispose() {
  controller.dispose();
  super.dispose();
}
```

#### Checkbox

A Material Design checkbox.

```dart
bool isChecked = false;

Checkbox(
  value: isChecked,
  onChanged: (bool? value) {
    setState(() {
      isChecked = value ?? false;
    });
  },
)

// With CheckboxListTile
CheckboxListTile(
  title: Text('Accept terms and conditions'),
  value: isChecked,
  onChanged: (bool? value) {
    setState(() {
      isChecked = value ?? false;
    });
  },
)
```

#### Radio

A Material Design radio button.

```dart
String selectedValue = 'option1';

Column(
  children: [
    RadioListTile<String>(
      title: Text('Option 1'),
      value: 'option1',
      groupValue: selectedValue,
      onChanged: (String? value) {
        setState(() {
          selectedValue = value!;
        });
      },
    ),
    RadioListTile<String>(
      title: Text('Option 2'),
      value: 'option2',
      groupValue: selectedValue,
      onChanged: (String? value) {
        setState(() {
          selectedValue = value!;
        });
      },
    ),
  ],
)
```

#### Switch

A Material Design switch.

```dart
bool isSwitched = false;

Switch(
  value: isSwitched,
  onChanged: (bool value) {
    setState(() {
      isSwitched = value;
    });
  },
)

// With SwitchListTile
SwitchListTile(
  title: Text('Enable notifications'),
  value: isSwitched,
  onChanged: (bool value) {
    setState(() {
      isSwitched = value;
    });
  },
)
```

#### Slider

A Material Design slider.

```dart
double sliderValue = 50;

Slider(
  value: sliderValue,
  min: 0,
  max: 100,
  divisions: 10,
  label: sliderValue.round().toString(),
  onChanged: (double value) {
    setState(() {
      sliderValue = value;
    });
  },
)
```

### Button Widgets

#### ElevatedButton

A Material Design elevated button.

```dart
ElevatedButton(
  onPressed: () {
    print('Button pressed');
  },
  child: Text('Click Me'),
)

// With custom style
ElevatedButton(
  onPressed: () {},
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,
    foregroundColor: Colors.white,
    padding: EdgeInsets.symmetric(horizontal: 32, vertical: 16),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(8),
    ),
  ),
  child: Text('Styled Button'),
)

// Disabled button
ElevatedButton(
  onPressed: null,  // null disables the button
  child: Text('Disabled'),
)
```

#### TextButton

A simple flat button.

```dart
TextButton(
  onPressed: () {
    print('Text button pressed');
  },
  child: Text('Cancel'),
)
```

#### OutlinedButton

A button with an outlined border.

```dart
OutlinedButton(
  onPressed: () {
    print('Outlined button pressed');
  },
  child: Text('Outlined'),
)
```

#### IconButton

A button with an icon.

```dart
IconButton(
  icon: Icon(Icons.favorite),
  color: Colors.red,
  onPressed: () {
    print('Favorite pressed');
  },
)
```

#### FloatingActionButton

A circular Material Design floating action button.

```dart
FloatingActionButton(
  onPressed: () {
    print('FAB pressed');
  },
  child: Icon(Icons.add),
)

// Extended FAB
FloatingActionButton.extended(
  onPressed: () {},
  icon: Icon(Icons.add),
  label: Text('Create'),
)
```

### Scrolling Widgets

#### ListView

A scrollable list of widgets.

```dart
// Simple ListView
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
    ListTile(title: Text('Item 3')),
  ],
)

// ListView.builder (efficient for large lists)
ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('Item $index'),
    );
  },
)

// ListView.separated (with separators)
ListView.separated(
  itemCount: 20,
  separatorBuilder: (context, index) => Divider(),
  itemBuilder: (context, index) {
    return ListTile(title: Text('Item $index'));
  },
)
```

#### GridView

A scrollable grid of widgets.

```dart
// GridView.count
GridView.count(
  crossAxisCount: 2,  // 2 columns
  children: List.generate(10, (index) {
    return Card(
      child: Center(child: Text('Item $index')),
    );
  }),
)

// GridView.builder
GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 3,
    crossAxisSpacing: 10,
    mainAxisSpacing: 10,
  ),
  itemCount: 30,
  itemBuilder: (context, index) {
    return Card(
      child: Center(child: Text('$index')),
    );
  },
)
```

#### SingleChildScrollView

Makes a single widget scrollable.

```dart
SingleChildScrollView(
  child: Column(
    children: [
      Container(height: 200, color: Colors.red),
      Container(height: 200, color: Colors.blue),
      Container(height: 200, color: Colors.green),
      Container(height: 200, color: Colors.yellow),
    ],
  ),
)
```

### Other Common Widgets

#### Card

A Material Design card.

```dart
Card(
  elevation: 4,
  margin: EdgeInsets.all(8),
  child: Padding(
    padding: EdgeInsets.all(16),
    child: Column(
      children: [
        Text('Card Title', style: TextStyle(fontSize: 20)),
        SizedBox(height: 8),
        Text('Card content goes here'),
      ],
    ),
  ),
)
```

#### Divider

A horizontal line.

```dart
Column(
  children: [
    Text('Item 1'),
    Divider(),
    Text('Item 2'),
  ],
)
```

#### CircularProgressIndicator

A circular Material Design progress indicator.

```dart
Center(
  child: CircularProgressIndicator(),
)

// With custom color and size
SizedBox(
  width: 50,
  height: 50,
  child: CircularProgressIndicator(
    valueColor: AlwaysStoppedAnimation<Color>(Colors.blue),
  ),
)
```

#### AlertDialog

A Material Design alert dialog.

```dart
void showAlertDialog(BuildContext context) {
  showDialog(
    context: context,
    builder: (BuildContext context) {
      return AlertDialog(
        title: Text('Alert'),
        content: Text('This is an alert message'),
        actions: [
          TextButton(
            child: Text('Cancel'),
            onPressed: () {
              Navigator.of(context).pop();
            },
          ),
          TextButton(
            child: Text('OK'),
            onPressed: () {
              Navigator.of(context).pop();
              // Perform action
            },
          ),
        ],
      );
    },
  );
}
```

#### AppBar

A Material Design app bar.

```dart
Scaffold(
  appBar: AppBar(
    title: Text('My App'),
    backgroundColor: Colors.blue,
    actions: [
      IconButton(
        icon: Icon(Icons.search),
        onPressed: () {},
      ),
      IconButton(
        icon: Icon(Icons.more_vert),
        onPressed: () {},
      ),
    ],
  ),
  body: Center(child: Text('Content')),
)
```

#### Scaffold

Implements the basic Material Design visual layout structure.

```dart
Scaffold(
  appBar: AppBar(title: Text('Home')),
  body: Center(child: Text('Body content')),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
  drawer: Drawer(
    child: ListView(
      children: [
        DrawerHeader(
          child: Text('Header'),
          decoration: BoxDecoration(color: Colors.blue),
        ),
        ListTile(
          title: Text('Item 1'),
          onTap: () {},
        ),
      ],
    ),
  ),
  bottomNavigationBar: BottomNavigationBar(
    items: [
      BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
      BottomNavigationBarItem(icon: Icon(Icons.search), label: 'Search'),
    ],
  ),
)
```

### Building a Complete Example

Here's a complete example combining multiple widgets:

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
      title: 'Flutter Fundamentals',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int _counter = 0;
  bool _showImage = true;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  void _toggleImage() {
    setState(() {
      _showImage = !_showImage;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: const Text('Flutter Fundamentals Demo'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            if (_showImage)
              const Icon(
                Icons.flutter_dash,
                size: 100,
                color: Colors.blue,
              ),
            const SizedBox(height: 20),
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _toggleImage,
              child: Text(_showImage ? 'Hide Icon' : 'Show Icon'),
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

---

## References and Further Reading

### Project Structure and Configuration
- [Flutter Project Structure](https://docs.flutter.dev/development/tools/sdk/overview) - Understanding Flutter project organization
- [pubspec.yaml Documentation](https://dart.dev/tools/pub/pubspec) - Complete pubspec.yaml reference
- [Adding Assets and Images](https://docs.flutter.dev/development/ui/assets-and-images) - How to add and use assets
- [Using Packages](https://docs.flutter.dev/development/packages-and-plugins/using-packages) - Working with pub.dev packages

### Widgets
- [Widget Catalog](https://docs.flutter.dev/development/ui/widgets) - Complete list of Flutter widgets
- [Introduction to Widgets](https://docs.flutter.dev/development/ui/widgets-intro) - Widget basics and concepts
- [Layouts in Flutter](https://docs.flutter.dev/development/ui/layout) - Building layouts with widgets
- [Layout Cheat Sheet](https://medium.com/flutter-community/flutter-layout-cheat-sheet-5363348d037e) - Quick reference for layouts

### Stateless and Stateful Widgets
- [StatefulWidget Documentation](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html) - Official StatefulWidget API
- [StatelessWidget Documentation](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html) - Official StatelessWidget API
- [State Management](https://docs.flutter.dev/development/data-and-backend/state-mgmt/intro) - Introduction to state management
- [Widget Lifecycle](https://docs.flutter.dev/development/ui/widgets-intro#state-management) - Understanding widget lifecycle

### Basic Widgets Deep Dive
- [Material Components Widgets](https://docs.flutter.dev/development/ui/widgets/material) - Material Design widgets
- [Layout Widgets](https://docs.flutter.dev/development/ui/widgets/layout) - Row, Column, Stack, etc.
- [Input Widgets](https://docs.flutter.dev/cookbook/forms) - Forms and user input
- [Scrolling Widgets](https://docs.flutter.dev/cookbook/lists) - ListView, GridView, etc.

### Interactive Learning
- [Write Your First Flutter App](https://docs.flutter.dev/get-started/codelab) - Official beginner codelab
- [Building Layouts](https://docs.flutter.dev/development/ui/layout/tutorial) - Step-by-step layout tutorial
- [Adding Interactivity](https://docs.flutter.dev/development/ui/interactive) - Making apps interactive
- [Flutter Gallery](https://gallery.flutter.dev/) - Live demos of Flutter widgets and features
