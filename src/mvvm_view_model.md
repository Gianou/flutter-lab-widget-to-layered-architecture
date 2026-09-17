# MVVM: ViewModel

## Learning Outcome

By the end of this chapter, you'll create a separate `ViewModel` class that manages state, notify listeners when data changes, and connect your View to the ViewModel using `ListenableBuilder`.

## Theory / Explanation

The **ViewModel** is a separate class (not a widget) that:
- Holds the state (data) the View needs
- Provides methods to update that state
- Notifies listeners when state changes using [`ChangeNotifier`](https://api.flutter.dev/flutter/foundation/ChangeNotifier-class.html)

The View listens to the ViewModel using `ListenableBuilder`. When the ViewModel calls `notifyListeners()`, the View rebuilds automatically.

This separation keeps View logic (UI) separate from state management logic (ViewModel).

## Practice

### Exercise 1: Create FilmsViewModel
This ViewModel is responsible for holding the state that contains all the films data. In the next chapter, we will implement the fetch of data via the REST API. For now, the list of films should be an empty array. On call of a function `fetchFilms()` the mock data we used previously is assigned to the ViewModel state. We use this extra step to demonstrate how to trigger UI update from the ViewModel.

Create a new file `/lib/view_models/films_view_model.dart` with a `FilmsViewModel` class that:
- Extends `ChangeNotifier`
- Has a `films` list property (initially empty)
- Has a `fetchFilms()` method that populates the list with mock Film data
- Calls `notifyListeners()` after updating the list

**Steps:**
- Create the `/lib/view_models/` folder
- Create `films_view_model.dart` and define the `FilmsViewModel` class that extends [`ChangeNotifier`](https://api.flutter.dev/flutter/foundation/ChangeNotifier-class.html)
- Define mock Film objects (you can copy them from the previous chapter)
- The `fetchFilms()` method should assign the mock films to the `films` list and call `notifyListeners()`


<details>
<summary>Solution</summary>

```dart
// /lib/view_models/films_view_model.dart
import 'package:flutter/material.dart';
import 'package:ghibli_viewer_lab/models/film_model.dart';

class FilmsViewModel extends ChangeNotifier {
  List<Film> films = [];

  void fetchFilms() {
    films = [
      Film(
        id: 'ea660b10-85c4-4ae3-8a5f-41cea3648e3e',
        title: "Kiki's Delivery Service",
        originalTitle: '魔女の宅急便',
        originalTitleRomanised: 'Majo no takkyūbin',
        image: 'https://image.tmdb.org/t/p/w600_and_h900_bestv2/7nO5DUMnGUuXrA4r2h6ESOKQRrx.jpg',
        movieBanner: 'https://image.tmdb.org/t/p/original/h5pAEVma835u8xoE60kmLVopLct.jpg',
        description: 'A young witch, on her mandatory year of independent life, finds fitting into a new community difficult while she supports herself by running an air courier service.',
        director: 'Hayao Miyazaki',
        producer: 'Hayao Miyazaki',
        releaseDate: '1989',
        runningTime: '102',
        rtScore: '96',
        people: [
          'https://ghibliapi.vercel.app/people/2409052a-9029-4e8d-bfaf-70fd82c8e48d',
          'https://ghibliapi.vercel.app/people/7151abc6-1a9e-4e6a-9711-ddb50ea572ec',
        ],
        species: [
          'https://ghibliapi.vercel.app/species/af3910a6-429f-4c74-9ad5-dfe1c4aa04f2',
        ],
        locations: ['https://ghibliapi.vercel.app/locations/'],
        vehicles: ['https://ghibliapi.vercel.app/vehicles/'],
        url: 'https://ghibliapi.vercel.app/films/ea660b10-85c4-4ae3-8a5f-41cea3648e3e',
      ),
      Film(
        id: '2baf70d1-42bb-4437-b551-e5fed5a87abe',
        title: 'Castle in the Sky',
        originalTitle: '天空の城ラピュタ',
        originalTitleRomanised: 'Tenkū no shiro Rapyuta',
        image: 'https://image.tmdb.org/t/p/w600_and_h900_bestv2/npOnzAbLh6VOIu3naU5QaEcTepo.jpg',
        movieBanner: 'https://image.tmdb.org/t/p/w533_and_h300_bestv2/3cyjYtLWCBE1uvWINHFsFnE8LUK.jpg',
        description: 'The orphan Sheeta inherited a mysterious crystal that links her to the mythical sky-kingdom of Laputa. With the help of resourceful Pazu and a rollicking band of sky pirates, she makes her way to the ruins of the once-great civilization. Sheeta and Pazu must outwit the evil Muska, who plans to use Laputa\'s science to make himself ruler of the world.',
        director: 'Hayao Miyazaki',
        producer: 'Isao Takahata',
        releaseDate: '1986',
        runningTime: '124',
        rtScore: '95',
        people: [
          'https://ghibliapi.vercel.app/people/598f7048-74ff-41e0-92ef-87dc1ad980a9',
          'https://ghibliapi.vercel.app/people/fe93adf2-2f3a-4ec4-9f68-5422f1b87c01',
          'https://ghibliapi.vercel.app/people/3bc0b41e-3569-4d20-ae73-2da329bf0786',
          'https://ghibliapi.vercel.app/people/40c005ce-3725-4f15-8409-3e1b1b14b583',
          'https://ghibliapi.vercel.app/people/5c83c12a-62d5-4e92-8672-33ac76ae1fa0',
          'https://ghibliapi.vercel.app/people/e08880d0-6938-44f3-b179-81947e7873fc',
          'https://ghibliapi.vercel.app/people/2a1dad70-802a-459d-8cc2-4ebd8821248b',
        ],
        species: [
          'https://ghibliapi.vercel.app/species/af3910a6-429f-4c74-9ad5-dfe1c4aa04f2',
        ],
        locations: ['https://ghibliapi.vercel.app/locations/'],
        vehicles: [
          'https://ghibliapi.vercel.app/vehicles/4e09b023-f650-4747-9ab9-eacf14540cfb',
        ],
        url: 'https://ghibliapi.vercel.app/films/2baf70d1-42bb-4437-b551-e5fed5a87abe',
      ),
    ];
    notifyListeners();
  }
}
```

</details>

### Exercise 2: Connect FilmsView to FilmsViewModel Using ListenableBuilder

Update `FilmsView` to use the `FilmsViewModel` and rebuild when the ViewModel notifies listeners.
![alt text](viewmodel.gif)
**Steps:**
- Convert `FilmsView` to a `StatefulWidget`
- In `_FilmsViewState`, create an instance of `FilmsViewModel`
- Wrap the body content of `_FilmsViewState` with [`ListenableBuilder`](https://api.flutter.dev/flutter/widgets/ListenableBuilder-class.html) to listen to the ViewModel
- Inside the builder, check if `viewModel.films.isEmpty` and show a button to fetch data if it is
- When the button is clicked, call `viewModel.fetchFilms()`
- If films are available, display them in a ListView with FilmCard widgets
- You can now remove the `mockFilms` from the `main.dart` and remove the `films` property from the View, since it is stored in the ViewModel now.

<details>
<summary>Hints</summary>

Use `ListenableBuilder` to listen to the ViewModel:

```dart
ListenableBuilder(
  listenable: viewModel,
  builder: (context, _) {
    return ListView.builder(
      itemCount: viewModel.films.length,
      itemBuilder: (context, index) {
        // Display each film
      },
    );
  },
)
```  
</details>  

<details>
<summary>Solution</summary>

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
          if (viewModel.films.isEmpty) {
            return Center(
              child: ElevatedButton(
                onPressed: () => viewModel.fetchFilms(),
                child: const Text('Fetch Films'),
              ),
            );
          }

          return ListView.builder(
            itemCount: viewModel.films.length,
            itemBuilder: (context, index) {
              return Center(
                child: Padding(
                  padding: const EdgeInsets.symmetric(vertical: 16),
                  child: FilmCard(film: viewModel.films[index]),
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

```dart
// /lib/main.dart

// lib/main.dart
import 'package:flutter/material.dart';
import 'package:ghibli_viewer_lab/views/films/films_view.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(body: Center(child: FilmsView())),
    );
  }
}
```   

</details>




## Recap

- ✓ Created a separate `FilmsViewModel` class that extends `ChangeNotifier`
- ✓ Defined mock films in the ViewModel
- ✓ Implemented `fetchFilms()` to update the films list
- ✓ Called `notifyListeners()` to trigger UI rebuilds
- ✓ Connected the View to ViewModel using `ListenableBuilder`

## Next Steps

Now that your ViewModel can manage films and notify the View, the final chapter introduces the **Model layer**. This is where you'll connect to the real Ghibli API, fetch actual film data, handle errors, and complete the MVVM pattern.  
