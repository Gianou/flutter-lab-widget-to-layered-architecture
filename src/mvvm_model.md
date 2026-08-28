# MVVM: Model

The **Model** is the **data layer** of your application. It handles all operations related to data: fetching from APIs, parsing responses, managing databases, and providing clean data to the ViewModel.

## Components of the Model Layer

The Model layer consists of two main parts:

1. **Data Classes**: Structures that represent your data (like the `Film` class you created earlier). These define what data looks like.

2. **Services**: Classes that fetch and manage data. A service handles:
   - Making API requests
   - Parsing JSON responses into data objects
   - Error handling
   - Caching (if needed)

## Fetching Data from the API

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

1. **Add the http package**
   - Run `flutter pub add http`
   - Verify it was added to `pubspec.yaml`

2. **Create a FilmRepository**
   - Create a class `FilmRepository` with a method `fetchFilms()`
   - This method calls the Ghibli API at `https://ghibliapi.vercel.app/films`
   - Parse the JSON response into a list of `Film` objects
   - Return the list

3. **Update your Film model** (if needed)
   - Add a `Film.fromJson()` constructor that parses JSON data
   - This allows converting API responses into Film objects

4. **Update your FilmsViewModel**
   - Instead of creating sample films, create an instance of `FilmRepository`
   - In `fetchFilms()`, call `repository.fetchFilms()` and store the result
   - Handle errors gracefully (show an error message if the fetch fails)

5. **Test the flow**
   - Tap the "Fetch Films" button in your View
   - The ViewModel calls the Repository
   - The Repository fetches from the API
   - Films appear on screen

![alt text](image-14.png)
