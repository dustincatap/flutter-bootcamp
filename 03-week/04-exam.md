# Week 3 Final Project: Task Manager with API Integration

Congratulations on completing Week 3! Now it's time to put everything you've learned into practice by building a full-featured task manager app with API integration, dynamic lists, and local data persistence.

## Project Overview

You will create a Flutter application that connects to a REST API to manage tasks (todos). This project will help you demonstrate your understanding of:
- Making HTTP requests (GET, POST)
- Parsing JSON data
- Displaying data in dynamic lists (ListView/GridView)
- Using FutureBuilder for async operations
- Local data persistence with SharedPreferences
- User preferences and caching
- Error handling and loading states

## Project Requirements

### Required Features

Your Task Manager app must include the following:

1. **Home Screen (Task List)**
   - Display a list of tasks fetched from the API
   - Show task title, completion status, and user ID
   - Use ListView.builder for efficient scrolling
   - Pull-to-refresh functionality to reload tasks
   - Loading indicator while fetching data
   - Error handling with retry option
   - Floating action button to add new task
   - Filter tasks by completion status (All/Completed/Pending)

2. **Task Detail Screen**
   - Show full task details when tapped
   - Display task ID, user ID, title, and completion status
   - Back button to return to home screen

3. **Add Task Screen**
   - Form to create a new task
   - Title field (required, minimum 3 characters)
   - User ID field (required, must be a number)
   - Completion status toggle
   - Save button to submit to API
   - Cancel button to go back
   - Form validation

4. **User Preferences**
   - Settings screen accessible from home screen
   - Dark mode toggle (persist preference)
   - **Note:** You may add other user preferences like view mode (List/Grid), default filter, etc.

5. **Local Data Persistence**
   - Cache API responses using SharedPreferences
   - Save user preferences (view mode, theme, default filter)
   - Show cached data when offline (no internet connection)
   - Display indicator when showing cached data

### API Endpoints to Use

Use JSONPlaceholder API: `https://jsonplaceholder.typicode.com/`

**Required Endpoints:**

1. **Get all tasks (GET)**
   ```
   GET https://jsonplaceholder.typicode.com/todos
   ```

2. **Get single task (GET)**
   ```
   GET https://jsonplaceholder.typicode.com/todos/{id}
   ```

3. **Create task (POST)**
   ```
   POST https://jsonplaceholder.typicode.com/todos
   Body: {
     "userId": 1,
     "title": "New task",
     "completed": false
   }
   ```

**Note:** JSONPlaceholder is a fake API, so POST won't actually persist data on the server, but will return a successful response that you can use to update your local UI.

### Technical Requirements

1. **Project Setup**
   - Create a new Flutter project named `task_manager` or similar
   - Add required dependencies:
     - `http: ^1.1.0`
     - `shared_preferences: ^2.2.2`
   - Use proper project structure with separate folders

2. **HTTP Requests**
   - Create an API service class to handle all HTTP requests
   - Implement GET and POST methods
   - Add proper error handling for network errors
   - Use try-catch blocks for exception handling
   - Set timeouts for requests (10 seconds)

3. **JSON Parsing**
   - Create a Task model class with fromJson and toJson methods
   - Parse API responses correctly
   - Handle null values appropriately

4. **UI Components**
   - Use FutureBuilder to display async data
   - Implement ListView.builder for task list
   - Implement GridView.builder for grid view option
   - Use RefreshIndicator for pull-to-refresh
   - Show CircularProgressIndicator for loading states
   - Display error messages with retry button

5. **State Management**
   - Use StatefulWidget for screens with dynamic data
   - Update UI after CRUD operations
   - Manage loading, success, and error states
   - Properly dispose of controllers and resources

6. **Local Storage**
   - Cache task list when successfully fetched from API
   - Save dark mode preference
   - Load cached tasks when offline

7. **Code Organization**
   ```
   lib/
   ├── main.dart
   ├── models/
   │   └── task.dart
   ├── services/
   │   ├── api_service.dart
   │   └── cache_service.dart
   ├── screens/
   │   ├── home_screen.dart
   │   ├── task_detail_screen.dart
   │   ├── task_form_screen.dart
   │   └── settings_screen.dart
   └── widgets/
       └── task_card.dart (optional)
   ```

## Getting Started

### Step 1: Create the Project

```bash
flutter create task_manager
cd task_manager
```

### Step 2: Add Dependencies

Update `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0
  shared_preferences: ^2.2.2
```

Run:
```bash
flutter pub get
```

## Tips and Best Practices

### 1. Error Handling

- Always wrap HTTP requests in try-catch blocks
- Check response status codes before processing
- Throw meaningful exceptions with error details
- Display user-friendly error messages in the UI

### 2. Loading States

- Use FutureBuilder for async operations
- Show CircularProgressIndicator while loading
- Handle ConnectionState.waiting appropriately
- Display error states with retry options

### 3. Cache Management

- Cache tasks immediately after successful API fetch
- Use cache as fallback when API fails (offline mode)
- Always try API first, then fall back to cache
- Parse JSON correctly when saving/loading from cache

### 4. Optimistic UI Updates

- Update UI immediately when user performs an action
- Sync with API in the background
- Revert changes if API call fails
- Show feedback to user (SnackBar for success/failure)

### 5. Code Organization

Keep related code together:
- All API calls in `ApiService`
- All caching logic in `CacheService`
- Separate screens in `screens/` folder
- Reusable widgets in `widgets/` folder

## Bonus Challenges (Optional)

1. **Advanced Features**
   - Add search functionality to filter tasks by title
   - Implement pagination (load 20 tasks at a time)
   - Add user filter (show tasks for specific user ID)
   - Swipe to delete gesture on list items

2. **Better UX**
   - Add animations for screen transitions
   - Implement shimmer loading effect
   - Add success/error snackbars
   - Add confirmation dialogs before delete

3. **Offline Support**
   - Queue offline actions (create/update/delete)
   - Sync queued actions when back online
   - Show offline indicator in app bar

4. **Data Visualization**
   - Show statistics (total tasks, completed %, by user)
   - Add charts/graphs for task completion
   - Show most active users

## Submission Guidelines

### Step 1: Prepare Your Code

1. **Test thoroughly**
   - Test all CRUD operations
   - Test offline mode (turn off internet)
   - Test cache expiration
   - Test all user preferences
   - Verify error handling works
   - Test pull-to-refresh
   - Test list and grid views

2. **Clean up**
   - Remove debug print statements
   - Remove commented code
   - Format code properly
   - Add necessary comments

### Step 2: Create a README

Create a comprehensive `README.md`:

- Project title and description
- List of features implemented
- API endpoints used
- Technologies and packages used
- Project structure diagram
- How to run the project
- Screenshots of the app
- Checklist of implemented features (CRUD operations, UI features, persistence)
- (Optional) What you learned and challenges faced

### Step 3: Take Screenshots

Capture screenshots showing:
1. Home screen with task list (list view)
2. Home screen with grid view
3. Task detail screen
4. Add new task form
5. Settings screen
6. Offline mode indicator
7. Error state with retry button
8. Different filter states (All/Completed/Pending)

### Step 4: Create GitHub Repository

Initialize git in your project:
```bash
git init
git add .
git commit -m "Initial commit: Task Manager with API integration"
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
- ✅ Task model with JSON parsing
- ✅ API service with GET and POST operations
- ✅ Cache service with expiration
- ✅ All four screens (Home, Detail, Form, Settings)
- ✅ Working offline mode
- ✅ User preferences persistence
- ✅ Error handling throughout
- ✅ Updated README with screenshots
- ✅ Clean, well-organized code

**Submission Format:**
```
GitHub Repository URL: https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
```

### Repository Checklist

- [ ] Project runs without errors
- [ ] All GET and POST operations work
- [ ] API service properly handles errors
- [ ] JSON parsing works correctly
- [ ] Tasks display in list view
- [ ] Tasks display in grid view
- [ ] Pull-to-refresh works
- [ ] Filters work (All/Completed/Pending)
- [ ] Add task creates and displays new task
- [ ] Cache saves and loads tasks
- [ ] Offline mode shows cached data
- [ ] User preferences save and load
- [ ] Settings screen works
- [ ] README.md is complete
- [ ] Screenshots are included
- [ ] Code is properly organized
- [ ] No debug prints in production code

## Evaluation Criteria

### 1. API Integration (35%)
- Correct implementation of GET and POST
- Proper JSON parsing with model class
- Error handling for network errors
- Timeout handling
- HTTP status code handling

### 2. UI & UX (25%)
- FutureBuilder implementation
- ListView and GridView
- Pull-to-refresh functionality
- Loading states
- Error states with retry
- Smooth navigation between screens
- Filter functionality

### 3. Local Persistence (20%)
- Cache implementation with SharedPreferences
- Dark mode preference saving/loading
- Offline mode support

### 4. Code Quality (15%)
- Clean code structure
- Proper separation of concerns (models, services, screens)
- Proper disposal of resources
- Following Flutter best practices
- Code comments where necessary
- Consistent naming conventions

### 5. Features Completeness (5%)
- All required features implemented
- App runs without crashes
- Proper form validation
- Settings screen functional
- README and screenshots

## Common Issues and Solutions

### Issue: Network Error - No Internet

**Solution:** Implement offline mode with cached data
- Try API first
- If API fails, load from cache
- Show offline indicator when displaying cached data

### Issue: JSON Parsing Error

**Solution:** Ensure model matches API response structure
- Check field names match exactly
- Use correct data types (int, String, bool)
- Handle nullable fields appropriately

### Issue: Tasks Not Updating After Adding New Task

**Solution:** Refresh the task list after operations
- Return a success value from form screen
- Check for success result in home screen
- Call refresh method to reload tasks

### Issue: Cache Not Loading When Offline

**Solution:** Verify cache implementation
- Ensure tasks are saved after successful API fetch
- Check JSON encoding/decoding is correct
- Verify cache is loaded when API throws exception

### Issue: FutureBuilder Rebuilding Too Often

**Solution:** Store Future in state variable
- Create Future in initState, not in build method
- Store in a late variable
- Only recreate when explicitly refreshing

## Need Help?

Review this week's materials:

1. [Networking and HTTP Requests](01-networking.md)
2. [Dynamic Lists with FutureBuilder](02-dynamic-lists.md)
3. [Local Data Persistence](03-local-data-persistence.md)

Official Flutter documentation:
- [Networking](https://docs.flutter.dev/cookbook/networking/fetch-data)
- [JSON Parsing](https://docs.flutter.dev/development/data-and-backend/json)
- [Lists](https://docs.flutter.dev/cookbook/lists/long-lists)
- [SharedPreferences](https://docs.flutter.dev/cookbook/persistence/key-value)

Debugging tips:
- Use debugPrint() to debug API responses
- Check the debug console for error messages
- Test API endpoints in Postman first
- Start simple, then add features incrementally

## Final Thoughts

This project combines everything from Week 3: API integration, dynamic lists, and local persistence. It's a real-world application pattern you'll use in many Flutter apps. Take your time to implement each feature correctly, and test thoroughly.

Remember:
- Start with the API service and test it first
- Build the UI incrementally
- Add caching after basic functionality works
- Test offline mode thoroughly
- Handle all error cases
- Keep your code organized

Good luck! 🚀
