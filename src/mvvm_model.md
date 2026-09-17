# MVVM: Model

## Learning Outcome

By the end of this chapter, you'll create a `FilmService` class to fetch data from the Ghibli API, handle API responses and errors gracefully, integrate the service with your ViewModel, and understand how data flows through the complete MVVM architecture (View → ViewModel → Service).

## Theory / Explanation

The **Model** is the **data layer** of your application. It handles all operations related to data: fetching from APIs, parsing responses, managing databases, and providing clean data to the ViewModel.

### Components of the Model Layer

The Model layer can be quite confusing. Unlike the View and the ViewModel, the Model is multiple elements. Mainly:

1. **Data Classes**: Structures that represent your data (like the `Film` class in this exercise). These define what data looks like.

2. **Services**: Classes used to access the data from external sources. A service handles:
   - Making API requests
   - Parsing JSON responses into data objects
   - Error handling
   - Caching (if needed)

And in some cases even more elements can compose the Model. For instance, the [Architecture Case Study](https://docs.flutter.dev/app-architecture/case-study) from Flutter's documentation also includes "Repositories" between the ViewModel and the Service. But for now, let us focus on a simple MVVM implementation.

### Fetching Data from the API

Flutter comes with many built-in capabilities, but for specialized tasks like making HTTP requests, we use **external packages**. Packages are reusable code libraries that other developers have created and published.

If you've worked with other languages, you've seen this before:
- **JavaScript/Node.js**: npm packages managed in `package.json`
- **Python**: pip packages managed in `requirements.txt`
- **Dart/Flutter**: pub packages managed in `pubspec.yaml`

To make HTTP requests in Flutter, we use the `http` package, a widely-used, well-maintained package for working with web APIs. Add it with:

```bash
flutter pub add http
```

This command downloads the package from the pub registry and adds it to your `pubspec.yaml` file. Once added, you can import it in your service files:
```dart
import 'package:http/http.dart' as http;
import 'dart:convert'; // For JSON parsing
```

### Async and Await

Network requests are **asynchronous**. In Dart, async functions are defined with the `Future` return type:

```dart
Future<List<Film>> fetchFilms() async {
  // This function will return a List<Film> in the future
}
```

Inside an async function, you can use `await` to wait for an async operation to complete:

```dart
final response = await http.get(Uri.parse(url));
// Code here runs only after the request completes
```

The `Future<T>` return type means "this function will eventually return a value of type T". For example:
- `Future<List<Film>>` means it will eventually return a list of films
- `Future<void>` means it does something but returns nothing
- `Future<String>` means it will eventually return a string

Making a request inside an async function is simple:
```dart
final response = await http.get(Uri.parse(url));
```

Always check the response status code before processing:
```dart
if (response.statusCode == 200) {
  // Success - parse the data
} else {
  // Error - handle the failure
  throw Exception('Failed to load data');
}
```

Once you have a successful response, parse the JSON:
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
  http: ^1.6.0  # Version may vary
  provider: ^6.0.0
```

</details>

### Exercise 2: Create a Service class for API Integration

- This service handles all communication with the Ghibli API
- It should have a method to fetch films 
- It should Handle errors  
  

**Steps:**
- In the existing `/lib/models` folder, create `ghibli_api_service.dart` 
- Create a class `GhibliApiService`. It does not extend anything, this class just define the function to interact with the Ghibli API.  
- Define a `getFilms()` function to fetch all available films from the [Ghibli API](https://ghibliapi.vercel.app/)

 

<details>
<summary>Solution</summary>

```dart
// lib/services/film_service.dart
import 'dart:convert';
import 'dart:io';

import 'package:ghibli_viewer_lab/models/film_model.dart';
import 'package:http/http.dart' as http;

class GhibliApiService {
  Future<List<Film>> getFilms() async {
    final uri = Uri.https('ghibliapi.vercel.app', '/films');

    final response = await http.get(uri);

    if (response.statusCode != 200) {
      throw HttpException('Failed to fetch films: HTTP ${response.statusCode}');
    }

    final List<dynamic> jsonList = jsonDecode(response.body) as List<dynamic>;

    return jsonList
        .map((jsonItem) => Film.fromJson(jsonItem as Map<String, dynamic>))
        .toList();
  }
}

```

</details>

### Exercise 3: Connect FilmService to ViewModel

Update your FilmsViewModel.
- Add a FilmService instance to the ViewModel
- Modify `fetchFilms()` to call the service instead of assigning mock data
- Handle loading and error states
- Update the UI to show loading indicator or error message when appropriate

<details>
<summary>Solution</summary>

```dart
// lib/view_models/films_view_model.dart
import 'package:flutter/material.dart';
import 'package:ghibli_viewer_lab/models/film_model.dart';
import 'package:ghibli_viewer_lab/models/ghibli_api_service.dart';

class FilmsViewModel extends ChangeNotifier {
  final GhibliApiService service = GhibliApiService();
  List<Film> films = [];
  bool isLoading = false;
  String? errorMessage;

  Future<void> fetchFilms() async {
    isLoading = true;
    errorMessage = null;
    notifyListeners();

    try {
      films = await service.getFilms();
    } catch (e) {
      errorMessage = e.toString();
    } finally {
      isLoading = false;
      notifyListeners();
    }
  }
}
```

```dart
// lib/views/films/films_view.dart
import 'package:flutter/material.dart';
import 'package:ghibli_viewer_lab/view_models/films_view_model.dart';
import 'package:ghibli_viewer_lab/views/films/widgets/film_card.dart';

class FilmsView extends StatefulWidget {
  const FilmsView({super.key});

  @override
  State<FilmsView> createState() => _FilmsViewState();
}

class _FilmsViewState extends State<FilmsView> {
  final FilmsViewModel viewModel = FilmsViewModel();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Ghibli Films"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: ListenableBuilder(
        listenable: viewModel,
        builder: (context, _) {
          // 1. Handle Error State
          if (viewModel.errorMessage != null) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(
                    viewModel.errorMessage!,
                    style: const TextStyle(color: Colors.red),
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 16),
                  ElevatedButton(
                    onPressed: () => viewModel.fetchFilms(),
                    child: const Text("Retry"),
                  ),
                ],
              ),
            );
          }

          // 2. Handle Loading State
          if (viewModel.isLoading) {
            return const Center(child: CircularProgressIndicator());
          }

          // 3. Handle Success State (List not empty)
          if (viewModel.films.isNotEmpty) {
            return ListView.builder(
              itemCount: viewModel.films.length,
              itemBuilder: (context, index) {
                return Center(
                  child: Padding(
                    padding: const EdgeInsets.symmetric(vertical: 8.0),
                    child: FilmCard(film: viewModel.films[index]),
                  ),
                );
              },
            );
          }

          // 4. Handle Idle/Empty State (Initial load)
          return Center(
            child: ElevatedButton(
              onPressed: () => viewModel.fetchFilms(),
              child: const Text('Fetch Films'),
            ),
          );
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
