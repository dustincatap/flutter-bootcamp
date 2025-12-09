# Networking in Flutter

Networking is essential for modern mobile applications. In this guide, you'll learn how to connect your Flutter app to the internet, fetch data from REST APIs, and parse JSON responses.

## Understanding REST APIs

**REST (Representational State Transfer)** is an architectural style for building web services. REST APIs allow applications to communicate over HTTP using standard methods:

- **GET**: Retrieve data from the server
- **POST**: Send new data to the server
- **PUT**: Update existing data on the server
- **DELETE**: Remove data from the server

In this guide, we'll focus on **GET** requests to fetch data from APIs.

## The http Package

Flutter's `http` package provides a simple way to make HTTP requests. It's the most commonly used package for networking in Flutter.

### Step 1: Add the http Package

Add the `http` package to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0
```

Run the following command to install the package:

```bash
flutter pub get
```

### Step 2: Import the Package

Import the http package in your Dart file:

```dart
import 'package:http/http.dart' as http;
import 'dart:convert'; // For JSON decoding
```

**Note**: We import `http` with the `as http` prefix to avoid naming conflicts.

## Making GET Requests

A GET request retrieves data from a specified URL.

### Basic GET Request

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class BasicGetExample extends StatefulWidget {
  const BasicGetExample({super.key});

  @override
  State<BasicGetExample> createState() => _BasicGetExampleState();
}

class _BasicGetExampleState extends State<BasicGetExample> {
  String _data = 'No data yet';
  bool _isLoading = false;

  Future<void> fetchData() async {
    setState(() {
      _isLoading = true;
    });

    try {
      // Make GET request
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      // Check if request was successful
      if (response.statusCode == 200) {
        setState(() {
          _data = response.body;
          _isLoading = false;
        });
      } else {
        setState(() {
          _data = 'Error: ${response.statusCode}';
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _data = 'Error: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic GET Request'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            if (_isLoading)
              const CircularProgressIndicator()
            else
              Padding(
                padding: const EdgeInsets.all(16.0),
                child: Text(_data),
              ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: fetchData,
              child: const Text('Fetch Data'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Understanding the Code

1. **Uri.parse()**: Converts a string URL into a Uri object
2. **await http.get()**: Makes an asynchronous GET request
3. **response.statusCode**: HTTP status code (200 = success, 404 = not found, etc.)
4. **response.body**: The response data as a string
5. **try-catch**: Handles network errors

### HTTP Status Codes

Common HTTP status codes:

- **200**: OK - Request succeeded
- **201**: Created - Resource created successfully
- **400**: Bad Request - Invalid request
- **401**: Unauthorized - Authentication required
- **404**: Not Found - Resource doesn't exist
- **500**: Internal Server Error - Server error

## JSON Decoding

Most APIs return data in JSON (JavaScript Object Notation) format. We need to decode this JSON string into Dart objects.

### Understanding JSON

JSON is a lightweight data format:

```json
{
  "id": 1,
  "title": "Hello World",
  "completed": false
}
```

### Decoding Simple JSON

```dart
import 'dart:convert';

// JSON string
String jsonString = '{"name": "John", "age": 30}';

// Decode to Map
Map<String, dynamic> user = jsonDecode(jsonString);

// Access values
print(user['name']); // John
print(user['age']);  // 30
```

### Decoding JSON Arrays

```dart
String jsonArray = '[{"id": 1}, {"id": 2}, {"id": 3}]';

// Decode to List
List<dynamic> items = jsonDecode(jsonArray);

// Access items
print(items[0]['id']); // 1
print(items.length);   // 3
```

### Complete Example: Fetching and Decoding JSON

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class JsonDecodingExample extends StatefulWidget {
  const JsonDecodingExample({super.key});

  @override
  State<JsonDecodingExample> createState() => _JsonDecodingExampleState();
}

class _JsonDecodingExampleState extends State<JsonDecodingExample> {
  Map<String, dynamic>? _post;
  bool _isLoading = false;
  String? _error;

  Future<void> fetchPost() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        // Decode JSON
        final Map<String, dynamic> data = jsonDecode(response.body);
        
        setState(() {
          _post = data;
          _isLoading = false;
        });
      } else {
        setState(() {
          _error = 'Failed to load post: ${response.statusCode}';
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Error: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('JSON Decoding'),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              if (_isLoading)
                const CircularProgressIndicator()
              else if (_error != null)
                Text(
                  _error!,
                  style: const TextStyle(color: Colors.red),
                )
              else if (_post != null)
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'ID: ${_post!['id']}',
                          style: const TextStyle(fontWeight: FontWeight.bold),
                        ),
                        const SizedBox(height: 8),
                        Text(
                          'Title: ${_post!['title']}',
                          style: const TextStyle(fontSize: 18),
                        ),
                        const SizedBox(height: 8),
                        Text('Body: ${_post!['body']}'),
                      ],
                    ),
                  ),
                )
              else
                const Text('Press the button to fetch data'),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _isLoading ? null : fetchPost,
                child: const Text('Fetch Post'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## Creating Model Classes

For better type safety and code organization, create model classes to represent your data.

### Step 1: Define a Model Class

```dart
class Post {
  final int id;
  final int userId;
  final String title;
  final String body;

  Post({
    required this.id,
    required this.userId,
    required this.title,
    required this.body,
  });

  // Factory constructor to create a Post from JSON
  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'] as int,
      userId: json['userId'] as int,
      title: json['title'] as String,
      body: json['body'] as String,
    );
  }

  // Convert Post to JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'userId': userId,
      'title': title,
      'body': body,
    };
  }
}
```

### Step 2: Use the Model Class

```dart
class PostExample extends StatefulWidget {
  const PostExample({super.key});

  @override
  State<PostExample> createState() => _PostExampleState();
}

class _PostExampleState extends State<PostExample> {
  Post? _post;
  bool _isLoading = false;
  String? _error;

  Future<void> fetchPost() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        final Map<String, dynamic> data = jsonDecode(response.body);
        
        setState(() {
          _post = Post.fromJson(data);
          _isLoading = false;
        });
      } else {
        setState(() {
          _error = 'Failed to load post';
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Error: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Post Model Example'),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              if (_isLoading)
                const CircularProgressIndicator()
              else if (_error != null)
                Text(_error!, style: const TextStyle(color: Colors.red))
              else if (_post != null)
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'ID: ${_post!.id}',
                          style: const TextStyle(fontWeight: FontWeight.bold),
                        ),
                        const SizedBox(height: 8),
                        Text(
                          _post!.title,
                          style: const TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 8),
                        Text(_post!.body),
                      ],
                    ),
                  ),
                )
              else
                const Text('Press the button to fetch data'),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _isLoading ? null : fetchPost,
                child: const Text('Fetch Post'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## Fetching Multiple Items

Most APIs return lists of data. Here's how to fetch and parse multiple items:

```dart
class User {
  final int id;
  final String name;
  final String email;

  User({
    required this.id,
    required this.name,
    required this.email,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as int,
      name: json['name'] as String,
      email: json['email'] as String,
    );
  }
}

class UserListExample extends StatefulWidget {
  const UserListExample({super.key});

  @override
  State<UserListExample> createState() => _UserListExampleState();
}

class _UserListExampleState extends State<UserListExample> {
  List<User> _users = [];
  bool _isLoading = false;
  String? _error;

  Future<void> fetchUsers() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/users'),
      );

      if (response.statusCode == 200) {
        // Decode JSON array
        final List<dynamic> data = jsonDecode(response.body);
        
        // Convert each item to User object
        final List<User> users = data.map((json) => User.fromJson(json)).toList();
        
        setState(() {
          _users = users;
          _isLoading = false;
        });
      } else {
        setState(() {
          _error = 'Failed to load users';
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Error: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('User List'),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: ElevatedButton(
              onPressed: _isLoading ? null : fetchUsers,
              child: const Text('Fetch Users'),
            ),
          ),
          if (_isLoading)
            const Center(child: CircularProgressIndicator())
          else if (_error != null)
            Center(
              child: Text(
                _error!,
                style: const TextStyle(color: Colors.red),
              ),
            )
          else
            Expanded(
              child: ListView.builder(
                itemCount: _users.length,
                itemBuilder: (context, index) {
                  final user = _users[index];
                  return ListTile(
                    leading: CircleAvatar(
                      child: Text(user.id.toString()),
                    ),
                    title: Text(user.name),
                    subtitle: Text(user.email),
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

## Error Handling

Proper error handling is crucial for a good user experience.

### Types of Errors

1. **Network Errors**: No internet connection, timeout
2. **HTTP Errors**: 404 Not Found, 500 Server Error
3. **Parsing Errors**: Invalid JSON format
4. **Type Errors**: Unexpected data types

### Comprehensive Error Handling

```dart
class RobustApiCall extends StatefulWidget {
  const RobustApiCall({super.key});

  @override
  State<RobustApiCall> createState() => _RobustApiCallState();
}

class _RobustApiCallState extends State<RobustApiCall> {
  Post? _post;
  bool _isLoading = false;
  String? _error;

  Future<void> fetchPost() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      ).timeout(
        const Duration(seconds: 10),
        onTimeout: () {
          throw Exception('Request timed out');
        },
      );

      if (response.statusCode == 200) {
        try {
          final Map<String, dynamic> data = jsonDecode(response.body);
          setState(() {
            _post = Post.fromJson(data);
            _isLoading = false;
          });
        } catch (e) {
          setState(() {
            _error = 'Failed to parse response';
            _isLoading = false;
          });
        }
      } else if (response.statusCode == 404) {
        setState(() {
          _error = 'Post not found';
          _isLoading = false;
        });
      } else {
        setState(() {
          _error = 'Server error: ${response.statusCode}';
          _isLoading = false;
        });
      }
    } on http.ClientException {
      setState(() {
        _error = 'Network error. Please check your connection.';
        _isLoading = false;
      });
    } on FormatException {
      setState(() {
        _error = 'Invalid response format';
        _isLoading = false;
      });
    } catch (e) {
      setState(() {
        _error = 'An unexpected error occurred: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Robust API Call'),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              if (_isLoading)
                const Column(
                  children: [
                    CircularProgressIndicator(),
                    SizedBox(height: 16),
                    Text('Loading...'),
                  ],
                )
              else if (_error != null)
                Column(
                  children: [
                    Icon(Icons.error_outline, size: 48, color: Colors.red),
                    const SizedBox(height: 16),
                    Text(
                      _error!,
                      style: const TextStyle(color: Colors.red),
                      textAlign: TextAlign.center,
                    ),
                    const SizedBox(height: 16),
                    ElevatedButton(
                      onPressed: fetchPost,
                      child: const Text('Retry'),
                    ),
                  ],
                )
              else if (_post != null)
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          _post!.title,
                          style: const TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 8),
                        Text(_post!.body),
                      ],
                    ),
                  ),
                )
              else
                const Text('Press the button to fetch data'),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _isLoading ? null : fetchPost,
                child: const Text('Fetch Post'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## Complete Example: Photo Gallery

Here's a complete example that fetches and displays photos from an API:

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class Photo {
  final int id;
  final String title;
  final String url;
  final String thumbnailUrl;

  Photo({
    required this.id,
    required this.title,
    required this.url,
    required this.thumbnailUrl,
  });

  factory Photo.fromJson(Map<String, dynamic> json) {
    return Photo(
      id: json['id'] as int,
      title: json['title'] as String,
      url: json['url'] as String,
      thumbnailUrl: json['thumbnailUrl'] as String,
    );
  }
}

class PhotoGallery extends StatefulWidget {
  const PhotoGallery({super.key});

  @override
  State<PhotoGallery> createState() => _PhotoGalleryState();
}

class _PhotoGalleryState extends State<PhotoGallery> {
  List<Photo> _photos = [];
  bool _isLoading = false;
  String? _error;

  @override
  void initState() {
    super.initState();
    fetchPhotos();
  }

  Future<void> fetchPhotos() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/photos?_limit=20'),
      );

      if (response.statusCode == 200) {
        final List<dynamic> data = jsonDecode(response.body);
        final List<Photo> photos = data.map((json) => Photo.fromJson(json)).toList();
        
        setState(() {
          _photos = photos;
          _isLoading = false;
        });
      } else {
        setState(() {
          _error = 'Failed to load photos';
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Error: $e';
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Photo Gallery'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: fetchPhotos,
          ),
        ],
      ),
      body: _buildBody(),
    );
  }

  Widget _buildBody() {
    if (_isLoading) {
      return const Center(
        child: CircularProgressIndicator(),
      );
    }

    if (_error != null) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.error_outline, size: 48, color: Colors.red),
            const SizedBox(height: 16),
            Text(_error!, style: const TextStyle(color: Colors.red)),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: fetchPhotos,
              child: const Text('Retry'),
            ),
          ],
        ),
      );
    }

    if (_photos.isEmpty) {
      return const Center(
        child: Text('No photos available'),
      );
    }

    return GridView.builder(
      padding: const EdgeInsets.all(8),
      gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
        crossAxisSpacing: 8,
        mainAxisSpacing: 8,
      ),
      itemCount: _photos.length,
      itemBuilder: (context, index) {
        final photo = _photos[index];
        return Card(
          clipBehavior: Clip.antiAlias,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Expanded(
                child: Image.network(
                  photo.thumbnailUrl,
                  fit: BoxFit.cover,
                  width: double.infinity,
                  loadingBuilder: (context, child, loadingProgress) {
                    if (loadingProgress == null) return child;
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
                    return const Center(
                      child: Icon(Icons.error),
                    );
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text(
                  photo.title,
                  maxLines: 2,
                  overflow: TextOverflow.ellipsis,
                  style: const TextStyle(fontSize: 12),
                ),
              ),
            ],
          ),
        );
      },
    );
  }
}
```

## Best Practices

### 1. Use Async/Await

```dart
// ✅ Good - Using async/await
Future<void> fetchData() async {
  final response = await http.get(uri);
  // Handle response
}

// ❌ Avoid - Using .then()
void fetchData() {
  http.get(uri).then((response) {
    // Handle response
  });
}
```

### 2. Add Timeouts

```dart
final response = await http.get(uri).timeout(
  const Duration(seconds: 10),
);
```

### 3. Handle All Error Cases

```dart
try {
  final response = await http.get(uri);
  if (response.statusCode == 200) {
    // Success
  } else {
    // HTTP error
  }
} catch (e) {
  // Network error
}
```

### 4. Show Loading States

```dart
if (_isLoading) {
  return const CircularProgressIndicator();
}
```

### 5. Use Model Classes

```dart
// ✅ Good - Type-safe
class User {
  final String name;
  factory User.fromJson(Map<String, dynamic> json) {
    return User(name: json['name']);
  }
}

// ❌ Avoid - Untyped
Map<String, dynamic> user = jsonDecode(response.body);
```

### 6. Separate API Logic

Create a separate service class for API calls:

```dart
class ApiService {
  static const String baseUrl = 'https://jsonplaceholder.typicode.com';

  Future<List<Post>> fetchPosts() async {
    final response = await http.get(Uri.parse('$baseUrl/posts'));
    
    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => Post.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load posts');
    }
  }
}

// Usage in widget
final apiService = ApiService();
final posts = await apiService.fetchPosts();
```

### 7. Handle Null Safety

```dart
factory Post.fromJson(Map<String, dynamic> json) {
  return Post(
    id: json['id'] as int? ?? 0,  // Provide default value
    title: json['title'] as String? ?? '',
  );
}
```

## Common Issues and Solutions

### Issue: No Internet Connection

**Solution**: Add internet permission and handle network errors

For Android, add to `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

For iOS, it's enabled by default.

### Issue: Certificate Errors

**Solution**: Ensure the API uses HTTPS, or configure certificate validation for development.

### Issue: Slow Response

**Solution**: Add loading indicators and timeouts

```dart
final response = await http.get(uri).timeout(
  const Duration(seconds: 10),
  onTimeout: () {
    throw Exception('Request timed out');
  },
);
```

### Issue: CORS Errors (Web)

**Solution**: CORS is a server-side issue. Ensure the API allows requests from your domain.

## Key Takeaways

1. **http package** is the standard way to make HTTP requests in Flutter
2. Use **async/await** for asynchronous operations
3. **Uri.parse()** converts strings to Uri objects
4. **jsonDecode()** converts JSON strings to Dart objects
5. Create **model classes** for type safety and better code organization
6. Always handle **errors** and **loading states**
7. Use **try-catch** to handle network errors
8. Check **response.statusCode** to verify success
9. Add **timeouts** to prevent hanging requests
10. Separate **API logic** into service classes for better organization

---

## References and Further Reading

### HTTP Package
- [http Package](https://pub.dev/packages/http) - Official http package documentation
- [Fetch Data from the Internet](https://docs.flutter.dev/cookbook/networking/fetch-data) - Official Flutter networking guide
- [Send Data to the Internet](https://docs.flutter.dev/cookbook/networking/send-data) - Sending data with POST requests

### JSON and Serialization
- [JSON and Serialization](https://docs.flutter.dev/development/data-and-backend/json) - Official JSON guide
- [dart:convert Library](https://api.dart.dev/stable/dart-convert/dart-convert-library.html) - JSON encoding/decoding

### Error Handling
- [Handle Errors in Dart](https://dart.dev/guides/libraries/futures-error-handling) - Error handling with Futures
- [Exception Class](https://api.dart.dev/stable/dart-core/Exception-class.html) - Exception documentation

### Best Practices
- [Effective Dart](https://dart.dev/guides/language/effective-dart) - Dart style guide
- [Networking Best Practices](https://docs.flutter.dev/perf/best-practices) - Performance best practices
