# Local Data Persistence in Flutter

Local data persistence allows your app to save and retrieve data on the device, even after the app is closed. In this guide, you'll learn how to store user preferences, secure sensitive data, and implement simple data caching.

## Understanding Data Persistence

Different types of data require different storage solutions:

- **SharedPreferences**: Simple key-value pairs (settings, preferences, small data)
- **SecureStorage**: Sensitive data that needs encryption (tokens, passwords)
- **Data Caching**: Temporary storage for API responses and offline access

## SharedPreferences

SharedPreferences is the simplest way to store small amounts of data persistently. It stores data as key-value pairs.

### Use Cases for SharedPreferences

- User preferences (theme, language, font size)
- App settings (notifications enabled, auto-save)
- Simple user data (username, email, last login date)
- App state (onboarding completed, first launch)
- Cache simple API responses

**Note**: Do NOT use SharedPreferences for sensitive data like passwords or tokens.

### Step 1: Add the Package

Add `shared_preferences` to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^2.2.2
```

Run:

```bash
flutter pub get
```

### Step 2: Import the Package

```dart
import 'package:shared_preferences/shared_preferences.dart';
```

### Basic Usage

#### Saving Data

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

class SaveDataExample extends StatefulWidget {
  const SaveDataExample({super.key});

  @override
  State<SaveDataExample> createState() => _SaveDataExampleState();
}

class _SaveDataExampleState extends State<SaveDataExample> {
  final TextEditingController _nameController = TextEditingController();

  Future<void> _saveData() async {
    // Get SharedPreferences instance
    final prefs = await SharedPreferences.getInstance();
    
    // Save different data types
    await prefs.setString('username', _nameController.text);
    await prefs.setInt('age', 25);
    await prefs.setBool('isDarkMode', true);
    await prefs.setDouble('fontSize', 16.5);
    await prefs.setStringList('favorites', ['item1', 'item2', 'item3']);

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Data saved!')),
      );
    }
  }

  @override
  void dispose() {
    _nameController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Save Data'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Enter your name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _saveData,
              child: const Text('Save Data'),
            ),
          ],
        ),
      ),
    );
  }
}
```

#### Reading Data

```dart
class ReadDataExample extends StatefulWidget {
  const ReadDataExample({super.key});

  @override
  State<ReadDataExample> createState() => _ReadDataExampleState();
}

class _ReadDataExampleState extends State<ReadDataExample> {
  String _username = 'Not set';
  int _age = 0;
  bool _isDarkMode = false;
  double _fontSize = 14.0;
  List<String> _favorites = [];

  @override
  void initState() {
    super.initState();
    _loadData();
  }

  Future<void> _loadData() async {
    final prefs = await SharedPreferences.getInstance();
    
    setState(() {
      // Read data with default values if key doesn't exist
      _username = prefs.getString('username') ?? 'Not set';
      _age = prefs.getInt('age') ?? 0;
      _isDarkMode = prefs.getBool('isDarkMode') ?? false;
      _fontSize = prefs.getDouble('fontSize') ?? 14.0;
      _favorites = prefs.getStringList('favorites') ?? [];
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Read Data'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Username: $_username', style: const TextStyle(fontSize: 18)),
            Text('Age: $_age', style: const TextStyle(fontSize: 18)),
            Text('Dark Mode: $_isDarkMode', style: const TextStyle(fontSize: 18)),
            Text('Font Size: $_fontSize', style: const TextStyle(fontSize: 18)),
            Text('Favorites: ${_favorites.join(', ')}', style: const TextStyle(fontSize: 18)),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _loadData,
              child: const Text('Reload Data'),
            ),
          ],
        ),
      ),
    );
  }
}
```

#### Deleting Data

```dart
Future<void> _deleteData() async {
  final prefs = await SharedPreferences.getInstance();
  
  // Remove specific key
  await prefs.remove('username');
  
  // Or remove all keys
  await prefs.clear();
  
  // Check if key exists
  bool hasUsername = prefs.containsKey('username');
  print('Has username: $hasUsername');
}
```

### Complete Example: User Preferences

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

class UserPreferences extends StatefulWidget {
  const UserPreferences({super.key});

  @override
  State<UserPreferences> createState() => _UserPreferencesState();
}

class _UserPreferencesState extends State<UserPreferences> {
  bool _notificationsEnabled = true;
  bool _darkModeEnabled = false;
  double _fontSize = 16.0;
  String _language = 'English';

  @override
  void initState() {
    super.initState();
    _loadPreferences();
  }

  Future<void> _loadPreferences() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _notificationsEnabled = prefs.getBool('notifications') ?? true;
      _darkModeEnabled = prefs.getBool('darkMode') ?? false;
      _fontSize = prefs.getDouble('fontSize') ?? 16.0;
      _language = prefs.getString('language') ?? 'English';
    });
  }

  Future<void> _savePreferences() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setBool('notifications', _notificationsEnabled);
    await prefs.setBool('darkMode', _darkModeEnabled);
    await prefs.setDouble('fontSize', _fontSize);
    await prefs.setString('language', _language);

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Preferences saved!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('User Preferences'),
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
              _savePreferences();
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
              _savePreferences();
            },
          ),
          ListTile(
            title: const Text('Font Size'),
            subtitle: Slider(
              value: _fontSize,
              min: 12.0,
              max: 24.0,
              divisions: 12,
              label: _fontSize.toInt().toString(),
              onChanged: (value) {
                setState(() {
                  _fontSize = value;
                });
              },
              onChangeEnd: (value) {
                _savePreferences();
              },
            ),
          ),
          ListTile(
            title: const Text('Language'),
            subtitle: Text(_language),
            trailing: DropdownButton<String>(
              value: _language,
              items: const [
                DropdownMenuItem(value: 'English', child: Text('English')),
                DropdownMenuItem(value: 'Spanish', child: Text('Spanish')),
                DropdownMenuItem(value: 'French', child: Text('French')),
              ],
              onChanged: (value) {
                if (value != null) {
                  setState(() {
                    _language = value;
                  });
                  _savePreferences();
                }
              },
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Text(
              'Sample Text',
              style: TextStyle(fontSize: _fontSize),
            ),
          ),
        ],
      ),
    );
  }
}
```

### Onboarding Example

```dart
class OnboardingCheck extends StatefulWidget {
  const OnboardingCheck({super.key});

  @override
  State<OnboardingCheck> createState() => _OnboardingCheckState();
}

class _OnboardingCheckState extends State<OnboardingCheck> {
  bool _isFirstLaunch = true;

  @override
  void initState() {
    super.initState();
    _checkFirstLaunch();
  }

  Future<void> _checkFirstLaunch() async {
    final prefs = await SharedPreferences.getInstance();
    final hasSeenOnboarding = prefs.getBool('hasSeenOnboarding') ?? false;
    
    setState(() {
      _isFirstLaunch = !hasSeenOnboarding;
    });
  }

  Future<void> _completeOnboarding() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setBool('hasSeenOnboarding', true);
    
    setState(() {
      _isFirstLaunch = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_isFirstLaunch) {
      return Scaffold(
        appBar: AppBar(
          title: const Text('Welcome!'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Icon(Icons.waving_hand, size: 100, color: Colors.blue),
              const SizedBox(height: 20),
              const Text(
                'Welcome to Our App!',
                style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 20),
              const Text('This is your first time using the app.'),
              const SizedBox(height: 40),
              ElevatedButton(
                onPressed: _completeOnboarding,
                child: const Text('Get Started'),
              ),
            ],
          ),
        ),
      );
    }

    return Scaffold(
      appBar: AppBar(
        title: const Text('Home'),
      ),
      body: const Center(
        child: Text('Welcome back!'),
      ),
    );
  }
}
```

## Secure Storage

For sensitive data like authentication tokens, API keys, or passwords, use `flutter_secure_storage` which provides encrypted storage.

### Step 1: Add the Package

Add to `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_secure_storage: ^9.0.0
```

### Step 2: Platform-Specific Setup

#### Android Configuration

Add to `android/app/build.gradle`:

```gradle
android {
    ...
    defaultConfig {
        ...
        minSdkVersion 18
    }
}
```

#### iOS Configuration

No additional configuration needed for iOS.

### Step 3: Using Secure Storage

```dart
import 'package:flutter/material.dart';
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

class SecureStorageExample extends StatefulWidget {
  const SecureStorageExample({super.key});

  @override
  State<SecureStorageExample> createState() => _SecureStorageExampleState();
}

class _SecureStorageExampleState extends State<SecureStorageExample> {
  // Create storage instance
  final storage = const FlutterSecureStorage();
  
  final TextEditingController _tokenController = TextEditingController();
  String _savedToken = 'No token saved';

  // Save data
  Future<void> _saveToken() async {
    await storage.write(
      key: 'auth_token',
      value: _tokenController.text,
    );
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Token saved securely!')),
      );
    }
  }

  // Read data
  Future<void> _readToken() async {
    final token = await storage.read(key: 'auth_token');
    setState(() {
      _savedToken = token ?? 'No token saved';
    });
  }

  // Delete data
  Future<void> _deleteToken() async {
    await storage.delete(key: 'auth_token');
    setState(() {
      _savedToken = 'No token saved';
    });
  }

  // Delete all data
  Future<void> _deleteAll() async {
    await storage.deleteAll();
    setState(() {
      _savedToken = 'No token saved';
    });
  }

  @override
  void initState() {
    super.initState();
    _readToken();
  }

  @override
  void dispose() {
    _tokenController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Secure Storage'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            TextField(
              controller: _tokenController,
              decoration: const InputDecoration(
                labelText: 'Auth Token',
                border: OutlineInputBorder(),
              ),
              obscureText: true,
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _saveToken,
              child: const Text('Save Token'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _readToken,
              child: const Text('Read Token'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _deleteToken,
              style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
              child: const Text('Delete Token'),
            ),
            const SizedBox(height: 24),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Saved Token:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text(_savedToken),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Authentication Example with Secure Storage

```dart
class AuthService {
  final storage = const FlutterSecureStorage();

  // Save authentication data
  Future<void> saveAuthData({
    required String token,
    required String refreshToken,
    required String userId,
  }) async {
    await storage.write(key: 'auth_token', value: token);
    await storage.write(key: 'refresh_token', value: refreshToken);
    await storage.write(key: 'user_id', value: userId);
  }

  // Get authentication token
  Future<String?> getAuthToken() async {
    return await storage.read(key: 'auth_token');
  }

  // Check if user is logged in
  Future<bool> isLoggedIn() async {
    final token = await storage.read(key: 'auth_token');
    return token != null && token.isNotEmpty;
  }

  // Logout (clear all auth data)
  Future<void> logout() async {
    await storage.delete(key: 'auth_token');
    await storage.delete(key: 'refresh_token');
    await storage.delete(key: 'user_id');
  }

  // Get all stored keys
  Future<Map<String, String>> getAllData() async {
    return await storage.readAll();
  }
}

// Usage in a widget
class AuthExample extends StatefulWidget {
  const AuthExample({super.key});

  @override
  State<AuthExample> createState() => _AuthExampleState();
}

class _AuthExampleState extends State<AuthExample> {
  final AuthService _authService = AuthService();
  bool _isLoggedIn = false;

  @override
  void initState() {
    super.initState();
    _checkLoginStatus();
  }

  Future<void> _checkLoginStatus() async {
    final isLoggedIn = await _authService.isLoggedIn();
    setState(() {
      _isLoggedIn = isLoggedIn;
    });
  }

  Future<void> _login() async {
    // Simulate login
    await _authService.saveAuthData(
      token: 'sample_auth_token_12345',
      refreshToken: 'sample_refresh_token_67890',
      userId: 'user_123',
    );
    
    setState(() {
      _isLoggedIn = true;
    });
  }

  Future<void> _logout() async {
    await _authService.logout();
    setState(() {
      _isLoggedIn = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Auth Example'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              _isLoggedIn ? 'Logged In' : 'Not Logged In',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _isLoggedIn ? _logout : _login,
              child: Text(_isLoggedIn ? 'Logout' : 'Login'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Simple Data Caching

Caching API responses improves performance and enables offline access.

### Cache with Expiration

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class CacheManager {
  static const String _cachePrefix = 'cache_';
  static const String _timestampPrefix = 'timestamp_';

  // Save data to cache with timestamp
  Future<void> cacheData(String key, String data) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('$_cachePrefix$key', data);
    await prefs.setInt('$_timestampPrefix$key', DateTime.now().millisecondsSinceEpoch);
  }

  // Get cached data if not expired
  Future<String?> getCachedData(String key, {Duration maxAge = const Duration(hours: 1)}) async {
    final prefs = await SharedPreferences.getInstance();
    
    final timestamp = prefs.getInt('$_timestampPrefix$key');
    if (timestamp == null) return null;

    final cacheTime = DateTime.fromMillisecondsSinceEpoch(timestamp);
    final now = DateTime.now();
    
    // Check if cache is expired
    if (now.difference(cacheTime) > maxAge) {
      // Cache expired, delete it
      await clearCache(key);
      return null;
    }

    return prefs.getString('$_cachePrefix$key');
  }

  // Clear specific cache
  Future<void> clearCache(String key) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove('$_cachePrefix$key');
    await prefs.remove('$_timestampPrefix$key');
  }

  // Clear all cache
  Future<void> clearAllCache() async {
    final prefs = await SharedPreferences.getInstance();
    final keys = prefs.getKeys();
    
    for (final key in keys) {
      if (key.startsWith(_cachePrefix) || key.startsWith(_timestampPrefix)) {
        await prefs.remove(key);
      }
    }
  }
}

// Usage example
class CachedApiExample extends StatefulWidget {
  const CachedApiExample({super.key});

  @override
  State<CachedApiExample> createState() => _CachedApiExampleState();
}

class _CachedApiExampleState extends State<CachedApiExample> {
  final CacheManager _cacheManager = CacheManager();
  List<dynamic> _posts = [];
  bool _isLoading = false;
  bool _isFromCache = false;

  @override
  void initState() {
    super.initState();
    _loadPosts();
  }

  Future<void> _loadPosts({bool forceRefresh = false}) async {
    setState(() {
      _isLoading = true;
      _isFromCache = false;
    });

    try {
      // Try to get from cache first
      if (!forceRefresh) {
        final cachedData = await _cacheManager.getCachedData(
          'posts',
          maxAge: const Duration(minutes: 5),
        );

        if (cachedData != null) {
          setState(() {
            _posts = jsonDecode(cachedData);
            _isLoading = false;
            _isFromCache = true;
          });
          return;
        }
      }

      // Fetch from API
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts?_limit=10'),
      );

      if (response.statusCode == 200) {
        // Cache the response
        await _cacheManager.cacheData('posts', response.body);
        
        setState(() {
          _posts = jsonDecode(response.body);
          _isLoading = false;
          _isFromCache = false;
        });
      }
    } catch (e) {
      setState(() {
        _isLoading = false;
      });
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error: $e')),
        );
      }
    }
  }

  Future<void> _clearCache() async {
    await _cacheManager.clearCache('posts');
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Cache cleared')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Cached API Data'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () => _loadPosts(forceRefresh: true),
          ),
          IconButton(
            icon: const Icon(Icons.delete),
            onPressed: _clearCache,
          ),
        ],
      ),
      body: Column(
        children: [
          if (_isFromCache)
            Container(
              color: Colors.blue[100],
              padding: const EdgeInsets.all(8),
              child: const Row(
                children: [
                  Icon(Icons.offline_bolt, size: 16),
                  SizedBox(width: 8),
                  Text('Loaded from cache'),
                ],
              ),
            ),
          if (_isLoading)
            const Expanded(
              child: Center(child: CircularProgressIndicator()),
            )
          else
            Expanded(
              child: ListView.builder(
                itemCount: _posts.length,
                itemBuilder: (context, index) {
                  final post = _posts[index];
                  return ListTile(
                    leading: CircleAvatar(
                      child: Text('${post['id']}'),
                    ),
                    title: Text(post['title']),
                    subtitle: Text(
                      post['body'],
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
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

### Complete Example: Shopping Cart with Persistence

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

class CartItem {
  final String id;
  final String name;
  final double price;
  int quantity;

  CartItem({
    required this.id,
    required this.name,
    required this.price,
    this.quantity = 1,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'price': price,
      'quantity': quantity,
    };
  }

  factory CartItem.fromJson(Map<String, dynamic> json) {
    return CartItem(
      id: json['id'],
      name: json['name'],
      price: json['price'],
      quantity: json['quantity'],
    );
  }
}

class ShoppingCart extends StatefulWidget {
  const ShoppingCart({super.key});

  @override
  State<ShoppingCart> createState() => _ShoppingCartState();
}

class _ShoppingCartState extends State<ShoppingCart> {
  List<CartItem> _cartItems = [];

  @override
  void initState() {
    super.initState();
    _loadCart();
  }

  Future<void> _loadCart() async {
    final prefs = await SharedPreferences.getInstance();
    final cartJson = prefs.getString('shopping_cart');
    
    if (cartJson != null) {
      final List<dynamic> decoded = jsonDecode(cartJson);
      setState(() {
        _cartItems = decoded.map((item) => CartItem.fromJson(item)).toList();
      });
    }
  }

  Future<void> _saveCart() async {
    final prefs = await SharedPreferences.getInstance();
    final cartJson = jsonEncode(_cartItems.map((item) => item.toJson()).toList());
    await prefs.setString('shopping_cart', cartJson);
  }

  void _addItem(CartItem item) {
    setState(() {
      final existingIndex = _cartItems.indexWhere((i) => i.id == item.id);
      if (existingIndex >= 0) {
        _cartItems[existingIndex].quantity++;
      } else {
        _cartItems.add(item);
      }
    });
    _saveCart();
  }

  void _removeItem(String id) {
    setState(() {
      _cartItems.removeWhere((item) => item.id == id);
    });
    _saveCart();
  }

  void _updateQuantity(String id, int quantity) {
    setState(() {
      final item = _cartItems.firstWhere((item) => item.id == id);
      item.quantity = quantity;
    });
    _saveCart();
  }

  void _clearCart() {
    setState(() {
      _cartItems.clear();
    });
    _saveCart();
  }

  double get _totalPrice {
    return _cartItems.fold(0, (sum, item) => sum + (item.price * item.quantity));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping Cart'),
        actions: [
          if (_cartItems.isNotEmpty)
            IconButton(
              icon: const Icon(Icons.delete_forever),
              onPressed: _clearCart,
            ),
        ],
      ),
      body: Column(
        children: [
          Expanded(
            child: _cartItems.isEmpty
                ? const Center(
                    child: Text('Your cart is empty'),
                  )
                : ListView.builder(
                    itemCount: _cartItems.length,
                    itemBuilder: (context, index) {
                      final item = _cartItems[index];
                      return Card(
                        margin: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                        child: ListTile(
                          title: Text(item.name),
                          subtitle: Text('\$${item.price.toStringAsFixed(2)}'),
                          trailing: Row(
                            mainAxisSize: MainAxisSize.min,
                            children: [
                              IconButton(
                                icon: const Icon(Icons.remove),
                                onPressed: () {
                                  if (item.quantity > 1) {
                                    _updateQuantity(item.id, item.quantity - 1);
                                  } else {
                                    _removeItem(item.id);
                                  }
                                },
                              ),
                              Text('${item.quantity}'),
                              IconButton(
                                icon: const Icon(Icons.add),
                                onPressed: () {
                                  _updateQuantity(item.id, item.quantity + 1);
                                },
                              ),
                              IconButton(
                                icon: const Icon(Icons.delete),
                                onPressed: () => _removeItem(item.id),
                              ),
                            ],
                          ),
                        ),
                      );
                    },
                  ),
          ),
          Container(
            padding: const EdgeInsets.all(16),
            decoration: BoxDecoration(
              color: Colors.grey[200],
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.1),
                  blurRadius: 4,
                  offset: const Offset(0, -2),
                ),
              ],
            ),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                const Text(
                  'Total:',
                  style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                ),
                Text(
                  '\$${_totalPrice.toStringAsFixed(2)}',
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
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Simulate adding a product
          _addItem(
            CartItem(
              id: DateTime.now().toString(),
              name: 'Product ${_cartItems.length + 1}',
              price: (10 + _cartItems.length * 5).toDouble(),
            ),
          );
        },
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## Best Practices

### 1. Don't Store Sensitive Data in SharedPreferences

```dart
// ❌ WRONG - Password in SharedPreferences
await prefs.setString('password', 'myPassword123');

// ✅ CORRECT - Password in SecureStorage
await storage.write(key: 'password', value: 'myPassword123');
```

### 2. Provide Default Values

```dart
// ✅ GOOD - Always provide defaults
final username = prefs.getString('username') ?? 'Guest';
final isDarkMode = prefs.getBool('darkMode') ?? false;
```

### 3. Use Consistent Key Names

```dart
// ✅ GOOD - Use constants for keys
class StorageKeys {
  static const String username = 'user_username';
  static const String authToken = 'auth_token';
  static const String isDarkMode = 'settings_dark_mode';
}

final username = prefs.getString(StorageKeys.username);
```

### 4. Handle Async Operations Properly

```dart
// ✅ GOOD - Use async/await
Future<void> saveData() async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString('key', 'value');
}
```

### 5. Implement Cache Invalidation

```dart
// ✅ GOOD - Add timestamp to cache
class CacheEntry {
  final String data;
  final DateTime timestamp;
  
  bool isExpired(Duration maxAge) {
    return DateTime.now().difference(timestamp) > maxAge;
  }
}
```

### 6. Clear Sensitive Data on Logout

```dart
Future<void> logout() async {
  final storage = FlutterSecureStorage();
  await storage.deleteAll();
  
  final prefs = await SharedPreferences.getInstance();
  await prefs.remove('user_data');
}
```

### 7. Use Try-Catch for Error Handling

```dart
try {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString('key', 'value');
} catch (e) {
  print('Error saving data: $e');
}
```

## Common Issues and Solutions

### Issue: Data Not Persisting

**Solution**: Make sure you're using `await` when saving

```dart
// ❌ WRONG
prefs.setString('key', 'value'); // Not awaited

// ✅ CORRECT
await prefs.setString('key', 'value');
```

### Issue: Secure Storage Error on Android

**Solution**: Ensure minimum SDK version is 18 or higher

```gradle
minSdkVersion 18
```

### Issue: Cache Growing Too Large

**Solution**: Implement cache size limits and cleanup

```dart
Future<void> cleanOldCache() async {
  final prefs = await SharedPreferences.getInstance();
  final keys = prefs.getKeys();
  
  for (final key in keys) {
    if (key.startsWith('cache_')) {
      final timestamp = prefs.getInt('timestamp_$key');
      if (timestamp != null) {
        final age = DateTime.now().millisecondsSinceEpoch - timestamp;
        if (age > Duration(days: 7).inMilliseconds) {
          await prefs.remove(key);
        }
      }
    }
  }
}
```

### Issue: JSON Encoding Error

**Solution**: Ensure your model has proper toJson/fromJson methods

```dart
class User {
  final String name;
  
  Map<String, dynamic> toJson() => {'name': name};
  factory User.fromJson(Map<String, dynamic> json) => User(name: json['name']);
}
```

## Key Takeaways

1. **SharedPreferences** for simple key-value storage (settings, preferences)
2. **FlutterSecureStorage** for sensitive data (tokens, passwords)
3. Never store **passwords in SharedPreferences**
4. Always provide **default values** when reading data
5. Use **const keys** to avoid typos
6. Implement **cache expiration** for API responses
7. **Clear sensitive data** on logout
8. Use **try-catch** for error handling
9. **Await** all async storage operations
10. Keep cache size **manageable** with cleanup strategies

---

## References and Further Reading

### SharedPreferences
- [shared_preferences Package](https://pub.dev/packages/shared_preferences) - Official package documentation
- [Store Key-Value Data](https://docs.flutter.dev/cookbook/persistence/key-value) - Official Flutter guide
- [SharedPreferences API](https://pub.dev/documentation/shared_preferences/latest/) - API documentation

### Secure Storage
- [flutter_secure_storage Package](https://pub.dev/packages/flutter_secure_storage) - Official package documentation
- [Secure Storage Guide](https://docs.flutter.dev/development/data-and-backend/state-mgmt/options) - Security best practices

### Data Persistence
- [Persistence Options](https://docs.flutter.dev/cookbook/persistence) - Flutter persistence cookbook
- [Data & Backend](https://docs.flutter.dev/development/data-and-backend) - Official data handling guide
- [JSON Serialization](https://docs.flutter.dev/development/data-and-backend/json) - Working with JSON

### Best Practices
- [Security Best Practices](https://docs.flutter.dev/security) - Flutter security guide
- [Effective Dart](https://dart.dev/guides/language/effective-dart) - Dart best practices
