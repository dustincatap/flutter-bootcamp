# Week 1 Final Project: Personal Profile Page

Congratulations on completing Week 1! Now it's time to put everything you've learned into practice by building your own personal profile page app.

## Project Overview

You will create a simple Flutter application that displays your personal profile. This project will help you demonstrate your understanding of:
- Flutter project structure
- Stateless and Stateful widgets
- Basic layout widgets
- User interface design

## Project Requirements

### Required Features

Your profile page must include the following elements:

1. **Profile Picture**
   - Display a circular avatar image
   - You can use a placeholder icon if you don't have a photo

2. **Personal Information**
   - Full name
   - Current role/title (e.g., "Flutter Developer", "Student", etc.)
   - Brief bio or description (2-3 sentences about yourself)

3. **Contact Information**
   - Email address
   - Phone number (optional)
   - Location (city/country)

4. **Skills or Interests**
   - Display at least 3-5 skills or interests
   - Use chips, badges, or a simple list

5. **Interactive Element**
   - At least one interactive button or element
   - Examples: "Contact Me" button, like counter, theme toggle, etc.

### Technical Requirements

1. **Project Setup**
   - Create a new Flutter project named `profile_page` or similar
   - Use proper project structure

2. **Code Organization**
   - Use at least one Stateless widget
   - Use at least one Stateful widget (for the interactive element)
   - Keep code clean and well-organized

3. **Styling**
   - Use appropriate colors and spacing
   - Ensure the UI is visually appealing
   - Make good use of padding and margins

4. **Widgets to Use**
   - Scaffold with AppBar
   - Column and/or Row for layout
   - Container for styling
   - Text widgets with different styles
   - Icon or Image widgets
   - At least one button widget
   - Any other widgets you find appropriate

## Getting Started

### Step 1: Create the Project

```bash
flutter create profile_page
cd profile_page
```

### Step 2: Plan Your Layout

Before coding, sketch out your profile page. Consider:
- Where will the profile picture be positioned?
- How will you organize the information sections?
- What colors will you use?
- What interactive element will you include?

### Step 3: Build the Basic Structure

Start with a basic Scaffold:

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
      title: 'My Profile',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const ProfilePage(),
    );
  }
}

class ProfilePage extends StatelessWidget {
  const ProfilePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('My Profile'),
      ),
      body: // Your profile content here
    );
  }
}
```

### Step 4: Add Your Content

Build out your profile sections one by one:

1. **Add the profile picture section**
   ```dart
   CircleAvatar(
     radius: 60,
     backgroundColor: Colors.blue,
     child: Icon(Icons.person, size: 60, color: Colors.white),
   )
   ```

2. **Add name and title**
   ```dart
   Text(
     'Your Name',
     style: TextStyle(
       fontSize: 28,
       fontWeight: FontWeight.bold,
     ),
   )
   ```

3. **Add bio section**
4. **Add contact information**
5. **Add skills/interests section**
6. **Add interactive element**

### Step 5: Make It Interactive

Add a stateful widget for your interactive element. Example:

```dart
class LikeButton extends StatefulWidget {
  const LikeButton({super.key});

  @override
  State<LikeButton> createState() => _LikeButtonState();
}

class _LikeButtonState extends State<LikeButton> {
  int _likes = 0;

  void _incrementLikes() {
    setState(() {
      _likes++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        IconButton(
          icon: Icon(Icons.favorite),
          color: Colors.red,
          iconSize: 30,
          onPressed: _incrementLikes,
        ),
        Text('$_likes likes'),
      ],
    );
  }
}
```

## Tips and Best Practices

1. **Use const constructors** wherever possible for better performance
2. **Add SizedBox** for spacing between widgets instead of margin
3. **Use SingleChildScrollView** to make content scrollable on smaller screens
4. **Test on different screen sizes** if possible
5. **Keep your code organized** with proper indentation
6. **Add comments** to explain your code
7. **Experiment** with different colors and styles to make it your own

## Bonus Challenges (Optional)

If you finish early and want to go further, try these:

1. **Add Images**
   - Use a real profile photo from assets
   - Add background images or icons

2. **Add More Interactivity**
   - Add a "View More" button to expand the bio
   - Create a counter for profile views
   - Add a theme toggle (light/dark mode)

3. **Enhance the UI**
   - Add animations
   - Use gradients for backgrounds
   - Add shadows and elevations to cards

4. **Add More Sections**
   - Education section
   - Work experience
   - Social media links
   - Projects or portfolio items

## Submission Guidelines

### Step 1: Prepare Your Code

1. **Test your app** thoroughly
   - Run it on an emulator or physical device
   - Make sure all interactive elements work
   - Check that the UI looks good on different screen sizes

2. **Clean up your code**
   - Remove any commented-out code
   - Ensure proper formatting
   - Add necessary comments to explain your code

### Step 2: Create a README

Update your project's `README.md` file with the following information:

```markdown
# Personal Profile Page

## Description
Brief description of your profile page app.

## Features
- Profile picture display
- Personal information section
- Contact details
- Skills/interests showcase
- Interactive element (describe what it does)

## Screenshots
(Add screenshots of your app here)

## How to Run
1. Clone the repository
2. Run `flutter pub get`
3. Run `flutter run`

## Challenges Faced
(Optional: Describe any challenges you encountered and how you solved them)

## What I Learned
(Optional: Share what you learned while building this project)
```

### Step 3: Take Screenshots

1. Run your app on an emulator or device
2. Take screenshots showing:
   - The complete profile page
   - The interactive element in action (before and after)
3. Save screenshots in a `screenshots/` folder in your project
4. Reference them in your README

### Step 4: Create GitHub Repository

1. **Initialize Git** (if not already done)
   ```bash
   git init
   ```

2. **Create a `.gitignore` file** (Flutter projects come with this by default)
   - Verify it includes common Flutter ignore patterns

3. **Commit your code**
   ```bash
   git add .
   git commit -m "Initial commit: Personal profile page app"
   ```

4. **Create a new repository on GitHub**
   - Go to [GitHub](https://github.com)
   - Click "New repository"
   - Name it `flutter-profile-page` or similar
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
- ✅ Updated README.md with project description
- ✅ Screenshots of your running app
- ✅ All required features implemented
- ✅ Clean, well-organized code

**Submission Format:**
```
GitHub Repository URL: https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
```

### Repository Checklist

Before submitting, ensure your repository has:

- [ ] All project files (lib/, pubspec.yaml, etc.)
- [ ] Updated README.md with project information
- [ ] Screenshots folder with app screenshots
- [ ] .gitignore file (to exclude build files)
- [ ] All code properly committed
- [ ] Repository is set to public (so it can be reviewed)
- [ ] Code runs without errors when cloned and executed

### What NOT to Include

Your `.gitignore` should exclude:
- `/build/` directory
- `/.dart_tool/` directory
- `/android/` build files
- `/ios/` build files
- `.flutter-plugins`
- `.flutter-plugins-dependencies`

These are automatically excluded if you used `flutter create` to generate your project.

## Evaluation Criteria

Your project will be evaluated on:

1. **Functionality (40%)**
   - All required features are implemented
   - Interactive elements work correctly
   - App runs without errors

2. **Code Quality (30%)**
   - Clean and organized code
   - Proper use of widgets
   - Following Flutter best practices
   - Use of both Stateless and Stateful widgets

3. **UI/UX Design (20%)**
   - Visually appealing layout
   - Good use of colors and spacing
   - Consistent styling
   - Responsive design

4. **Creativity (10%)**
   - Personal touches and customization
   - Going beyond basic requirements
   - Innovative interactive elements

## Common Issues and Solutions

### Issue: Widget Overflow
**Solution:** Wrap your Column in a SingleChildScrollView

```dart
SingleChildScrollView(
  child: Column(
    children: [
      // Your widgets
    ],
  ),
)
```

### Issue: Images Not Showing
**Solution:** Make sure you've declared assets in pubspec.yaml

```yaml
flutter:
  assets:
    - assets/images/
```

### Issue: Buttons Not Responding
**Solution:** Check that onPressed is not null and setState is called properly

```dart
ElevatedButton(
  onPressed: () {
    // Your action here
  },
  child: Text('Click Me'),
)
```

## Need Help?

If you get stuck:

1. Review the week's materials, especially:
   - [Flutter Fundamentals](04-flutter-fundamentals.md)
   - [Dart Language Basics](03-dart-language-basics.md)

2. Check the official Flutter documentation:
   - [Widget Catalog](https://docs.flutter.dev/development/ui/widgets)
   - [Layout Tutorial](https://docs.flutter.dev/development/ui/layout/tutorial)

3. Use Flutter's hot reload to experiment quickly

4. Start simple and add features incrementally

## Final Thoughts

This project is your opportunity to showcase what you've learned in Week 1. Don't worry about making it perfect – focus on applying the concepts and making it your own. Have fun and be creative!

Good luck! 🚀
