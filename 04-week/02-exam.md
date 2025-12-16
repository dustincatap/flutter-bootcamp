# Week 4 Final Project: E-Commerce Shopping App

Congratulations on completing Week 4! Now it's time to put everything you've learned into practice by building a full-featured e-commerce shopping app with **Cubit** state management, API integration, and local data persistence.

## Project Overview

You will create a Flutter application that connects to a REST API to display and manage products in a shopping cart. This project will help you demonstrate your understanding of:
- State management with **Cubit** pattern
- Making HTTP requests (GET)
- Parsing JSON data
- Displaying data in dynamic lists (ListView/GridView)
- Local data persistence with SharedPreferences
- Shopping cart management
- Search and filter functionality (local)
- Dark mode theming
- Error handling and loading states

**Important:** This project focuses on **Cubit** for state management. Use Cubit for all your blocs (products, cart, theme).

## Project Requirements

Your E-Commerce Shopping app must include the following:

1. **Home Screen (Product List)**
   - Display all products fetched from API
   - Show product image, title, price, and rating
   - Grid or list view layout (toggle between views)
   - Pull-to-refresh functionality
   - Search bar (local filtering)
   - Category filter tabs (local filtering based on product.category)
   - Loading indicator while fetching data
   - Error handling with retry option
   - Navigate to Product Detail on tap
   - Cart icon with badge showing item count

2. **Product Detail Screen**
   - Display product from API (GET single product endpoint)
   - Show full product information:
     - High-quality image
     - Title and description
     - Price and rating
     - Category
   - "Add to Cart" button with quantity selector
   - Visual feedback when added to cart
   - Back button to return to home screen

3. **Shopping Cart Screen**
   - Display all items added to cart (locally managed)
   - Show product thumbnail, title, price, and quantity
   - Adjust quantity (+/- buttons)
   - Remove item from cart
   - Calculate and display:
     - Subtotal per item
     - Total price
     - Item count
   - Empty cart state with helpful message
   - Cart data persisted locally using SharedPreferences

4. **Settings Screen**
   - Dark mode toggle (persist preference)
   - Display app version
   - About section
   - Settings persisted using SharedPreferences
   - **Note:** You may add other user preferences like default view mode (List/Grid)

5. **Search and Filter (Local Only)**
   - Search products by title or description
   - Filter by category (extract from product data)
   - Filter by price range
   - Sort options:
     - Price: Low to High
     - Price: High to Low
     - Rating
     - Name (A-Z)
   - All filtering done on locally cached product data

### API Endpoints to Use

Use FakeStore API: `https://fakestoreapi.com/`

**Required Endpoints:**

1. **Get all products (GET)**
   ```
   GET https://fakestoreapi.com/products
   ```

2. **Get single product (GET)**
   ```
   GET https://fakestoreapi.com/products/{id}
   ```

**Note:** All other functionality (search, filter, categories, cart management) must be implemented **locally** without additional API calls

### Technical Requirements

1. **Project Setup**
   - Create a new Flutter project named `ecommerce_app` or similar
   - Add required dependencies:
     - `flutter_bloc: ^8.1.3`
     - `http: ^1.1.0`
     - `shared_preferences: ^2.2.2`
     - (Optional) `cached_network_image` for image caching
   - Use proper project structure with separate folders

2. **State Management**
   - **Use Cubit pattern for all state management**
   - Implement separate Cubits for:
     - Product list management (`ProductCubit`)
     - Shopping cart (`CartCubit`)
     - Theme/settings (`ThemeCubit`)
   - Use BlocProvider, BlocBuilder, and BlocListener appropriately
   - Handle loading, success, and error states
   - Use direct method calls (e.g., `loadProducts()`, `addToCart()`)

3. **HTTP Requests**
   - Create an API service class to handle all HTTP requests
   - Implement GET methods for products
   - Add proper error handling for network errors
   - Use try-catch blocks for exception handling
   - Set timeouts for requests (10 seconds)

4. **JSON Parsing**
   - Create a Product model class with fromJson and toJson methods
   - Create a CartItem model class for cart management
   - Parse API responses correctly
   - Handle null values appropriately

5. **UI Components**
   - Use BlocBuilder to display state-based UI
   - Implement ListView.builder and GridView.builder for product list
   - Use RefreshIndicator for pull-to-refresh
   - Show CircularProgressIndicator for loading states
   - Display error messages with retry button
   - Implement search and filter UI

6. **Local Storage**
   - Save shopping cart data using SharedPreferences (JSON serialization)
   - Cache product list when successfully fetched from API
   - Save dark mode preference
   - Save view mode preference (List/Grid)
   - Load cached products when offline

## Getting Started

### Step 1: Create the Project

```bash
flutter create ecommerce_app
cd ecommerce_app
```

### Step 2: Add Dependencies

Update `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_bloc: ^8.1.3
  http: ^1.1.0
  shared_preferences: ^2.2.2
  cached_network_image: ^3.3.0  # Optional but recommended
```

Run:
```bash
flutter pub get
```

## Tips and Best Practices

### 1. State Management with Cubit

- **Use Cubit for all state management** (simpler than full Bloc)
- Create separate Cubits for products, cart, and theme
- Always emit new state objects (immutable states)
- Handle all state cases: initial, loading, success, error
- Use BlocListener for side effects (navigation, snackbars)
- Call Cubit methods directly: `context.read<ProductCubit>().loadProducts()`
- Use `copyWith` for state updates

### 2. Error Handling

- Always wrap HTTP requests in try-catch blocks
- Check response status codes before processing
- Throw meaningful exceptions with error details
- Display user-friendly error messages in the UI
- Provide retry options on error states

### 3. Loading States

- Use BlocBuilder to react to state changes
- Show CircularProgressIndicator while loading
- Handle loading states appropriately
- Display error states with retry options

### 4. Cart Management with CartCubit

- Store cart as List<CartItem> with product ID, quantity
- Persist cart to SharedPreferences as JSON
- Load cart on app startup in Cubit constructor
- Update cart state through Cubit methods:
  - `addItem(productId, title, price)`
  - `removeItem(productId)`
  - `updateQuantity(productId, quantity)`
  - `clearCart()`
- Calculate totals in the state or getter methods
- Save to SharedPreferences after every cart change

### 5. Local Search and Filter

- Keep all products in memory after fetching
- Filter products locally based on search text
- Filter by category without API calls
- Implement sort functions on local data
- Update UI reactively through Cubit state changes
- Add methods to ProductCubit:
  - `searchProducts(query)`
  - `filterByCategory(category)`
  - `sortProducts(sortOption)`

### 6. Cubit Best Practices

- Keep Cubits simple and focused
- Use `emit()` to output new states
- Don't emit states in loops (use a single emit with updated list)
- Use `copyWith` for partial state updates
- Handle async operations with try-catch
- Close Cubits automatically with BlocProvider
- Test Cubits independently from UI

## Bonus Challenges (Optional)

1. **Advanced Features**
   - Add product image zoom functionality
   - Implement pagination or infinite scroll
   - Add favorites/wishlist functionality
   - Swipe gestures on cart items
   - Product comparison feature

2. **Better UX**
   - Add Hero animations for product images
   - Implement skeleton loading screens
   - Add success/error snackbars
   - Smooth page transitions
   - Animated cart badge

3. **Offline Support**
   - Full offline mode with cached products
   - Show offline indicator in app bar

## Submission Guidelines

### Step 1: Prepare Your Code

1. **Test thoroughly**
   - Test all screens and navigation
   - Test state management (add to cart, remove, update quantity)
   - Test offline mode (turn off internet)
   - Test search and filter functionality
   - Test theme switching (light/dark)
   - Verify error handling works
   - Test pull-to-refresh
   - Test list and grid views

2. **Clean up**
   - Remove debug print statements
   - Remove commented code
   - Format code properly
   - Add necessary comments
   - Ensure all states are handled

### Step 2: Create a README

Create a comprehensive `README.md`:

- Project title and description
- List of features implemented
- API endpoints used
- Technologies and packages used (Bloc/Cubit, HTTP, SharedPreferences)
- Project structure diagram
- State management architecture explanation
- How to run the project
- Screenshots of the app
- Checklist of implemented features
- (Optional) What you learned and challenges faced

### Step 3: Take Screenshots

Capture screenshots showing:
1. Home screen with product grid
2. Home screen with product list view
3. Product detail screen
4. Shopping cart with items
5. Empty cart state
6. Settings screen with dark mode
7. Search/filter in action
8. Error state with retry button
9. Dark mode theme

### Step 4: Create GitHub Repository

Initialize git in your project:
```bash
git init
git add .
git commit -m "Initial commit: E-Commerce Shopping App with Bloc/Cubit"
```

Create a new repository on GitHub and push your code:
```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git branch -M main
git push -u origin main
```

### Step 5: Submit Your Work

Submit your GitHub repository URL containing:
- ✅ Complete Flutter project code
- ✅ All required features implemented
- ✅ Product model with JSON parsing
- ✅ CartItem model for cart management
- ✅ API service with GET operations
- ✅ **Cubit implementation** for state management (ProductCubit, CartCubit, ThemeCubit)
- ✅ Cache service with SharedPreferences
- ✅ All required screens (Home, Detail, Cart, Settings)
- ✅ Search and filter functionality (local)
- ✅ Working offline mode
- ✅ Dark mode support
- ✅ Cart persistence
- ✅ Error handling throughout
- ✅ Updated README with screenshots
- ✅ Clean, well-organized code with proper Cubit structure

**Submission Format:**
```
GitHub Repository URL: https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
```

### Repository Checklist

- [ ] Project runs without errors
- [ ] All GET operations work
- [ ] API service properly handles errors
- [ ] JSON parsing works correctly
- [ ] Products display in list view
- [ ] Products display in grid view
- [ ] Pull-to-refresh works
- [ ] Search functionality works (local)
- [ ] Filter by category works (local)
- [ ] Sort options work (local)
- [ ] Add to cart works
- [ ] Cart displays items correctly
- [ ] Quantity adjustment works
- [ ] Remove from cart works
- [ ] Cart persists using SharedPreferences
- [ ] Cart calculates totals correctly
- [ ] Dark mode toggle works
- [ ] Settings persist correctly
- [ ] Offline mode shows cached data
- [ ] Product detail screen works
- [ ] **Cubit state management** implemented for all features
- [ ] ProductCubit handles product list and filtering
- [ ] CartCubit handles cart operations
- [ ] ThemeCubit handles theme switching
- [ ] All states handled (loading, success, error)
- [ ] Direct method calls used (no events)
- [ ] README.md is complete
- [ ] Screenshots are included
- [ ] Code is properly organized in cubits/ folder
- [ ] No debug prints in production code

## Evaluation Criteria

### 1. State Management with Cubit (30%)
- Correct implementation of **Cubit pattern** (not full Bloc)
- Proper separation of Cubits (ProductCubit, CartCubit, ThemeCubit)
- Clean state classes with copyWith methods
- Direct method calls (e.g., `loadProducts()`, `addToCart()`)
- BlocProvider, BlocBuilder, BlocListener usage
- State immutability (always emit new state objects)
- No event classes (Cubit uses direct methods)

### 2. API Integration & Data (25%)
- Correct implementation of GET operations
- Proper JSON parsing with model classes
- Error handling for network errors
- Timeout handling
- HTTP status code handling
- Offline mode with cached data

### 3. UI & UX (20%)
- BlocBuilder implementation
- ListView and GridView
- Pull-to-refresh functionality
- Loading states
- Error states with retry
- Smooth navigation between screens
- Search and filter UI
- Cart UI with calculations
- Dark mode implementation

### 4. Local Persistence (15%)
- Cart persistence with SharedPreferences
- Product caching
- Settings persistence (dark mode, view mode)
- Proper JSON serialization/deserialization
- Data loading on app start

### 5. Code Quality (10%)
- Clean code structure
- Proper separation of concerns (models, services, cubits, screens)
- Following Flutter and **Cubit** best practices
- Code comments where necessary
- Consistent naming conventions
- Proper disposal of resources (automatic with BlocProvider)
- Organized folder structure (cubits/ folder with subfolders)

## Common Issues and Solutions

### Issue: State Not Updating in UI

**Solution:** Ensure you're emitting new state objects in Cubit
- States must be immutable
- Use copyWith or create new instances
- Don't mutate existing state objects
- Always use `emit()` with a new state
- Use BlocBuilder correctly
- Example: `emit(state.copyWith(products: newProducts))`

### Issue: Cart Not Persisting

**Solution:** Save cart after every change
- Save to SharedPreferences after add/remove/update
- Load cart in initState or app startup
- Serialize CartItem list to JSON
- Deserialize JSON back to CartItem list

### Issue: JSON Parsing Error

**Solution:** Ensure model matches API response structure
- Check FakeStore API response format
- Use correct field names (id, title, price, description, category, image, rating)
- Handle nullable fields appropriately
- Test parsing with sample data

### Issue: Products Not Loading

**Solution:** Verify API calls and Cubit state management
- Check API service implementation
- Verify HTTP response handling
- Ensure Cubit emits correct states (ProductLoading → ProductLoaded)
- Check BlocBuilder is listening to correct Cubit (ProductCubit)
- Debug with print statements in Cubit methods
- Make sure you're calling `loadProducts()` on initial load

### Issue: Offline Mode Not Working

**Solution:** Implement proper caching
- Cache products after successful API fetch
- Load cache when API throws exception
- Check internet connectivity
- Show offline indicator
- Test with airplane mode

### Issue: Search/Filter Not Working

**Solution:** Implement local filtering correctly
- Filter on local product list, not API
- Update filtered list in state
- Emit new state with filtered products
- Handle empty search results

## Need Help?

Review this week's materials:

1. [State Management with Cubit](01-bloc-state-management.md)
2. [Networking and HTTP Requests](../week-03/01-networking.md)
3. [Local Data Persistence](../week-03/03-local-data-persistence.md)

Official documentation:
- [flutter_bloc Package](https://pub.dev/packages/flutter_bloc)
- [Cubit Documentation](https://bloclibrary.dev/#/coreconcepts?id=cubit)
- [FakeStore API](https://fakestoreapi.com/)
- [SharedPreferences](https://docs.flutter.dev/cookbook/persistence/key-value)

Debugging tips:
- Use debugPrint() in Cubit methods to see state changes
- Test API endpoints in browser or Postman first
- **Use Cubit (not full Bloc)** for all state management
- Build UI incrementally
- Test each Cubit method independently
- Start with ProductCubit, then CartCubit, then ThemeCubit

## Final Thoughts

This capstone project combines everything from the bootcamp: state management with **Cubit**, API integration, local persistence, and building a complete app. It's a real-world e-commerce pattern you'll use in professional Flutter development. Take your time to implement each feature correctly, and test thoroughly.

Remember:
- **Use Cubit for all state management** (not full Bloc with events)
- Start with data layer (models, API service)
- Build state management layer (Cubits) with direct method calls
- Then build UI screens
- Add local persistence in Cubit methods
- Test each Cubit independently
- Handle all error cases
- Keep your code organized in cubits/ folder

**Cubit Checklist:**
1. ✅ ProductCubit with `loadProducts()`, `searchProducts()`, `filterByCategory()`
2. ✅ CartCubit with `addItem()`, `removeItem()`, `updateQuantity()`, `clearCart()`
3. ✅ ThemeCubit with `toggleTheme()`, `setDarkMode()`, `setLightMode()`
4. ✅ All Cubits emit new state objects (immutable)
5. ✅ All Cubits handle errors with try-catch
6. ✅ All Cubits persist data with SharedPreferences

Good luck! This is your chance to showcase everything you've learned about Cubit state management. Build something you'll be proud to show in your portfolio! 🚀
