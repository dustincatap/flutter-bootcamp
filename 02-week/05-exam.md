# Week 2 Final Project: Multi-Page Registration Form

Congratulations on completing Week 2! Now it's time to put everything you've learned into practice by building a multi-page registration form with navigation.

## Project Overview

You will create a Flutter application with multiple screens that collect user information through forms. This project will help you demonstrate your understanding of:
- Navigation between screens
- Form handling and validation
- State management with setState
- TextField controllers
- Passing data between screens

## Project Requirements

### Required Features

Your registration form app must include the following:

1. **Welcome/Home Screen**
   - Welcome message or app title
   - Brief description of what the app does
   - "Get Started" or "Register" button to navigate to the form
   - Display the submitted data when returning from the form (if any)

2. **Registration Form Screen (Page 1)**
   - Full Name field (required, minimum 3 characters)
   - Email field (required, valid email format)
   - Phone Number field (required, minimum 10 digits)
   - Password field (required, minimum 6 characters, hidden text)
   - Confirm Password field (must match password)
   - "Next" button to navigate to Page 2
   - Form validation for all fields

3. **Additional Information Screen (Page 2)**
   - Age field (required, must be a valid number, 18+)
   - Gender selection (Radio buttons or Dropdown)
   - Country/City field (required)
   - Bio or About Me field (multiline, optional)
   - "Submit" button to complete registration
   - "Back" button to return to Page 1
   - Form validation for required fields

4. **Data Display**
   - After successful submission, return to home screen
   - Display the submitted registration data on the home screen
   - Show all collected information in a formatted way

### Technical Requirements

1. **Project Setup**
   - Create a new Flutter project named `registration_form` or similar
   - Use proper project structure with separate files for each screen

2. **Navigation**
   - Use Navigator.push() and Navigator.pop()
   - Pass data from Page 1 to Page 2
   - Return complete data from Page 2 to Home Screen
   - Handle back navigation properly

3. **Form Handling**
   - Use TextEditingController for all text fields
   - Implement Form widget with GlobalKey<FormState>
   - Add proper validation for all required fields
   - Show appropriate error messages

4. **State Management**
   - Use StatefulWidget for screens with forms
   - Use setState() to update UI when data changes
   - Properly dispose of controllers

5. **Code Organization**
   - Create separate files for each screen:
     - `screens/home_screen.dart`
     - `screens/registration_page1.dart`
     - `screens/registration_page2.dart`
   - Create a model class for user data (optional but recommended)
   - Keep code clean and well-organized

### Widgets to Use

- Scaffold with AppBar
- Form and TextFormField
- TextEditingController
- Navigator for screen transitions
- Column/ListView for layout
- ElevatedButton or TextButton
- Radio buttons or DropdownButton
- Card widget for displaying data
- SingleChildScrollView for scrollable content

## Getting Started

### Step 1: Create the Project

```bash
flutter create registration_form
cd registration_form
```

### Step 2: Plan Your Screens

Before coding, plan your screens:
- **Home Screen**: Welcome message, navigation button, data display area
- **Page 1**: Personal info form (name, email, phone, password)
- **Page 2**: Additional info form (age, gender, country, bio)

### Step 3: Create the Project Structure

Organize your project files:

```
lib/
├── main.dart
└── screens/
    ├── home_screen.dart
    ├── registration_page1.dart
    └── registration_page2.dart
```

### Step 4: Build the Home Screen

Start with the home screen that has navigation to the registration form:

```dart
import 'package:flutter/material.dart';
import 'screens/home_screen.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Registration Form',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const HomeScreen(),
    );
  }
}
```

### Step 5: Create Registration Page 1

Build the first registration form with validation:

```dart
class RegistrationPage1 extends StatefulWidget {
  const RegistrationPage1({super.key});

  @override
  State<RegistrationPage1> createState() => _RegistrationPage1State();
}

class _RegistrationPage1State extends State<RegistrationPage1> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    super.dispose();
  }

  void _goToNextPage() {
    if (_formKey.currentState!.validate()) {
      // Navigate to Page 2 and pass data
      // TODO: Implement navigation
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registration - Step 1 of 2'),
      ),
      body: Form(
        key: _formKey,
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              // Add your form fields here
            ],
          ),
        ),
      ),
    );
  }
}
```

### Step 6: Add Form Validation

Implement validators for each field:

```dart
// Name validation
TextFormField(
  controller: _nameController,
  decoration: const InputDecoration(
    labelText: 'Full Name',
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
)

// Email validation
TextFormField(
  controller: _emailController,
  keyboardType: TextInputType.emailAddress,
  decoration: const InputDecoration(
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
)
```

### Step 7: Create Registration Page 2

Build the second page that receives data from Page 1:

```dart
class RegistrationPage2 extends StatefulWidget {
  // Receive data from Page 1
  final String name;
  final String email;
  final String phone;
  final String password;

  const RegistrationPage2({
    super.key,
    required this.name,
    required this.email,
    required this.phone,
    required this.password,
  });

  @override
  State<RegistrationPage2> createState() => _RegistrationPage2State();
}

class _RegistrationPage2State extends State<RegistrationPage2> {
  final _formKey = GlobalKey<FormState>();
  final _ageController = TextEditingController();
  final _countryController = TextEditingController();
  final _bioController = TextEditingController();
  
  String _selectedGender = 'Male';

  @override
  void dispose() {
    _ageController.dispose();
    _countryController.dispose();
    _bioController.dispose();
    super.dispose();
  }

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // Collect all data and return to home screen
      // TODO: Implement submission and navigation
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registration - Step 2 of 2'),
      ),
      body: Form(
        key: _formKey,
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              // Add your form fields here
            ],
          ),
        ),
      ),
    );
  }
}
```

### Step 8: Implement Navigation

Navigate from Page 1 to Page 2:

```dart
// In RegistrationPage1
void _goToNextPage() {
  if (_formKey.currentState!.validate()) {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => RegistrationPage2(
          name: _nameController.text,
          email: _emailController.text,
          phone: _phoneController.text,
          password: _passwordController.text,
        ),
      ),
    );
  }
}
```

Return data to home screen from Page 2:

```dart
// In RegistrationPage2
void _submitForm() {
  if (_formKey.currentState!.validate()) {
    final userData = {
      'name': widget.name,
      'email': widget.email,
      'phone': widget.phone,
      'age': _ageController.text,
      'gender': _selectedGender,
      'country': _countryController.text,
      'bio': _bioController.text,
    };
    
    // Pop twice to go back to home screen and pass data
    Navigator.pop(context); // Close Page 2
    Navigator.pop(context, userData); // Close Page 1 and return data
  }
}
```

### Step 9: Display Data on Home Screen

Update home screen to show submitted data:

```dart
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  Map<String, dynamic>? _userData;

  Future<void> _startRegistration() async {
    final result = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => const RegistrationPage1(),
      ),
    );

    if (result != null) {
      setState(() {
        _userData = result;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registration App'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Welcome section
            // Registration button
            // Display user data if available
          ],
        ),
      ),
    );
  }
}
```

## Tips and Best Practices

1. **Validation**
   - Validate each field appropriately
   - Show clear error messages
   - Test all validation rules

2. **Controllers**
   - Always dispose controllers in the dispose() method
   - Use one controller per TextField
   - Clear controllers after successful submission if needed

3. **Navigation**
   - Use async/await when waiting for navigation results
   - Handle null results when user presses back button
   - Pass only necessary data between screens

4. **User Experience**
   - Use appropriate keyboard types for each field
   - Hide password fields with obscureText: true
   - Show clear labels and hints
   - Use SingleChildScrollView to prevent overflow

5. **State Management**
   - Update state when receiving data from navigation
   - Use setState() to refresh UI
   - Keep state minimal and clean

6. **Code Organization**
   - Separate screens into different files
   - Use meaningful variable names
   - Add comments for complex logic
   - Follow Flutter naming conventions

## Bonus Challenges (Optional)

If you finish early and want to go further, try these:

1. **Enhanced Validation**
   - Add password strength indicator
   - Show real-time validation feedback
   - Add custom regex patterns for phone numbers

2. **Better UI**
   - Add icons to form fields
   - Use custom colors and themes
   - Add Card widgets for data display
   - Implement password visibility toggle

3. **Additional Features**
   - Add a progress indicator showing step 1 of 2, 2 of 2
   - Save data locally (use shared_preferences)
   - Add "Edit" functionality to modify submitted data
   - Add profile picture selection

4. **Advanced Navigation**
   - Create a third page for confirmation
   - Add a "Clear All" button to reset the form
   - Implement named routes
   - Add page transitions/animations

## Submission Guidelines

### Step 1: Prepare Your Code

1. **Test your app** thoroughly
   - Navigate through all screens
   - Test all validation rules
   - Verify data is passed correctly between screens
   - Test back button behavior
   - Ensure no errors occur

2. **Clean up your code**
   - Remove any debug print statements
   - Remove commented-out code
   - Ensure proper formatting
   - Add necessary comments

### Step 2: Create a README

Update your project's `README.md` file:

```markdown
# Multi-Page Registration Form

## Description
A Flutter application demonstrating navigation and form handling across multiple screens. Users can register by filling out a two-page form with validation.

## Features
- Multi-screen navigation
- Form validation on all fields
- Data passing between screens
- Password confirmation
- Age verification
- Gender selection
- Display of submitted registration data

## Screens
1. **Home Screen**: Welcome page with navigation to registration and data display
2. **Registration Page 1**: Personal information form (name, email, phone, password)
3. **Registration Page 2**: Additional information form (age, gender, country, bio)

## Validation Rules
- Name: Required, minimum 3 characters
- Email: Required, valid email format
- Phone: Required, minimum 10 digits
- Password: Required, minimum 6 characters
- Confirm Password: Must match password
- Age: Required, must be 18 or older
- Country: Required

## Screenshots
(Add screenshots of your app here)
- Home screen (empty state)
- Registration Page 1
- Registration Page 2
- Home screen (with submitted data)

## How to Run
1. Clone the repository
2. Run `flutter pub get`
3. Run `flutter run`

## What I Learned
(Optional: Share what you learned while building this project)

## Challenges Faced
(Optional: Describe any challenges you encountered and how you solved them)
```

### Step 3: Take Screenshots

Take screenshots showing:
1. Home screen (before registration)
2. Registration Page 1 with all fields filled
3. Registration Page 2 with all fields filled
4. Home screen showing submitted data
5. (Optional) Validation error messages

Save screenshots in a `screenshots/` folder in your project.

### Step 4: Create GitHub Repository

1. **Initialize Git** (if not already done)
   ```bash
   git init
   ```

2. **Verify `.gitignore` file**
   - Should exclude build files, .dart_tool, etc.

3. **Commit your code**
   ```bash
   git add .
   git commit -m "Initial commit: Multi-page registration form app"
   ```

4. **Create a new repository on GitHub**
   - Go to [GitHub](https://github.com)
   - Click "New repository"
   - Name it `flutter-registration-form` or similar
   - Do NOT initialize with README (you already have one)
   - Click "Create repository"

5. **Push your code to GitHub**
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   git branch -M main
   git push -u origin main
   ```

### Step 5: Submit Your Work

**Submit your GitHub repository URL** containing:
- ✅ Complete Flutter project code
- ✅ All three screens implemented
- ✅ Working navigation between screens
- ✅ Form validation on all required fields
- ✅ Data passing and display functionality
- ✅ Updated README.md with project description
- ✅ Screenshots of your running app
- ✅ Clean, well-organized code

**Submission Format:**
```
GitHub Repository URL: https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
```

### Repository Checklist

Before submitting, ensure your repository has:

- [ ] All project files organized in proper structure
- [ ] Separate screen files in screens/ folder
- [ ] Updated README.md with project information
- [ ] Screenshots folder with app screenshots
- [ ] .gitignore file (to exclude build files)
- [ ] All code properly committed
- [ ] Repository is set to public
- [ ] Code runs without errors when cloned and executed
- [ ] All navigation flows work correctly
- [ ] All form validations work properly

## Evaluation Criteria

Your project will be evaluated on:

1. **Functionality (40%)**
   - Navigation works correctly between all screens
   - Form validation works for all fields
   - Data is passed correctly between screens
   - Submitted data is displayed on home screen
   - App runs without errors

2. **Code Quality (30%)**
   - Clean and organized code structure
   - Proper use of controllers and disposal
   - Correct implementation of Form widgets
   - Use of StatefulWidget where appropriate
   - Following Flutter best practices
   - Separate files for each screen

3. **Form Validation (20%)**
   - All required fields are validated
   - Appropriate validation rules for each field
   - Clear error messages
   - Password confirmation works correctly
   - Email format validation
   - Age verification

4. **UI/UX Design (10%)**
   - Clear and intuitive navigation
   - Consistent styling across screens
   - Good use of spacing and layout
   - Appropriate keyboard types
   - User-friendly error messages

## Common Issues and Solutions

### Issue: Data Not Passing Between Screens
**Solution:** Make sure you're passing data correctly in Navigator.push() and receiving it in the constructor

```dart
// Sending screen
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => NextScreen(data: myData),
  ),
);

// Receiving screen
class NextScreen extends StatelessWidget {
  final String data;
  const NextScreen({super.key, required this.data});
}
```

### Issue: Controllers Not Disposed
**Solution:** Always dispose controllers in the dispose() method

```dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

### Issue: Validation Not Working
**Solution:** Ensure you're calling validate() on the form key

```dart
if (_formKey.currentState!.validate()) {
  // Form is valid
}
```

### Issue: Can't Return Data from Second Screen
**Solution:** Use Navigator.pop() with data and await the navigation

```dart
// Waiting for result
final result = await Navigator.push(...);

// Returning result
Navigator.pop(context, myData);
```

### Issue: Password Confirmation Not Matching
**Solution:** Access the password controller's text in the validator

```dart
validator: (value) {
  if (value != _passwordController.text) {
    return 'Passwords do not match';
  }
  return null;
}
```

## Need Help?

If you get stuck:

1. Review the week's materials, especially:
   - [Navigation](01-navigation.md)
   - [Forms and User Input](02-forms-and-user-input.md)
   - [Basic State Management](03-basic-state-management.md)

2. Check the official Flutter documentation:
   - [Navigation Cookbook](https://docs.flutter.dev/cookbook/navigation)
   - [Form Validation](https://docs.flutter.dev/cookbook/forms/validation)
   - [Passing Data](https://docs.flutter.dev/cookbook/navigation/passing-data)

3. Common debugging tips:
   - Use print() statements to track data flow
   - Check the debug console for error messages
   - Use hot reload to test changes quickly
   - Start with basic functionality, then add features

## Final Thoughts

This project brings together everything you learned in Week 2: navigation, forms, validation, and state management. Take your time to implement each feature correctly, and don't hesitate to experiment with different approaches. The goal is to understand how these concepts work together in a real application.

Remember:
- Start with the basic structure, then add features incrementally
- Test frequently as you build
- Keep your code organized and clean
- Focus on functionality first, then enhance the UI

Good luck! 🚀
