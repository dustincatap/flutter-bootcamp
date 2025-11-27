# Assets and Theming in Flutter

Assets and theming are essential for creating professional, polished Flutter applications. In this guide, you'll learn how to add images and custom fonts to your app, and how to implement light and dark themes.

## Adding Images

Images enhance the visual appeal of your app. Flutter supports various image formats including JPEG, PNG, GIF, WebP, and more.

### Image Sources

Flutter can load images from multiple sources:

1. **Asset Images**: Bundled with your app
2. **Network Images**: Loaded from the internet
3. **File Images**: From device storage
4. **Memory Images**: From memory (bytes)

### Using Asset Images

Asset images are bundled with your application and are the most common way to include images.

#### Step 1: Create an Assets Folder

Create a folder structure in your project root:

```
your_project/
├── lib/
├── assets/
│   └── images/
│       ├── logo.png
│       ├── profile.jpg
│       └── background.png
├── pubspec.yaml
└── ...
```

#### Step 2: Declare Assets in pubspec.yaml

Open `pubspec.yaml` and add your assets:

```yaml
flutter:
  assets:
    - assets/images/logo.png
    - assets/images/profile.jpg
    - assets/images/background.png
```

Or include an entire directory:

```yaml
flutter:
  assets:
    - assets/images/
```

**Important**: 
- Indentation matters in YAML! Use 2 spaces for each level.
- After modifying `pubspec.yaml`, run `flutter pub get` or restart your app.

#### Step 3: Display Images in Your App

```dart
import 'package:flutter/material.dart';

class ImageExample extends StatelessWidget {
  const ImageExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Image Example'),
      ),
      body: Center(
        child: Image.asset(
          'assets/images/logo.png',
          width: 200,
          height: 200,
        ),
      ),
    );
  }
}
```

### Image Widget Properties

```dart
Image.asset(
  'assets/images/logo.png',
  
  // Size
  width: 200,
  height: 200,
  
  // How image should fit in the box
  fit: BoxFit.cover,  // cover, contain, fill, fitWidth, fitHeight, none, scaleDown
  
  // Alignment
  alignment: Alignment.center,
  
  // Repeat pattern
  repeat: ImageRepeat.noRepeat,
  
  // Color and blend mode
  color: Colors.blue,
  colorBlendMode: BlendMode.colorBurn,
  
  // Opacity
  opacity: const AlwaysStoppedAnimation(0.8),
)
```

### BoxFit Options

```dart
// Cover: Scale to fill, may crop
Image.asset(
  'assets/images/photo.jpg',
  fit: BoxFit.cover,
)

// Contain: Scale to fit inside, may have empty space
Image.asset(
  'assets/images/photo.jpg',
  fit: BoxFit.contain,
)

// Fill: Stretch to fill (may distort)
Image.asset(
  'assets/images/photo.jpg',
  fit: BoxFit.fill,
)

// FitWidth: Scale to width
Image.asset(
  'assets/images/photo.jpg',
  fit: BoxFit.fitWidth,
)

// FitHeight: Scale to height
Image.asset(
  'assets/images/photo.jpg',
  fit: BoxFit.fitHeight,
)
```

### Network Images

Load images from the internet:

```dart
Image.network(
  'https://picsum.photos/200/300',
  width: 200,
  height: 300,
  fit: BoxFit.cover,
  loadingBuilder: (context, child, loadingProgress) {
    if (loadingProgress == null) {
      return child;
    }
    return Center(
      child: CircularProgressIndicator(
        value: loadingProgress.expectedTotalBytes != null
            ? loadingProgress.cumulativeBytesLoaded /
                loadingProgress.expectedTotalBytes!
            : null,
      ),
    );
  },
  errorBuilder: (context, error, stackTrace) {
    return const Icon(
      Icons.error,
      color: Colors.red,
      size: 50,
    );
  },
)
```

### Cached Network Images

For better performance with network images, use the `cached_network_image` package:

Add to `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  cached_network_image: ^3.3.0
```

Usage:

```dart
import 'package:cached_network_image/cached_network_image.dart';

CachedNetworkImage(
  imageUrl: 'https://picsum.photos/200/300',
  placeholder: (context, url) => const CircularProgressIndicator(),
  errorWidget: (context, url, error) => const Icon(Icons.error),
  width: 200,
  height: 300,
  fit: BoxFit.cover,
)
```

### Multiple Resolution Images

Provide different resolution images for different screen densities:

```
assets/
└── images/
    ├── logo.png          (1x)
    ├── 2.0x/
    │   └── logo.png      (2x)
    └── 3.0x/
        └── logo.png      (3x)
```

Flutter automatically selects the appropriate image based on device pixel ratio.

Declare in `pubspec.yaml`:

```yaml
flutter:
  assets:
    - assets/images/logo.png
```

### Circular Images

```dart
// Using ClipOval
ClipOval(
  child: Image.asset(
    'assets/images/profile.jpg',
    width: 100,
    height: 100,
    fit: BoxFit.cover,
  ),
)

// Using CircleAvatar
CircleAvatar(
  radius: 50,
  backgroundImage: AssetImage('assets/images/profile.jpg'),
)

// Network image in CircleAvatar
CircleAvatar(
  radius: 50,
  backgroundImage: NetworkImage('https://picsum.photos/200'),
)
```

### Complete Image Example

```dart
class ImageGallery extends StatelessWidget {
  const ImageGallery({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Image Gallery'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Asset Image',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Image.asset(
              'assets/images/logo.png',
              width: double.infinity,
              height: 200,
              fit: BoxFit.cover,
            ),
            const SizedBox(height: 24),
            const Text(
              'Network Image',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Image.network(
              'https://picsum.photos/400/200',
              width: double.infinity,
              height: 200,
              fit: BoxFit.cover,
            ),
            const SizedBox(height: 24),
            const Text(
              'Circular Images',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                CircleAvatar(
                  radius: 50,
                  backgroundImage: AssetImage('assets/images/profile.jpg'),
                ),
                ClipOval(
                  child: Image.network(
                    'https://picsum.photos/100',
                    width: 100,
                    height: 100,
                    fit: BoxFit.cover,
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

## Custom Fonts

Custom fonts help establish your app's visual identity and improve readability.

### Step 1: Download Fonts

Download fonts from sources like:
- [Google Fonts](https://fonts.google.com/)
- [Font Squirrel](https://www.fontsquirrel.com/)
- [DaFont](https://www.dafont.com/)

Common formats: `.ttf` (TrueType Font) or `.otf` (OpenType Font)

### Step 2: Add Fonts to Your Project

Create a fonts folder:

```
your_project/
├── lib/
├── assets/
│   └── fonts/
│       ├── Roboto-Regular.ttf
│       ├── Roboto-Bold.ttf
│       ├── Roboto-Italic.ttf
│       └── Pacifico-Regular.ttf
├── pubspec.yaml
└── ...
```

### Step 3: Declare Fonts in pubspec.yaml

```yaml
flutter:
  fonts:
    - family: Roboto
      fonts:
        - asset: assets/fonts/Roboto-Regular.ttf
        - asset: assets/fonts/Roboto-Bold.ttf
          weight: 700
        - asset: assets/fonts/Roboto-Italic.ttf
          style: italic
    
    - family: Pacifico
      fonts:
        - asset: assets/fonts/Pacifico-Regular.ttf
```

**Font Weights**:
- 100: Thin
- 200: Extra Light
- 300: Light
- 400: Regular/Normal
- 500: Medium
- 600: Semi Bold
- 700: Bold
- 800: Extra Bold
- 900: Black

### Step 4: Use Custom Fonts

#### Using in TextStyle

```dart
Text(
  'Hello, Custom Font!',
  style: TextStyle(
    fontFamily: 'Roboto',
    fontSize: 24,
    fontWeight: FontWeight.bold,  // Uses Roboto-Bold.ttf
  ),
)

Text(
  'Pacifico Font',
  style: TextStyle(
    fontFamily: 'Pacifico',
    fontSize: 32,
  ),
)
```

#### Set as Default Font in Theme

```dart
MaterialApp(
  theme: ThemeData(
    fontFamily: 'Roboto',  // Default font for entire app
  ),
  home: const MyHomePage(),
)
```

### Using Google Fonts Package

An easier way to use Google Fonts without downloading:

Add to `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_fonts: ^6.1.0
```

Usage:

```dart
import 'package:google_fonts/google_fonts.dart';

// Use in Text widget
Text(
  'Hello Google Fonts!',
  style: GoogleFonts.lato(
    fontSize: 24,
    fontWeight: FontWeight.bold,
  ),
)

Text(
  'Pacifico Style',
  style: GoogleFonts.pacifico(
    fontSize: 32,
    color: Colors.blue,
  ),
)

// Set as theme font
MaterialApp(
  theme: ThemeData(
    textTheme: GoogleFonts.latoTextTheme(),
  ),
  home: const MyHomePage(),
)
```

### Complete Font Example

```dart
class FontExample extends StatelessWidget {
  const FontExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Fonts'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Default Font',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 16),
            Text(
              'Roboto Regular',
              style: TextStyle(
                fontFamily: 'Roboto',
                fontSize: 24,
              ),
            ),
            const SizedBox(height: 16),
            Text(
              'Roboto Bold',
              style: TextStyle(
                fontFamily: 'Roboto',
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            Text(
              'Roboto Italic',
              style: TextStyle(
                fontFamily: 'Roboto',
                fontSize: 24,
                fontStyle: FontStyle.italic,
              ),
            ),
            const SizedBox(height: 16),
            Text(
              'Pacifico Font',
              style: TextStyle(
                fontFamily: 'Pacifico',
                fontSize: 32,
                color: Colors.purple,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Light and Dark Themes

Supporting light and dark themes improves user experience and accessibility.

### Basic Theme Setup

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
      title: 'Theme Demo',
      
      // Light theme
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.blue,
        scaffoldBackgroundColor: Colors.white,
      ),
      
      // Dark theme
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.blue,
        scaffoldBackgroundColor: Colors.grey[900],
      ),
      
      // Theme mode: system, light, or dark
      themeMode: ThemeMode.system,  // Follows system settings
      
      home: const MyHomePage(),
    );
  }
}
```

### ThemeMode Options

```dart
// Always light theme
themeMode: ThemeMode.light

// Always dark theme
themeMode: ThemeMode.dark

// Follow system settings
themeMode: ThemeMode.system
```

### Customizing Themes

#### Light Theme

```dart
ThemeData(
  brightness: Brightness.light,
  
  // Primary color
  primarySwatch: Colors.blue,
  primaryColor: Colors.blue,
  
  // Accent color
  colorScheme: ColorScheme.light(
    primary: Colors.blue,
    secondary: Colors.orange,
  ),
  
  // Scaffold
  scaffoldBackgroundColor: Colors.white,
  
  // AppBar
  appBarTheme: AppBarTheme(
    backgroundColor: Colors.blue,
    foregroundColor: Colors.white,
    elevation: 2,
  ),
  
  // Text
  textTheme: TextTheme(
    displayLarge: TextStyle(fontSize: 32, fontWeight: FontWeight.bold),
    bodyLarge: TextStyle(fontSize: 16, color: Colors.black87),
  ),
  
  // Cards
  cardTheme: CardTheme(
    color: Colors.white,
    elevation: 4,
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(12),
    ),
  ),
  
  // Buttons
  elevatedButtonTheme: ElevatedButtonThemeData(
    style: ElevatedButton.styleFrom(
      backgroundColor: Colors.blue,
      foregroundColor: Colors.white,
      padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(8),
      ),
    ),
  ),
  
  // Input decoration
  inputDecorationTheme: InputDecorationTheme(
    border: OutlineInputBorder(),
    focusedBorder: OutlineInputBorder(
      borderSide: BorderSide(color: Colors.blue, width: 2),
    ),
  ),
)
```

#### Dark Theme

```dart
ThemeData(
  brightness: Brightness.dark,
  
  // Primary color
  primarySwatch: Colors.blue,
  primaryColor: Colors.blue,
  
  // Accent color
  colorScheme: ColorScheme.dark(
    primary: Colors.blue,
    secondary: Colors.orange,
  ),
  
  // Scaffold
  scaffoldBackgroundColor: Colors.grey[900],
  
  // AppBar
  appBarTheme: AppBarTheme(
    backgroundColor: Colors.grey[850],
    foregroundColor: Colors.white,
    elevation: 0,
  ),
  
  // Text
  textTheme: TextTheme(
    displayLarge: TextStyle(fontSize: 32, fontWeight: FontWeight.bold, color: Colors.white),
    bodyLarge: TextStyle(fontSize: 16, color: Colors.white70),
  ),
  
  // Cards
  cardTheme: CardTheme(
    color: Colors.grey[850],
    elevation: 4,
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(12),
    ),
  ),
  
  // Buttons
  elevatedButtonTheme: ElevatedButtonThemeData(
    style: ElevatedButton.styleFrom(
      backgroundColor: Colors.blue,
      foregroundColor: Colors.white,
      padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(8),
      ),
    ),
  ),
  
  // Input decoration
  inputDecorationTheme: InputDecorationTheme(
    border: OutlineInputBorder(),
    focusedBorder: OutlineInputBorder(
      borderSide: BorderSide(color: Colors.blue, width: 2),
    ),
  ),
)
```

### Dynamic Theme Switching

Allow users to manually switch between light and dark themes:

```dart
class ThemeSwitcherApp extends StatefulWidget {
  const ThemeSwitcherApp({super.key});

  @override
  State<ThemeSwitcherApp> createState() => _ThemeSwitcherAppState();
}

class _ThemeSwitcherAppState extends State<ThemeSwitcherApp> {
  ThemeMode _themeMode = ThemeMode.system;

  void _toggleTheme(ThemeMode mode) {
    setState(() {
      _themeMode = mode;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theme Switcher',
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.blue,
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
        ),
      ),
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.blue,
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.grey[850],
          foregroundColor: Colors.white,
        ),
      ),
      themeMode: _themeMode,
      home: ThemeSettingsScreen(
        currentTheme: _themeMode,
        onThemeChanged: _toggleTheme,
      ),
    );
  }
}

class ThemeSettingsScreen extends StatelessWidget {
  final ThemeMode currentTheme;
  final Function(ThemeMode) onThemeChanged;

  const ThemeSettingsScreen({
    super.key,
    required this.currentTheme,
    required this.onThemeChanged,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Settings'),
      ),
      body: ListView(
        children: [
          RadioListTile<ThemeMode>(
            title: const Text('Light Mode'),
            subtitle: const Text('Always use light theme'),
            value: ThemeMode.light,
            groupValue: currentTheme,
            onChanged: (value) {
              if (value != null) {
                onThemeChanged(value);
              }
            },
          ),
          RadioListTile<ThemeMode>(
            title: const Text('Dark Mode'),
            subtitle: const Text('Always use dark theme'),
            value: ThemeMode.dark,
            groupValue: currentTheme,
            onChanged: (value) {
              if (value != null) {
                onThemeChanged(value);
              }
            },
          ),
          RadioListTile<ThemeMode>(
            title: const Text('System Default'),
            subtitle: const Text('Follow system theme settings'),
            value: ThemeMode.system,
            groupValue: currentTheme,
            onChanged: (value) {
              if (value != null) {
                onThemeChanged(value);
              }
            },
          ),
        ],
      ),
    );
  }
}
```

### Accessing Current Theme

Get the current theme in your widgets:

```dart
class ThemedWidget extends StatelessWidget {
  const ThemedWidget({super.key});

  @override
  Widget build(BuildContext context) {
    // Get current theme
    final theme = Theme.of(context);
    
    // Check if dark mode
    final isDarkMode = theme.brightness == Brightness.dark;
    
    return Container(
      color: theme.scaffoldBackgroundColor,
      child: Column(
        children: [
          Text(
            'Hello',
            style: theme.textTheme.displayLarge,
          ),
          Icon(
            Icons.brightness_6,
            color: theme.primaryColor,
          ),
          Text(
            isDarkMode ? 'Dark Mode Active' : 'Light Mode Active',
            style: theme.textTheme.bodyLarge,
          ),
        ],
      ),
    );
  }
}
```

### Theme Extensions

Create custom theme properties:

```dart
// Define custom colors
class CustomColors extends ThemeExtension<CustomColors> {
  final Color? success;
  final Color? warning;
  final Color? info;

  const CustomColors({
    required this.success,
    required this.warning,
    required this.info,
  });

  @override
  CustomColors copyWith({
    Color? success,
    Color? warning,
    Color? info,
  }) {
    return CustomColors(
      success: success ?? this.success,
      warning: warning ?? this.warning,
      info: info ?? this.info,
    );
  }

  @override
  CustomColors lerp(ThemeExtension<CustomColors>? other, double t) {
    if (other is! CustomColors) {
      return this;
    }
    return CustomColors(
      success: Color.lerp(success, other.success, t),
      warning: Color.lerp(warning, other.warning, t),
      info: Color.lerp(info, other.info, t),
    );
  }
}

// Use in theme
ThemeData(
  extensions: <ThemeExtension<dynamic>>[
    CustomColors(
      success: Colors.green,
      warning: Colors.orange,
      info: Colors.blue,
    ),
  ],
)

// Access in widget
final customColors = Theme.of(context).extension<CustomColors>();
Container(
  color: customColors?.success,
)
```

### Complete Theme Example

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const ThemeApp());
}

class ThemeApp extends StatefulWidget {
  const ThemeApp({super.key});

  @override
  State<ThemeApp> createState() => _ThemeAppState();
}

class _ThemeAppState extends State<ThemeApp> {
  ThemeMode _themeMode = ThemeMode.system;

  void _setThemeMode(ThemeMode mode) {
    setState(() {
      _themeMode = mode;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Complete Theme Example',
      debugShowCheckedModeBanner: false,
      
      // Light Theme
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.blue,
        scaffoldBackgroundColor: Colors.grey[50],
        
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
          elevation: 2,
          centerTitle: true,
        ),
        
        cardTheme: CardTheme(
          color: Colors.white,
          elevation: 2,
          margin: EdgeInsets.all(8),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
        ),
        
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.blue,
            foregroundColor: Colors.white,
            padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
      ),
      
      // Dark Theme
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.blue,
        scaffoldBackgroundColor: Colors.grey[900],
        
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.grey[850],
          foregroundColor: Colors.white,
          elevation: 0,
          centerTitle: true,
        ),
        
        cardTheme: CardTheme(
          color: Colors.grey[850],
          elevation: 2,
          margin: EdgeInsets.all(8),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
        ),
        
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.blue,
            foregroundColor: Colors.white,
            padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
      ),
      
      themeMode: _themeMode,
      
      home: HomeScreen(
        currentTheme: _themeMode,
        onThemeChanged: _setThemeMode,
      ),
    );
  }
}

class HomeScreen extends StatelessWidget {
  final ThemeMode currentTheme;
  final Function(ThemeMode) onThemeChanged;

  const HomeScreen({
    super.key,
    required this.currentTheme,
    required this.onThemeChanged,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final isDark = theme.brightness == Brightness.dark;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Demo'),
        actions: [
          IconButton(
            icon: Icon(isDark ? Icons.light_mode : Icons.dark_mode),
            onPressed: () {
              onThemeChanged(
                isDark ? ThemeMode.light : ThemeMode.dark,
              );
            },
          ),
          PopupMenuButton<ThemeMode>(
            icon: const Icon(Icons.more_vert),
            onSelected: onThemeChanged,
            itemBuilder: (context) => [
              const PopupMenuItem(
                value: ThemeMode.light,
                child: Text('Light Mode'),
              ),
              const PopupMenuItem(
                value: ThemeMode.dark,
                child: Text('Dark Mode'),
              ),
              const PopupMenuItem(
                value: ThemeMode.system,
                child: Text('System Default'),
              ),
            ],
          ),
        ],
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Current Theme',
                    style: theme.textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    isDark ? 'Dark Mode' : 'Light Mode',
                    style: theme.textTheme.bodyLarge,
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                children: [
                  Text(
                    'Sample Content',
                    style: theme.textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  Text(
                    'This is how text looks in the current theme. '
                    'The theme automatically adjusts colors and styles.',
                    style: theme.textTheme.bodyMedium,
                  ),
                  const SizedBox(height: 16),
                  ElevatedButton(
                    onPressed: () {},
                    child: const Text('Elevated Button'),
                  ),
                  const SizedBox(height: 8),
                  OutlinedButton(
                    onPressed: () {},
                    child: const Text('Outlined Button'),
                  ),
                  const SizedBox(height: 8),
                  TextButton(
                    onPressed: () {},
                    child: const Text('Text Button'),
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          Card(
            child: ListTile(
              leading: Icon(Icons.palette, color: theme.primaryColor),
              title: const Text('Primary Color'),
              subtitle: Text(
                theme.primaryColor.toString(),
                style: TextStyle(fontSize: 12),
              ),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.color_lens, color: theme.colorScheme.secondary),
              title: const Text('Secondary Color'),
              subtitle: Text(
                theme.colorScheme.secondary.toString(),
                style: TextStyle(fontSize: 12),
              ),
            ),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## Best Practices

### Images

1. **Use appropriate image formats**:
   - PNG for transparency
   - JPEG for photos
   - WebP for better compression

2. **Provide multiple resolutions**:
   - Include 1x, 2x, and 3x versions
   - Flutter automatically selects the right one

3. **Optimize image sizes**:
   - Compress images before adding to assets
   - Use appropriate dimensions

4. **Use cached_network_image for network images**:
   - Better performance
   - Automatic caching

5. **Handle loading and error states**:
   ```dart
   Image.network(
     url,
     loadingBuilder: ...,
     errorBuilder: ...,
   )
   ```

### Fonts

1. **Limit the number of custom fonts**:
   - Each font increases app size
   - Use 1-2 font families maximum

2. **Include only necessary font weights**:
   - Don't include all weights if you only use regular and bold

3. **Use Google Fonts package**:
   - No need to bundle fonts
   - Downloads only when needed
   - Can cache for offline use

4. **Set default font in theme**:
   - Consistency across app
   - Easy to change later

### Theming

1. **Define themes completely**:
   - Light and dark versions
   - Customize all important properties

2. **Use Theme.of(context)**:
   - Access theme colors and styles
   - Ensures consistency

3. **Test both themes**:
   - Verify readability in both modes
   - Check contrast ratios

4. **Consider accessibility**:
   - Sufficient color contrast
   - Support for system settings

5. **Use semantic colors**:
   ```dart
   // Good
   color: theme.primaryColor
   
   // Avoid
   color: Colors.blue
   ```

6. **Persist theme preference**:
   - Save user's choice (use shared_preferences)
   - Restore on app launch

## Key Takeaways

1. **Assets** must be declared in `pubspec.yaml`
2. Use `Image.asset()` for bundled images and `Image.network()` for web images
3. Provide **multiple resolution** images (1x, 2x, 3x)
4. **Custom fonts** enhance your app's visual identity
5. Use the `google_fonts` package for easy access to Google Fonts
6. Define both **light** and **dark** themes in MaterialApp
7. Use `ThemeMode.system` to follow device settings
8. Access theme with `Theme.of(context)`
9. Allow users to **manually switch themes** for better UX
10. Follow **best practices** for images, fonts, and themes

---

## References and Further Reading

### Images
- [Adding Assets and Images](https://docs.flutter.dev/development/ui/assets-and-images) - Official guide for assets
- [Image Class](https://api.flutter.dev/flutter/widgets/Image-class.html) - Image widget documentation
- [BoxFit Enum](https://api.flutter.dev/flutter/painting/BoxFit.html) - Image fit options
- [Cached Network Image](https://pub.dev/packages/cached_network_image) - Package for caching network images

### Fonts
- [Use Custom Fonts](https://docs.flutter.dev/cookbook/design/fonts) - Adding custom fonts
- [Google Fonts Package](https://pub.dev/packages/google_fonts) - Google Fonts for Flutter
- [TextStyle Class](https://api.flutter.dev/flutter/painting/TextStyle-class.html) - Text styling options

### Theming
- [Theming](https://docs.flutter.dev/cookbook/design/themes) - Official theming guide
- [ThemeData Class](https://api.flutter.dev/flutter/material/ThemeData-class.html) - Theme configuration
- [Material Design](https://m3.material.io/) - Material Design guidelines
- [Dark Theme](https://docs.flutter.dev/cookbook/design/themes#creating-a-theme) - Implementing dark mode
