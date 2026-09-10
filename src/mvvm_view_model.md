# MVVM: ViewModel

## Learning Outcome

By the end of this chapter, you'll create a `FilmsViewModel` that extends `ChangeNotifier`, manage application state using `setState()` and `notifyListeners()`, integrate the Provider package for dependency injection, and connect your FilmsView to the ViewModel using the `Consumer` widget.

## Theory / Explanation

The **ViewModel** is the bridge between your View and your data. It holds the state (data) that your View needs to display. 

If a stateful widget has a state, a View has a ViewModel that holds all its states and for all the View's widget to access.


## What Does Logic Mean?

The ViewModel holds **state** and manages **user interactions**:
- **State**: Data the View needs (a list of films, loading status, error messages)
- **User Interactions**: Methods that respond to user actions (like "fetchFilms" button taps)


## ChangeNotifier: Managing State Changes

Flutter provides a class called `ChangeNotifier` that makes it easy to notify widgets when state changes:
- Your ViewModel extends `ChangeNotifier`
- When state changes, call `notifyListeners()` to alert all watching widgets
- Watching widgets rebuild automatically with the new state



## The Provider Package

**Provider** is a popular package for dependency injection and state management. It makes it easy to:
- Create instances of your ViewModels
- Share them across your widget tree
- Have widgets listen to ViewModel state changes

Provider is not part of the base Flutter framework, therefore it must be added as a dependency in your project.  
The command `flutter pub add provider`, can be used to this effect.  
To use a package in your code, it must be imported:  
```dart
import 'package:provider/provider.dart';
```

## Using Consumer to Listen to State

The `Consumer` widget rebuilds whenever the ViewModel notifies listeners:
- Wrap your UI with a `Consumer<YourViewModel>`
- Access the ViewModel instance inside the Consumer
- When the ViewModel's state changes, only the Consumer rebuilds (efficient!)


## Practice

### Exercise 1: Create FilmsViewModel with ChangeNotifier

Create a FilmsViewModel that extends `ChangeNotifier`.
- Add a list property to store films: `List<Film> films = []`
- Add a method `fetchFilms()` that:
  - For now, just creates some sample Film objects and stores them in the `films` list
  - Calls `notifyListeners()` at the end to alert watching widgets

<details>
<summary>Solution</summary>

#### lib/view_models/films_view_model.dart
```dart
// lib/view_models/films_view_model.dart
import 'package:flutter/foundation.dart';
import 'package:flutter_lab_widget_to_layered_architecture/models/film_model.dart';

class FilmsViewModel extends ChangeNotifier {
  List<Film> films = [];

  void fetchFilms() {
    // TODO: Create mock Film objects and add to films list
    films = [
      // TODO: Add Film instances here
    ];
    notifyListeners();
  }
}
```

</details>

### Exercise 2: Install the Provider Package

Add the provider dependency.
- Run `flutter pub add provider` in your terminal
- Verify it was added to `pubspec.yaml`

<details>
<summary>Solution</summary>

```bash
# Run in terminal:
flutter pub add provider
```

After running, verify in `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.0  # Version may vary
```

</details>

### Exercise 3: Integrate Provider in FilmsView

Update your FilmsView to use Provider.
- Wrap your film list with `Consumer<FilmsViewModel>()`
- Inside the Consumer, access the ViewModel and display `viewModel.films`
- If the films list is empty, show a message like "No films yet"

<details>
<summary>Solution</summary>

#### lib/views/films/film_view.dart (Updated)
```dart
// lib/views/films/film_view.dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:flutter_lab_widget_to_layered_architecture/view_models/films_view_model.dart';

class FilmsView extends StatelessWidget {
  const FilmsView({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Ghibli Films'),
      ),
      body: Consumer<FilmsViewModel>(
        builder: (context, viewModel, child) {
          if (viewModel.films.isEmpty) {
            return const Center(child: Text('No films yet'));
          }
          
          return GridView.builder(
            gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
            ),
            itemCount: viewModel.films.length,
            itemBuilder: (context, index) {
              // TODO: Return FilmCard widget
              return Container();
            },
          );
        },
      ),
    );
  }
}
```

</details>

### Exercise 4: Add Fetch Films Button

Add a "Fetch Films" button.
- Add a button in the app bar or body
- When tapped, call `viewModel.fetchFilms()`
- The Consumer rebuilds automatically with the new films

<details>
<summary>Solution</summary>

#### lib/views/films/film_view.dart (with button)
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
          if (viewModel.films.isEmpty) {
            return const Center(child: Text('No films yet'));
          }
          
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

#### lib/main.dart (Updated with MultiProvider)
```dart
// lib/main.dart
import 'package:provider/provider.dart';
import 'package:flutter_lab_widget_to_layered_architecture/view_models/films_view_model.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => FilmsViewModel()),
      ],
      child: MaterialApp(
        home: FilmsView(),
      ),
    );
  }
}
```

</details>

## Recap

- ✓ Understood the ViewModel's role as bridge between View and data
- ✓ Learned how `ChangeNotifier` and `notifyListeners()` manage state changes
- ✓ Added the Provider package for dependency injection
- ✓ Created `FilmsViewModel` with a `fetchFilms()` method
- ✓ Used `Consumer<FilmsViewModel>()` to connect View to ViewModel
- ✓ Implemented a "Fetch Films" button that triggers state updates

## Next Steps

Now that your ViewModel can manage films and notify the View, the final chapter introduces the **Model layer**. This is where you'll connect to the real Ghibli API, fetch actual film data, handle errors, and complete the MVVM pattern.  
