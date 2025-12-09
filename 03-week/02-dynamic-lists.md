# Working with Dynamic Lists in Flutter

Dynamic lists are essential for displaying collections of data in mobile applications. In this guide, you'll learn how to use ListView and GridView to display scrollable lists, and FutureBuilder to handle asynchronous data loading efficiently.

## Understanding List Widgets

Flutter provides powerful widgets for displaying scrollable lists of data:

- **ListView**: Displays items in a vertical (or horizontal) scrolling list
- **GridView**: Displays items in a 2D grid with multiple columns
- **FutureBuilder**: Builds widgets based on the state of a Future (async operation)

## ListView

ListView is one of the most commonly used widgets in Flutter. It creates a scrollable list of widgets.

### Basic ListView

```dart
import 'package:flutter/material.dart';

class BasicListView extends StatelessWidget {
  const BasicListView({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic ListView'),
      ),
      body: ListView(
        padding: const EdgeInsets.all(8),
        children: const [
          Card(
            child: ListTile(
              leading: Icon(Icons.person),
              title: Text('John Doe'),
              subtitle: Text('Software Developer'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.person),
              title: Text('Jane Smith'),
              subtitle: Text('Designer'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.person),
              title: Text('Bob Johnson'),
              subtitle: Text('Product Manager'),
            ),
          ),
        ],
      ),
    );
  }
}
```

### ListView.builder

For large or dynamic lists, use `ListView.builder` which builds items on demand (lazy loading):

```dart
class ListViewBuilderExample extends StatelessWidget {
  const ListViewBuilderExample({super.key});

  final List<String> items = const [
    'Item 1',
    'Item 2',
    'Item 3',
    'Item 4',
    'Item 5',
    'Item 6',
    'Item 7',
    'Item 8',
    'Item 9',
    'Item 10',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ListView.builder'),
      ),
      body: ListView.builder(
        itemCount: items.length,
        itemBuilder: (context, index) {
          return ListTile(
            leading: CircleAvatar(
              child: Text('${index + 1}'),
            ),
            title: Text(items[index]),
            subtitle: Text('Description for ${items[index]}'),
            trailing: const Icon(Icons.arrow_forward_ios),
            onTap: () {
              print('Tapped on ${items[index]}');
            },
          );
        },
      ),
    );
  }
}
```

### ListView.separated

Add separators between list items:

```dart
class ListViewSeparatedExample extends StatelessWidget {
  const ListViewSeparatedExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ListView.separated'),
      ),
      body: ListView.separated(
        itemCount: 20,
        separatorBuilder: (context, index) {
          return const Divider(
            thickness: 1,
            color: Colors.grey,
          );
        },
        itemBuilder: (context, index) {
          return ListTile(
            leading: Icon(Icons.star, color: Colors.amber),
            title: Text('Item ${index + 1}'),
            subtitle: Text('Subtitle for item ${index + 1}'),
          );
        },
      ),
    );
  }
}
```

### Dynamic ListView with Data

```dart
class Task {
  final String title;
  final String description;
  final bool isCompleted;

  Task({
    required this.title,
    required this.description,
    this.isCompleted = false,
  });
}

class DynamicListView extends StatefulWidget {
  const DynamicListView({super.key});

  @override
  State<DynamicListView> createState() => _DynamicListViewState();
}

class _DynamicListViewState extends State<DynamicListView> {
  final List<Task> tasks = [
    Task(title: 'Buy groceries', description: 'Milk, eggs, bread'),
    Task(title: 'Complete Flutter project', description: 'Finish week 3 materials'),
    Task(title: 'Exercise', description: '30 minutes cardio', isCompleted: true),
    Task(title: 'Read a book', description: 'Read for 30 minutes'),
    Task(title: 'Call mom', description: 'Check in with family'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Task List'),
      ),
      body: ListView.builder(
        itemCount: tasks.length,
        itemBuilder: (context, index) {
          final task = tasks[index];
          return Card(
            margin: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
            child: ListTile(
              leading: Icon(
                task.isCompleted ? Icons.check_circle : Icons.radio_button_unchecked,
                color: task.isCompleted ? Colors.green : Colors.grey,
              ),
              title: Text(
                task.title,
                style: TextStyle(
                  decoration: task.isCompleted ? TextDecoration.lineThrough : null,
                ),
              ),
              subtitle: Text(task.description),
              trailing: IconButton(
                icon: const Icon(Icons.delete),
                onPressed: () {
                  setState(() {
                    tasks.removeAt(index);
                  });
                },
              ),
              onTap: () {
                setState(() {
                  tasks[index] = Task(
                    title: task.title,
                    description: task.description,
                    isCompleted: !task.isCompleted,
                  );
                });
              },
            ),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            tasks.add(Task(
              title: 'New Task',
              description: 'Task description',
            ));
          });
        },
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

### Horizontal ListView

```dart
class HorizontalListView extends StatelessWidget {
  const HorizontalListView({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Horizontal ListView'),
      ),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Text(
              'Categories',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
          ),
          SizedBox(
            height: 120,
            child: ListView.builder(
              scrollDirection: Axis.horizontal,
              itemCount: 10,
              padding: const EdgeInsets.symmetric(horizontal: 8),
              itemBuilder: (context, index) {
                return Container(
                  width: 100,
                  margin: const EdgeInsets.symmetric(horizontal: 8),
                  decoration: BoxDecoration(
                    color: Colors.primaries[index % Colors.primaries.length],
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Center(
                    child: Text(
                      'Category ${index + 1}',
                      style: const TextStyle(
                        color: Colors.white,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
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

## GridView

GridView displays items in a 2D scrollable grid. It's perfect for image galleries, product catalogs, and similar layouts.

### Basic GridView

```dart
class BasicGridView extends StatelessWidget {
  const BasicGridView({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic GridView'),
      ),
      body: GridView.count(
        crossAxisCount: 2, // Number of columns
        padding: const EdgeInsets.all(8),
        crossAxisSpacing: 8, // Horizontal spacing
        mainAxisSpacing: 8,  // Vertical spacing
        children: List.generate(20, (index) {
          return Card(
            color: Colors.primaries[index % Colors.primaries.length],
            child: Center(
              child: Text(
                'Item ${index + 1}',
                style: const TextStyle(
                  color: Colors.white,
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
          );
        }),
      ),
    );
  }
}
```

### GridView.builder

For dynamic grids with many items:

```dart
class GridViewBuilderExample extends StatelessWidget {
  const GridViewBuilderExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('GridView.builder'),
      ),
      body: GridView.builder(
        padding: const EdgeInsets.all(8),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3,
          crossAxisSpacing: 8,
          mainAxisSpacing: 8,
          childAspectRatio: 1.0, // Width to height ratio
        ),
        itemCount: 50,
        itemBuilder: (context, index) {
          return Container(
            decoration: BoxDecoration(
              color: Colors.blue[(index % 9 + 1) * 100],
              borderRadius: BorderRadius.circular(8),
            ),
            child: Center(
              child: Text(
                '${index + 1}',
                style: const TextStyle(
                  color: Colors.white,
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}
```

### GridView.extent

Create grids with maximum tile width:

```dart
class GridViewExtentExample extends StatelessWidget {
  const GridViewExtentExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('GridView.extent'),
      ),
      body: GridView.extent(
        maxCrossAxisExtent: 150, // Maximum width of each tile
        padding: const EdgeInsets.all(8),
        crossAxisSpacing: 8,
        mainAxisSpacing: 8,
        children: List.generate(20, (index) {
          return Card(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  Icons.image,
                  size: 48,
                  color: Colors.primaries[index % Colors.primaries.length],
                ),
                const SizedBox(height: 8),
                Text('Item ${index + 1}'),
              ],
            ),
          );
        }),
      ),
    );
  }
}
```

### Image Grid

```dart
class ImageGrid extends StatelessWidget {
  const ImageGrid({super.key});

  final List<String> imageUrls = const [
    'https://picsum.photos/200/200?random=1',
    'https://picsum.photos/200/200?random=2',
    'https://picsum.photos/200/200?random=3',
    'https://picsum.photos/200/200?random=4',
    'https://picsum.photos/200/200?random=5',
    'https://picsum.photos/200/200?random=6',
    'https://picsum.photos/200/200?random=7',
    'https://picsum.photos/200/200?random=8',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Image Grid'),
      ),
      body: GridView.builder(
        padding: const EdgeInsets.all(8),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
          crossAxisSpacing: 8,
          mainAxisSpacing: 8,
        ),
        itemCount: imageUrls.length,
        itemBuilder: (context, index) {
          return Card(
            clipBehavior: Clip.antiAlias,
            child: Image.network(
              imageUrls[index],
              fit: BoxFit.cover,
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
          );
        },
      ),
    );
  }
}
```

## FutureBuilder

FutureBuilder is a widget that builds itself based on the latest snapshot of interaction with a Future. It's perfect for displaying data that needs to be loaded asynchronously.

### Understanding FutureBuilder

FutureBuilder has three main states:

1. **Waiting**: Future hasn't completed yet
2. **Done with data**: Future completed successfully
3. **Done with error**: Future completed with an error

### Basic FutureBuilder

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class Post {
  final int id;
  final String title;
  final String body;

  Post({
    required this.id,
    required this.title,
    required this.body,
  });

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'] as int,
      title: json['title'] as String,
      body: json['body'] as String,
    );
  }
}

class BasicFutureBuilder extends StatelessWidget {
  const BasicFutureBuilder({super.key});

  Future<Post> fetchPost() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
    );

    if (response.statusCode == 200) {
      return Post.fromJson(jsonDecode(response.body));
    } else {
      throw Exception('Failed to load post');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('FutureBuilder Example'),
      ),
      body: Center(
        child: FutureBuilder<Post>(
          future: fetchPost(),
          builder: (context, snapshot) {
            // Check connection state
            if (snapshot.connectionState == ConnectionState.waiting) {
              return const CircularProgressIndicator();
            }

            // Check for errors
            if (snapshot.hasError) {
              return Text('Error: ${snapshot.error}');
            }

            // Check if data is available
            if (snapshot.hasData) {
              final post = snapshot.data!;
              return Padding(
                padding: const EdgeInsets.all(16.0),
                child: Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                      mainAxisSize: MainAxisSize.min,
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          post.title,
                          style: const TextStyle(
                            fontSize: 20,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 8),
                        Text(post.body),
                      ],
                    ),
                  ),
                ),
              );
            }

            return const Text('No data');
          },
        ),
      ),
    );
  }
}
```

### FutureBuilder with ListView

Combine FutureBuilder with ListView to display a list of data from an API:

```dart
class User {
  final int id;
  final String name;
  final String email;
  final String phone;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.phone,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as int,
      name: json['name'] as String,
      email: json['email'] as String,
      phone: json['phone'] as String,
    );
  }
}

class UserListFutureBuilder extends StatelessWidget {
  const UserListFutureBuilder({super.key});

  Future<List<User>> fetchUsers() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users'),
    );

    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => User.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load users');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Users List'),
      ),
      body: FutureBuilder<List<User>>(
        future: fetchUsers(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (snapshot.hasError) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  const Icon(Icons.error_outline, size: 48, color: Colors.red),
                  const SizedBox(height: 16),
                  Text(
                    'Error: ${snapshot.error}',
                    style: const TextStyle(color: Colors.red),
                  ),
                ],
              ),
            );
          }

          if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return const Center(
              child: Text('No users found'),
            );
          }

          final users = snapshot.data!;

          return ListView.builder(
            itemCount: users.length,
            itemBuilder: (context, index) {
              final user = users[index];
              return Card(
                margin: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                child: ListTile(
                  leading: CircleAvatar(
                    child: Text(user.id.toString()),
                  ),
                  title: Text(user.name),
                  subtitle: Text(user.email),
                  trailing: const Icon(Icons.arrow_forward_ios),
                  onTap: () {
                    // Navigate to user detail screen
                  },
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

### FutureBuilder with GridView

```dart
class Photo {
  final int id;
  final String title;
  final String thumbnailUrl;

  Photo({
    required this.id,
    required this.title,
    required this.thumbnailUrl,
  });

  factory Photo.fromJson(Map<String, dynamic> json) {
    return Photo(
      id: json['id'] as int,
      title: json['title'] as String,
      thumbnailUrl: json['thumbnailUrl'] as String,
    );
  }
}

class PhotoGridFutureBuilder extends StatelessWidget {
  const PhotoGridFutureBuilder({super.key});

  Future<List<Photo>> fetchPhotos() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/photos?_limit=30'),
    );

    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => Photo.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load photos');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Photo Gallery'),
      ),
      body: FutureBuilder<List<Photo>>(
        future: fetchPhotos(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (snapshot.hasError) {
            return Center(
              child: Text('Error: ${snapshot.error}'),
            );
          }

          if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return const Center(
              child: Text('No photos found'),
            );
          }

          final photos = snapshot.data!;

          return GridView.builder(
            padding: const EdgeInsets.all(8),
            gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 3,
              crossAxisSpacing: 8,
              mainAxisSpacing: 8,
            ),
            itemCount: photos.length,
            itemBuilder: (context, index) {
              final photo = photos[index];
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
                      ),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(4.0),
                      child: Text(
                        photo.title,
                        maxLines: 1,
                        overflow: TextOverflow.ellipsis,
                        style: const TextStyle(fontSize: 10),
                      ),
                    ),
                  ],
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

### Refreshable FutureBuilder

Add pull-to-refresh functionality:

```dart
class RefreshableList extends StatefulWidget {
  const RefreshableList({super.key});

  @override
  State<RefreshableList> createState() => _RefreshableListState();
}

class _RefreshableListState extends State<RefreshableList> {
  late Future<List<User>> _usersFuture;

  @override
  void initState() {
    super.initState();
    _usersFuture = fetchUsers();
  }

  Future<List<User>> fetchUsers() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users'),
    );

    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => User.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load users');
    }
  }

  Future<void> _refreshData() async {
    setState(() {
      _usersFuture = fetchUsers();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Refreshable List'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _refreshData,
          ),
        ],
      ),
      body: RefreshIndicator(
        onRefresh: _refreshData,
        child: FutureBuilder<List<User>>(
          future: _usersFuture,
          builder: (context, snapshot) {
            if (snapshot.connectionState == ConnectionState.waiting) {
              return const Center(
                child: CircularProgressIndicator(),
              );
            }

            if (snapshot.hasError) {
              return Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    const Icon(Icons.error_outline, size: 48, color: Colors.red),
                    const SizedBox(height: 16),
                    Text('Error: ${snapshot.error}'),
                    const SizedBox(height: 16),
                    ElevatedButton(
                      onPressed: _refreshData,
                      child: const Text('Retry'),
                    ),
                  ],
                ),
              );
            }

            if (!snapshot.hasData || snapshot.data!.isEmpty) {
              return const Center(
                child: Text('No users found'),
              );
            }

            final users = snapshot.data!;

            return ListView.builder(
              itemCount: users.length,
              itemBuilder: (context, index) {
                final user = users[index];
                return ListTile(
                  leading: CircleAvatar(
                    child: Text(user.id.toString()),
                  ),
                  title: Text(user.name),
                  subtitle: Text(user.email),
                );
              },
            );
          },
        ),
      ),
    );
  }
}
```

## Complete Example: Product Catalog

Here's a complete example combining all concepts:

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class Product {
  final int id;
  final String title;
  final double price;
  final String description;
  final String category;
  final String image;

  Product({
    required this.id,
    required this.title,
    required this.price,
    required this.description,
    required this.category,
    required this.image,
  });

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'] as int,
      title: json['title'] as String,
      price: (json['price'] as num).toDouble(),
      description: json['description'] as String,
      category: json['category'] as String,
      image: json['image'] as String,
    );
  }
}

class ProductCatalog extends StatefulWidget {
  const ProductCatalog({super.key});

  @override
  State<ProductCatalog> createState() => _ProductCatalogState();
}

class _ProductCatalogState extends State<ProductCatalog> {
  late Future<List<Product>> _productsFuture;
  bool _isGridView = true;

  @override
  void initState() {
    super.initState();
    _productsFuture = fetchProducts();
  }

  Future<List<Product>> fetchProducts() async {
    final response = await http.get(
      Uri.parse('https://fakestoreapi.com/products'),
    );

    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => Product.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load products');
    }
  }

  Future<void> _refreshProducts() async {
    setState(() {
      _productsFuture = fetchProducts();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Product Catalog'),
        actions: [
          IconButton(
            icon: Icon(_isGridView ? Icons.list : Icons.grid_view),
            onPressed: () {
              setState(() {
                _isGridView = !_isGridView;
              });
            },
          ),
        ],
      ),
      body: FutureBuilder<List<Product>>(
        future: _productsFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (snapshot.hasError) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  const Icon(Icons.error_outline, size: 48, color: Colors.red),
                  const SizedBox(height: 16),
                  Text('Error: ${snapshot.error}'),
                  const SizedBox(height: 16),
                  ElevatedButton(
                    onPressed: _refreshProducts,
                    child: const Text('Retry'),
                  ),
                ],
              ),
            );
          }

          if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return const Center(
              child: Text('No products found'),
            );
          }

          final products = snapshot.data!;

          return RefreshIndicator(
            onRefresh: _refreshProducts,
            child: _isGridView
                ? _buildGridView(products)
                : _buildListView(products),
          );
        },
      ),
    );
  }

  Widget _buildGridView(List<Product> products) {
    return GridView.builder(
      padding: const EdgeInsets.all(8),
      gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
        crossAxisSpacing: 8,
        mainAxisSpacing: 8,
        childAspectRatio: 0.7,
      ),
      itemCount: products.length,
      itemBuilder: (context, index) {
        final product = products[index];
        return Card(
          clipBehavior: Clip.antiAlias,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Expanded(
                child: Image.network(
                  product.image,
                  fit: BoxFit.contain,
                  width: double.infinity,
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      product.title,
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
                      style: const TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 4),
                    Text(
                      '\$${product.price.toStringAsFixed(2)}',
                      style: const TextStyle(
                        color: Colors.green,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ],
                ),
              ),
            ],
          ),
        );
      },
    );
  }

  Widget _buildListView(List<Product> products) {
    return ListView.builder(
      itemCount: products.length,
      itemBuilder: (context, index) {
        final product = products[index];
        return Card(
          margin: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
          child: ListTile(
            leading: Image.network(
              product.image,
              width: 50,
              height: 50,
              fit: BoxFit.contain,
            ),
            title: Text(product.title),
            subtitle: Text(
              '\$${product.price.toStringAsFixed(2)}',
              style: const TextStyle(
                color: Colors.green,
                fontWeight: FontWeight.bold,
              ),
            ),
            trailing: const Icon(Icons.arrow_forward_ios),
            onTap: () {
              // Navigate to product detail
            },
          ),
        );
      },
    );
  }
}
```

## Best Practices

### 1. Use ListView.builder for Long Lists

```dart
// ✅ Good - Efficient for long lists
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) => ListTile(title: Text(items[index])),
)

// ❌ Avoid - Creates all items at once
ListView(
  children: items.map((item) => ListTile(title: Text(item))).toList(),
)
```

### 2. Store Future in State Variable

```dart
// ✅ Good - Future created once
class MyWidget extends StatefulWidget {
  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  late Future<Data> _dataFuture;

  @override
  void initState() {
    super.initState();
    _dataFuture = fetchData();
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder(future: _dataFuture, ...);
  }
}

// ❌ Avoid - Creates new Future on every build
Widget build(BuildContext context) {
  return FutureBuilder(future: fetchData(), ...);
}
```

### 3. Handle All States

```dart
// ✅ Good - Handles all states
if (snapshot.connectionState == ConnectionState.waiting) {
  return CircularProgressIndicator();
}
if (snapshot.hasError) {
  return Text('Error: ${snapshot.error}');
}
if (!snapshot.hasData) {
  return Text('No data');
}
return Text(snapshot.data!);
```

### 4. Add Pull-to-Refresh

```dart
RefreshIndicator(
  onRefresh: _refreshData,
  child: ListView(...),
)
```

### 5. Use Separators for Better UX

```dart
ListView.separated(
  itemCount: items.length,
  separatorBuilder: (context, index) => Divider(),
  itemBuilder: (context, index) => ListTile(...),
)
```

### 6. Optimize Grid Performance

```dart
// Use childAspectRatio to control item size
SliverGridDelegateWithFixedCrossAxisCount(
  crossAxisCount: 2,
  childAspectRatio: 0.7, // Width / Height
)
```

### 7. Add Loading Indicators for Images

```dart
Image.network(
  url,
  loadingBuilder: (context, child, loadingProgress) {
    if (loadingProgress == null) return child;
    return CircularProgressIndicator();
  },
)
```

## Common Issues and Solutions

### Issue: FutureBuilder Rebuilding Too Often

**Solution**: Store the Future in a state variable in `initState()`

```dart
late Future<Data> _future;

@override
void initState() {
  super.initState();
  _future = fetchData();
}
```

### Issue: List Not Updating After setState

**Solution**: Make sure you're modifying the list correctly

```dart
setState(() {
  _items.add(newItem); // ✅ Modifies the list
});
```

### Issue: GridView Items Not Sizing Correctly

**Solution**: Adjust `childAspectRatio` or use `SliverGridDelegateWithMaxCrossAxisExtent`

```dart
SliverGridDelegateWithMaxCrossAxisExtent(
  maxCrossAxisExtent: 200,
  childAspectRatio: 1.0,
)
```

### Issue: Infinite List Scrolling

**Solution**: Implement pagination or limit API results

```dart
final response = await http.get(
  Uri.parse('https://api.example.com/items?_limit=50'),
);
```

## Key Takeaways

1. **ListView.builder** is efficient for dynamic lists with many items
2. **GridView** is perfect for displaying items in a grid layout
3. **FutureBuilder** handles asynchronous data loading and different states
4. Store **Future in state variable** to prevent unnecessary API calls
5. Always handle **loading, error, and no-data states**
6. Use **RefreshIndicator** for pull-to-refresh functionality
7. **ListView.separated** adds dividers between items
8. **GridView.builder** is efficient for large grids
9. Use appropriate **gridDelegate** for grid layouts
10. Optimize performance with **const constructors** and **keys**

---

## References and Further Reading

### ListView
- [ListView Class](https://api.flutter.dev/flutter/widgets/ListView-class.html) - ListView API documentation
- [Lists Cookbook](https://docs.flutter.dev/cookbook/lists) - Working with lists in Flutter
- [ListView.builder](https://api.flutter.dev/flutter/widgets/ListView/ListView.builder.html) - ListView.builder documentation

### GridView
- [GridView Class](https://api.flutter.dev/flutter/widgets/GridView-class.html) - GridView API documentation
- [Grid Lists](https://docs.flutter.dev/cookbook/lists/grid-lists) - Creating grid lists
- [SliverGridDelegate](https://api.flutter.dev/flutter/rendering/SliverGridDelegate-class.html) - Grid layout configuration

### FutureBuilder
- [FutureBuilder Class](https://api.flutter.dev/flutter/widgets/FutureBuilder-class.html) - FutureBuilder API documentation
- [Asynchronous Programming](https://dart.dev/codelabs/async-await) - Async/await in Dart
- [ConnectionState](https://api.flutter.dev/flutter/widgets/ConnectionState.html) - Future connection states

### Performance
- [Performance Best Practices](https://docs.flutter.dev/perf/best-practices) - Flutter performance tips
- [ListView Performance](https://docs.flutter.dev/perf/rendering/best-practices#avoid-rebuilding-large-widget-trees) - Optimizing list performance
