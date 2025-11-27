# Forms and User Input in Flutter

Forms are essential for collecting user input in mobile applications. In this guide, you'll learn how to work with TextField widgets, manage input with controllers, and implement form validation.

## Understanding TextFields

The `TextField` widget is Flutter's basic input field for text. It allows users to enter text using the device keyboard.

### Basic TextField

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
      title: 'TextField Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const TextFieldExample(),
    );
  }
}

class TextFieldExample extends StatelessWidget {
  const TextFieldExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic TextField'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          decoration: InputDecoration(
            labelText: 'Enter your name',
            hintText: 'John Doe',
            border: OutlineInputBorder(),
          ),
        ),
      ),
    );
  }
}
```

### TextField Properties

#### InputDecoration

The `decoration` property customizes the appearance of the TextField:

```dart
TextField(
  decoration: InputDecoration(
    // Label that floats above the field when focused
    labelText: 'Email',
    
    // Placeholder text shown when field is empty
    hintText: 'example@email.com',
    
    // Text shown above the field
    helperText: 'Enter your email address',
    
    // Error message
    errorText: null, // or 'Invalid email'
    
    // Icon at the start of the field
    prefixIcon: Icon(Icons.email),
    
    // Icon at the end of the field
    suffixIcon: Icon(Icons.clear),
    
    // Border styles
    border: OutlineInputBorder(),
    enabledBorder: OutlineInputBorder(
      borderSide: BorderSide(color: Colors.grey),
    ),
    focusedBorder: OutlineInputBorder(
      borderSide: BorderSide(color: Colors.blue, width: 2.0),
    ),
    errorBorder: OutlineInputBorder(
      borderSide: BorderSide(color: Colors.red),
    ),
  ),
)
```

#### Keyboard Types

Control which keyboard appears using `keyboardType`:

```dart
// Email keyboard with @ symbol
TextField(
  keyboardType: TextInputType.emailAddress,
  decoration: InputDecoration(labelText: 'Email'),
)

// Numeric keyboard
TextField(
  keyboardType: TextInputType.number,
  decoration: InputDecoration(labelText: 'Phone Number'),
)

// Multiline text
TextField(
  keyboardType: TextInputType.multiline,
  maxLines: 5,
  decoration: InputDecoration(labelText: 'Message'),
)

// URL keyboard
TextField(
  keyboardType: TextInputType.url,
  decoration: InputDecoration(labelText: 'Website'),
)
```

#### Text Input Actions

Set the action button on the keyboard:

```dart
TextField(
  textInputAction: TextInputAction.done,    // Shows "Done"
  // textInputAction: TextInputAction.next,  // Shows "Next"
  // textInputAction: TextInputAction.send,  // Shows "Send"
  // textInputAction: TextInputAction.search, // Shows "Search"
)
```

#### Password Fields

Create password fields with obscured text:

```dart
TextField(
  obscureText: true,
  decoration: InputDecoration(
    labelText: 'Password',
    prefixIcon: Icon(Icons.lock),
  ),
)
```

### TextField Callbacks

```dart
TextField(
  // Called when text changes
  onChanged: (value) {
    print('Current value: $value');
  },
  
  // Called when user submits (presses done/enter)
  onSubmitted: (value) {
    print('Submitted: $value');
  },
  
  // Called when field gains/loses focus
  onTap: () {
    print('TextField tapped');
  },
)
```

### Complete TextField Example

```dart
class TextFieldPropertiesDemo extends StatefulWidget {
  const TextFieldPropertiesDemo({super.key});

  @override
  State<TextFieldPropertiesDemo> createState() => _TextFieldPropertiesDemoState();
}

class _TextFieldPropertiesDemoState extends State<TextFieldPropertiesDemo> {
  String currentText = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('TextField Properties'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              decoration: InputDecoration(
                labelText: 'Username',
                hintText: 'Enter username',
                prefixIcon: Icon(Icons.person),
                suffixIcon: Icon(Icons.check_circle, color: Colors.green),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10),
                ),
              ),
              onChanged: (value) {
                setState(() {
                  currentText = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text('You typed: $currentText'),
          ],
        ),
      ),
    );
  }
}
```

## Text Editing Controllers

A `TextEditingController` allows you to read and control the text in a TextField.

### Why Use Controllers?

1. **Read the current value** of a TextField
2. **Set initial values** in TextFields
3. **Clear text** programmatically
4. **Listen to changes** in the text
5. **Control cursor position** and selection

### Basic Controller Usage

```dart
class ControllerExample extends StatefulWidget {
  const ControllerExample({super.key});

  @override
  State<ControllerExample> createState() => _ControllerExampleState();
}

class _ControllerExampleState extends State<ControllerExample> {
  // Create a controller
  final TextEditingController _controller = TextEditingController();

  @override
  void dispose() {
    // Always dispose controllers to prevent memory leaks
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Controller Example'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _controller,
              decoration: InputDecoration(
                labelText: 'Enter text',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                // Read the value
                print('Current text: ${_controller.text}');
                
                // Show in dialog
                showDialog(
                  context: context,
                  builder: (context) => AlertDialog(
                    title: const Text('You entered'),
                    content: Text(_controller.text),
                    actions: [
                      TextButton(
                        onPressed: () => Navigator.pop(context),
                        child: const Text('OK'),
                      ),
                    ],
                  ),
                );
              },
              child: const Text('Show Text'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: () {
                // Clear the text
                _controller.clear();
              },
              child: const Text('Clear Text'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Setting Initial Values

```dart
class InitialValueExample extends StatefulWidget {
  const InitialValueExample({super.key});

  @override
  State<InitialValueExample> createState() => _InitialValueExampleState();
}

class _InitialValueExampleState extends State<InitialValueExample> {
  final TextEditingController _controller = TextEditingController(
    text: 'Initial value',  // Set initial text
  );

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Initial Value'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: _controller,
          decoration: InputDecoration(
            labelText: 'Name',
            border: OutlineInputBorder(),
          ),
        ),
      ),
    );
  }
}
```

### Listening to Changes

```dart
class ListenerExample extends StatefulWidget {
  const ListenerExample({super.key});

  @override
  State<ListenerExample> createState() => _ListenerExampleState();
}

class _ListenerExampleState extends State<ListenerExample> {
  final TextEditingController _controller = TextEditingController();
  String displayText = '';

  @override
  void initState() {
    super.initState();
    
    // Add a listener
    _controller.addListener(() {
      setState(() {
        displayText = _controller.text;
      });
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
        title: const Text('Listener Example'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _controller,
              decoration: InputDecoration(
                labelText: 'Type something',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            Text(
              'You typed: $displayText',
              style: const TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 8),
            Text(
              'Character count: ${displayText.length}',
              style: const TextStyle(fontSize: 16, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Multiple Controllers

```dart
class MultipleControllersExample extends StatefulWidget {
  const MultipleControllersExample({super.key});

  @override
  State<MultipleControllersExample> createState() => _MultipleControllersExampleState();
}

class _MultipleControllersExampleState extends State<MultipleControllersExample> {
  final TextEditingController _firstNameController = TextEditingController();
  final TextEditingController _lastNameController = TextEditingController();
  final TextEditingController _emailController = TextEditingController();

  @override
  void dispose() {
    // Dispose all controllers
    _firstNameController.dispose();
    _lastNameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  void _submitForm() {
    final firstName = _firstNameController.text;
    final lastName = _lastNameController.text;
    final email = _emailController.text;
    
    print('First Name: $firstName');
    print('Last Name: $lastName');
    print('Email: $email');
    
    // Clear all fields
    _firstNameController.clear();
    _lastNameController.clear();
    _emailController.clear();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Multiple Controllers'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _firstNameController,
              decoration: InputDecoration(
                labelText: 'First Name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _lastNameController,
              decoration: InputDecoration(
                labelText: 'Last Name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _emailController,
              keyboardType: TextInputType.emailAddress,
              decoration: InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 24),
            ElevatedButton(
              onPressed: _submitForm,
              child: const Text('Submit'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Form Validation

Form validation ensures that user input meets your requirements before processing.

### The Form Widget

Flutter provides a `Form` widget that groups and validates multiple form fields together.

### Basic Form Structure

```dart
class BasicFormExample extends StatefulWidget {
  const BasicFormExample({super.key});

  @override
  State<BasicFormExample> createState() => _BasicFormExampleState();
}

class _BasicFormExampleState extends State<BasicFormExample> {
  // Create a global key that uniquely identifies the Form widget
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form Validation'),
      ),
      body: Form(
        key: _formKey,  // Assign the key to the Form
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              TextFormField(
                decoration: InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your email';
                  }
                  return null;  // Return null if valid
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: () {
                  // Validate returns true if the form is valid
                  if (_formKey.currentState!.validate()) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Form is valid!')),
                    );
                  }
                },
                child: const Text('Validate'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

### TextFormField vs TextField

- **TextField**: Basic input widget, no built-in validation
- **TextFormField**: Extends TextField, includes `validator` property, must be inside a `Form` widget

### Validation Rules

#### Required Field

```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Name',
    border: OutlineInputBorder(),
  ),
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'This field is required';
    }
    return null;
  },
)
```

#### Email Validation

```dart
TextFormField(
  keyboardType: TextInputType.emailAddress,
  decoration: InputDecoration(
    labelText: 'Email',
    border: OutlineInputBorder(),
  ),
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'Please enter your email';
    }
    // Basic email pattern
    final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
    if (!emailRegex.hasMatch(value)) {
      return 'Please enter a valid email';
    }
    return null;
  },
)
```

#### Password Validation

```dart
TextFormField(
  obscureText: true,
  decoration: InputDecoration(
    labelText: 'Password',
    border: OutlineInputBorder(),
  ),
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'Please enter a password';
    }
    if (value.length < 6) {
      return 'Password must be at least 6 characters';
    }
    return null;
  },
)
```

#### Number Validation

```dart
TextFormField(
  keyboardType: TextInputType.number,
  decoration: InputDecoration(
    labelText: 'Age',
    border: OutlineInputBorder(),
  ),
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'Please enter your age';
    }
    final age = int.tryParse(value);
    if (age == null) {
      return 'Please enter a valid number';
    }
    if (age < 18) {
      return 'You must be at least 18 years old';
    }
    return null;
  },
)
```

#### Matching Fields (Confirm Password)

```dart
class PasswordMatchExample extends StatefulWidget {
  const PasswordMatchExample({super.key});

  @override
  State<PasswordMatchExample> createState() => _PasswordMatchExampleState();
}

class _PasswordMatchExampleState extends State<PasswordMatchExample> {
  final _formKey = GlobalKey<FormState>();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();

  @override
  void dispose() {
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Password Match'),
      ),
      body: Form(
        key: _formKey,
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              TextFormField(
                controller: _passwordController,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'Password',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter a password';
                  }
                  if (value.length < 6) {
                    return 'Password must be at least 6 characters';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _confirmPasswordController,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'Confirm Password',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please confirm your password';
                  }
                  if (value != _passwordController.text) {
                    return 'Passwords do not match';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Passwords match!')),
                    );
                  }
                },
                child: const Text('Validate'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

### Complete Registration Form Example

```dart
class RegistrationForm extends StatefulWidget {
  const RegistrationForm({super.key});

  @override
  State<RegistrationForm> createState() => _RegistrationFormState();
}

class _RegistrationFormState extends State<RegistrationForm> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();
  final _phoneController = TextEditingController();

  bool _obscurePassword = true;
  bool _obscureConfirmPassword = true;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    _phoneController.dispose();
    super.dispose();
  }

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // Form is valid, process data
      final name = _nameController.text;
      final email = _emailController.text;
      final password = _passwordController.text;
      final phone = _phoneController.text;

      print('Registration Data:');
      print('Name: $name');
      print('Email: $email');
      print('Phone: $phone');

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Registration Successful!'),
          backgroundColor: Colors.green,
        ),
      );

      // Clear form
      _formKey.currentState!.reset();
      _nameController.clear();
      _emailController.clear();
      _passwordController.clear();
      _confirmPasswordController.clear();
      _phoneController.clear();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registration Form'),
      ),
      body: SingleChildScrollView(
        child: Form(
          key: _formKey,
          child: Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                // Name Field
                TextFormField(
                  controller: _nameController,
                  decoration: InputDecoration(
                    labelText: 'Full Name',
                    prefixIcon: Icon(Icons.person),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your name';
                    }
                    if (value.length < 3) {
                      return 'Name must be at least 3 characters';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),

                // Email Field
                TextFormField(
                  controller: _emailController,
                  keyboardType: TextInputType.emailAddress,
                  decoration: InputDecoration(
                    labelText: 'Email',
                    prefixIcon: Icon(Icons.email),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your email';
                    }
                    final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
                    if (!emailRegex.hasMatch(value)) {
                      return 'Please enter a valid email';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),

                // Phone Field
                TextFormField(
                  controller: _phoneController,
                  keyboardType: TextInputType.phone,
                  decoration: InputDecoration(
                    labelText: 'Phone Number',
                    prefixIcon: Icon(Icons.phone),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your phone number';
                    }
                    if (value.length < 10) {
                      return 'Please enter a valid phone number';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),

                // Password Field
                TextFormField(
                  controller: _passwordController,
                  obscureText: _obscurePassword,
                  decoration: InputDecoration(
                    labelText: 'Password',
                    prefixIcon: Icon(Icons.lock),
                    suffixIcon: IconButton(
                      icon: Icon(
                        _obscurePassword ? Icons.visibility : Icons.visibility_off,
                      ),
                      onPressed: () {
                        setState(() {
                          _obscurePassword = !_obscurePassword;
                        });
                      },
                    ),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter a password';
                    }
                    if (value.length < 6) {
                      return 'Password must be at least 6 characters';
                    }
                    if (!value.contains(RegExp(r'[A-Z]'))) {
                      return 'Password must contain at least one uppercase letter';
                    }
                    if (!value.contains(RegExp(r'[0-9]'))) {
                      return 'Password must contain at least one number';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),

                // Confirm Password Field
                TextFormField(
                  controller: _confirmPasswordController,
                  obscureText: _obscureConfirmPassword,
                  decoration: InputDecoration(
                    labelText: 'Confirm Password',
                    prefixIcon: Icon(Icons.lock_outline),
                    suffixIcon: IconButton(
                      icon: Icon(
                        _obscureConfirmPassword ? Icons.visibility : Icons.visibility_off,
                      ),
                      onPressed: () {
                        setState(() {
                          _obscureConfirmPassword = !_obscureConfirmPassword;
                        });
                      },
                    ),
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please confirm your password';
                    }
                    if (value != _passwordController.text) {
                      return 'Passwords do not match';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 24),

                // Submit Button
                ElevatedButton(
                  onPressed: _submitForm,
                  style: ElevatedButton.styleFrom(
                    padding: const EdgeInsets.symmetric(vertical: 16),
                  ),
                  child: const Text(
                    'Register',
                    style: TextStyle(fontSize: 18),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

### Saving Form Data

Use `onSaved` callback to save form data:

```dart
class SaveFormExample extends StatefulWidget {
  const SaveFormExample({super.key});

  @override
  State<SaveFormExample> createState() => _SaveFormExampleState();
}

class _SaveFormExampleState extends State<SaveFormExample> {
  final _formKey = GlobalKey<FormState>();
  
  String? _name;
  String? _email;
  String? _phone;

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // Save all form fields
      _formKey.currentState!.save();
      
      print('Saved Data:');
      print('Name: $_name');
      print('Email: $_email');
      print('Phone: $_phone');
      
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Form Saved!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Save Form'),
      ),
      body: Form(
        key: _formKey,
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              TextFormField(
                decoration: InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your name';
                  }
                  return null;
                },
                onSaved: (value) {
                  _name = value;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                decoration: InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your email';
                  }
                  return null;
                },
                onSaved: (value) {
                  _email = value;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                decoration: InputDecoration(
                  labelText: 'Phone',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your phone';
                  }
                  return null;
                },
                onSaved: (value) {
                  _phone = value;
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: _submitForm,
                child: const Text('Save'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## Advanced Form Techniques

### Auto-validation

Enable real-time validation as the user types:

```dart
class AutoValidationExample extends StatefulWidget {
  const AutoValidationExample({super.key});

  @override
  State<AutoValidationExample> createState() => _AutoValidationExampleState();
}

class _AutoValidationExampleState extends State<AutoValidationExample> {
  final _formKey = GlobalKey<FormState>();
  AutovalidateMode _autoValidateMode = AutovalidateMode.disabled;

  void _submitForm() {
    setState(() {
      _autoValidateMode = AutovalidateMode.onUserInteraction;
    });
    
    if (_formKey.currentState!.validate()) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Form is valid!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Auto Validation'),
      ),
      body: Form(
        key: _formKey,
        autovalidateMode: _autoValidateMode,
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              TextFormField(
                decoration: InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your email';
                  }
                  final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
                  if (!emailRegex.hasMatch(value)) {
                    return 'Please enter a valid email';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: _submitForm,
                child: const Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

### Focus Management

Control which field is focused:

```dart
class FocusExample extends StatefulWidget {
  const FocusExample({super.key});

  @override
  State<FocusExample> createState() => _FocusExampleState();
}

class _FocusExampleState extends State<FocusExample> {
  final FocusNode _emailFocusNode = FocusNode();
  final FocusNode _passwordFocusNode = FocusNode();

  @override
  void dispose() {
    _emailFocusNode.dispose();
    _passwordFocusNode.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Focus Management'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              decoration: InputDecoration(
                labelText: 'Username',
                border: OutlineInputBorder(),
              ),
              textInputAction: TextInputAction.next,
              onSubmitted: (_) {
                // Move focus to email field
                FocusScope.of(context).requestFocus(_emailFocusNode);
              },
            ),
            const SizedBox(height: 16),
            TextField(
              focusNode: _emailFocusNode,
              decoration: InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
              textInputAction: TextInputAction.next,
              onSubmitted: (_) {
                // Move focus to password field
                FocusScope.of(context).requestFocus(_passwordFocusNode);
              },
            ),
            const SizedBox(height: 16),
            TextField(
              focusNode: _passwordFocusNode,
              obscureText: true,
              decoration: InputDecoration(
                labelText: 'Password',
                border: OutlineInputBorder(),
              ),
              textInputAction: TextInputAction.done,
              onSubmitted: (_) {
                // Submit form
                print('Form submitted');
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

### Input Formatters

Restrict and format user input:

```dart
import 'package:flutter/services.dart';

// Allow only numbers
TextField(
  keyboardType: TextInputType.number,
  inputFormatters: [
    FilteringTextInputFormatter.digitsOnly,
  ],
  decoration: InputDecoration(
    labelText: 'Numbers Only',
    border: OutlineInputBorder(),
  ),
)

// Limit length
TextField(
  inputFormatters: [
    LengthLimitingTextInputFormatter(10),
  ],
  decoration: InputDecoration(
    labelText: 'Max 10 Characters',
    border: OutlineInputBorder(),
  ),
)

// Allow only letters
TextField(
  inputFormatters: [
    FilteringTextInputFormatter.allow(RegExp(r'[a-zA-Z]')),
  ],
  decoration: InputDecoration(
    labelText: 'Letters Only',
    border: OutlineInputBorder(),
  ),
)

// Uppercase only
TextField(
  inputFormatters: [
    UpperCaseTextFormatter(),
  ],
  decoration: InputDecoration(
    labelText: 'Uppercase Only',
    border: OutlineInputBorder(),
  ),
)

// Custom formatter
class UpperCaseTextFormatter extends TextInputFormatter {
  @override
  TextEditingValue formatEditUpdate(
    TextEditingValue oldValue,
    TextEditingValue newValue,
  ) {
    return TextEditingValue(
      text: newValue.text.toUpperCase(),
      selection: newValue.selection,
    );
  }
}
```

## Best Practices

### 1. Always Dispose Controllers

```dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

### 2. Use Form Keys Properly

```dart
final _formKey = GlobalKey<FormState>();  // Create once

// Use in Form widget
Form(
  key: _formKey,
  child: ...
)
```

### 3. Provide Clear Error Messages

```dart
// Good
validator: (value) {
  if (value == null || value.isEmpty) {
    return 'Email is required';
  }
  if (!value.contains('@')) {
    return 'Please enter a valid email address';
  }
  return null;
}

// Avoid
validator: (value) {
  return value!.isEmpty ? 'Error' : null;  // Not helpful
}
```

### 4. Use Appropriate Keyboard Types

Match the keyboard to the input type:

```dart
// Email
keyboardType: TextInputType.emailAddress

// Phone
keyboardType: TextInputType.phone

// Numbers
keyboardType: TextInputType.number

// URL
keyboardType: TextInputType.url
```

### 5. Handle Form Submission Properly

```dart
void _submitForm() {
  if (_formKey.currentState!.validate()) {
    _formKey.currentState!.save();
    // Process data
  }
}
```

### 6. Use SingleChildScrollView for Long Forms

```dart
SingleChildScrollView(
  child: Form(
    child: Column(
      children: [
        // Many form fields
      ],
    ),
  ),
)
```

## Key Takeaways

1. **TextField** is the basic widget for text input
2. **TextEditingController** allows you to read, set, and control text
3. **TextFormField** is used inside **Form** widgets for validation
4. **validator** function returns error message or `null`
5. Always **dispose controllers** to prevent memory leaks
6. Use **GlobalKey<FormState>** to manage form state
7. Call **validate()** before processing form data
8. Use **onSaved** to extract form data efficiently
9. Provide **clear error messages** for better user experience
10. Use **appropriate keyboard types** and **input formatters**

---

## References and Further Reading

### TextField and Input
- [TextField Class](https://api.flutter.dev/flutter/material/TextField-class.html) - TextField API documentation
- [TextFormField Class](https://api.flutter.dev/flutter/material/TextFormField-class.html) - TextFormField API documentation
- [InputDecoration](https://api.flutter.dev/flutter/material/InputDecoration-class.html) - Input decoration options
- [TextInputType](https://api.flutter.dev/flutter/services/TextInputType-class.html) - Keyboard type options

### Controllers
- [TextEditingController](https://api.flutter.dev/flutter/widgets/TextEditingController-class.html) - Controller API documentation
- [Focus and Text Fields](https://docs.flutter.dev/cookbook/forms/focus) - Managing focus in text fields

### Form Validation
- [Form Class](https://api.flutter.dev/flutter/widgets/Form-class.html) - Form widget documentation
- [Build a Form with Validation](https://docs.flutter.dev/cookbook/forms/validation) - Official form validation guide
- [Retrieve Form Values](https://docs.flutter.dev/cookbook/forms/retrieve-input) - Getting form data
- [Create and Style a Text Field](https://docs.flutter.dev/cookbook/forms/text-input) - TextField styling guide

### Input Formatters
- [TextInputFormatter](https://api.flutter.dev/flutter/services/TextInputFormatter-class.html) - Input formatting
- [FilteringTextInputFormatter](https://api.flutter.dev/flutter/services/FilteringTextInputFormatter-class.html) - Filtering input
