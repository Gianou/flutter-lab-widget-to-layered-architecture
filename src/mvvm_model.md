# MVVM: Model

## Learning Outcome

By the end of this chapter, you'll create a `FilmService` class to fetch data from the Ghibli API, handle API responses and errors gracefully, integrate the service with your ViewModel, and understand how data flows through the complete MVVM architecture (View → ViewModel → Service).

## Theory / Explanation

The **Model** is the **data layer** of your application. It handles all operations related to data: fetching from APIs, parsing responses, managing databases, and providing clean data to the ViewModel.

### Components of the Model Layer

The Model layer consists of two main parts:

1. **Data Classes**: Structures that represent your data (like the `Film` class you created earlier). These define what data looks like.

2. **Services**: Classes that fetch and manage data. A service handles:
   - Making API requests
   - Parsing JSON responses into data objects
   - Error handling
   - Caching (if needed)

### Fetching Data from the API

To make HTTP requests in Flutter, we use the `http` package. Add it with:
```bash
flutter pub add http
```

Making a request is simple:
```dart
final response = await http.get(Uri.parse(url));
```

Once you have the response, parse the JSON:
```dart
final films = (jsonDecode(response.body) as List)
    .map((data) => Film.fromJson(data))
    .toList();
```

Notice that `Film` needs a `fromJson()` constructor to parse JSON data into a `Film` object. This is why having a structured model class is so valuable.

## Practice

### Exercise 1: Install the HTTP Package

Add the http package.
- Run `flutter pub add http`
- Verify it was added to `pubspec.yaml`

<details>
<summary>Solution</summary>

```bash
# Run in terminal:
flutter pub add http
```

Verify in `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.0.0  # Version may vary
  provider: ^6.0.0
```

</details>

### Exercise 2: Create FilmService for API Integration

Create a FilmService class.
- This service handles all communication with the Ghibli API
- It should have a method to fetch films and parse them into Film objects
- Handle errors gracefully (network errors, parsing errors)

<details>
<summary>Solution</summary>

#### lib/services/film_service.dart
```dart
// lib/services/film_service.dart
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'package:flutter_lab_widget_to_layered_architecture/models/film_model.dart';

class FilmService {
  static const String baseUrl = 'https://ghibliapi.vercel.app';

  Future<List<Film>> fetchFilms() async {
    try {
      final response = await http.get(Uri.parse('$baseUrl/films'));
      
      if (response.statusCode == 200) {
        // TODO: Parse JSON response
        final films = (jsonDecode(response.body) as List)
            .map((data) => Film.fromJson(data as Map<String, dynamic>))
            .toList();
        return films;
      } else {
        // TODO: Handle HTTP errors
        throw Exception('Failed to load films: ${response.statusCode}');
      }
    } catch (e) {
      // TODO: Handle network and parsing errors
      rethrow;
    }
  }
}
```

</details>

### Exercise 3: Connect FilmService to ViewModel

Update your FilmsViewModel.
- Add a FilmService instance to the ViewModel
- Modify `fetchFilms()` to call the service instead of creating mock data
- Handle loading and error states
- Update the UI to show loading indicator or error message when appropriate

<details>
<summary>Solution</summary>

#### lib/view_models/films_view_model.dart (Updated)
```dart
// lib/view_models/films_view_model.dart
import 'package:flutter/foundation.dart';
import 'package:flutter_lab_widget_to_layered_architecture/models/film_model.dart';
import 'package:flutter_lab_widget_to_layered_architecture/services/film_service.dart';

class FilmsViewModel extends ChangeNotifier {
  final FilmService _filmService = FilmService();
  
  List<Film> films = [];
  bool isLoading = false;
  String? errorMessage;

  Future<void> fetchFilms() async {
    isLoading = true;
    errorMessage = null;
    notifyListeners();
    
    try {
      films = await _filmService.fetchFilms();
      errorMessage = null;
    } catch (e) {
      // TODO: Handle errors and set user-friendly error message
      errorMessage = 'Failed to load films: ${e.toString()}';
      films = [];
    } finally {
      isLoading = false;
      notifyListeners();
    }
  }
}
```

</details>

### Exercise 4: Complete the MVVM Data Flow

Connect everything together.
- On button click in the View, call `viewModel.fetchFilms()`
- The ViewModel calls `filmService.fetchFilms()`
- The service makes the API request and returns films
- The ViewModel updates its state and notifies listeners
- The Consumer rebuilds the View with the new data

<details>
<summary>Solution</summary>

#### lib/views/films/film_view.dart (Updated with loading and error states)
```dart
// lib/views/films/film_view.dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

class FilmsView extends StatelessWidget {
  const FilmsView({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Ghibli Films'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () {
              context.read<FilmsViewModel>().fetchFilms();
            },
          ),
        ],
      ),
      body: Consumer<FilmsViewModel>(
        builder: (context, viewModel, child) {
          // TODO: Show loading spinner
          if (viewModel.isLoading) {
            return const Center(child: CircularProgressIndicator());
          }

          // TODO: Show error message
          if (viewModel.errorMessage != null) {
            return Center(
              child: Text('Error: ${viewModel.errorMessage}'),
            );
          }

          // TODO: Show empty state
          if (viewModel.films.isEmpty) {
            return const Center(child: Text('No films loaded. Tap refresh!'));
          }

          // TODO: Display films grid
          return GridView.builder(
            gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
            ),
            itemCount: viewModel.films.length,
            itemBuilder: (context, index) {
              return FilmCard(film: viewModel.films[index]);
            },
          );
        },
      ),
    );
  }
}
```

**Complete Data Flow:**
1. User taps refresh button in View
2. View calls `context.read<FilmsViewModel>().fetchFilms()`
3. ViewModel sets `isLoading = true` and notifies listeners
4. View shows loading spinner
5. ViewModel calls `filmService.fetchFilms()`
6. FilmService makes HTTP request to Ghibli API
7. FilmService parses JSON response into Film objects
8. ViewModel receives films and updates state
9. ViewModel calls `notifyListeners()`
10. Consumer rebuilds with new films list

</details>
      ),
      body: Consumer<FilmsViewModel>(
        builder: (context, viewModel, _) {
          // TODO: Show loading indicator if viewModel.isLoading
          // TODO: Show error message if viewModel.errorMessage is not null
          // TODO: Show empty state if films list is empty
          // TODO: Show GridView/ListView of films otherwise
          return Container();
        },
      ),
    );
  }
}
```

</details>

## Recap

- ✓ Understood the Model layer as the data/API layer
- ✓ Learned the two components: Data Classes and Services
- ✓ Created `FilmService` to fetch data from the Ghibli API
- ✓ Implemented JSON parsing using `Film.fromJson()`
- ✓ Added error handling for graceful failure
- ✓ Integrated FilmService with FilmsViewModel
- ✓ Added loading and error states to the UI
- ✓ Completed the full MVVM architecture (View → ViewModel → Service → API)

## Conclusion

Congratulations! You've successfully implemented a complete MVVM architecture:

- **View Layer** (`FilmsView`): Displays UI and receives user interactions
- **ViewModel Layer** (`FilmsViewModel`): Manages state and orchestrates logic
- **Model Layer** (`FilmService`, `Film`): Fetches and manages data

This architecture scales well as your app grows—you can:
- Add more Views without touching the ViewModel or Service
- Modify the API source without changing the View
- Test ViewModels independently using mock services
- Reuse services across multiple ViewModels

The patterns you've learned here are foundational for professional Flutter development. From here, you could explore navigation, advanced state management, testing, and more complex features.
