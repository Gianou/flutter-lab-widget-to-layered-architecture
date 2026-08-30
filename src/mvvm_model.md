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

#@todo, should instantiate service in ViewModel or inject during creation of ViewModel?

1. **Add the http package**
   - Run `flutter pub add http`
   - Verify it was added to `pubspec.yaml`

2. **Update your FilmsViewModel and FilmsView**
   - On button click in the View, a function in the ViewModel calls a function in the Service to fetch the data
   - The ViewModel holds the response, the service does not have any attributes
   - Handle errors gracefully (show an error message if the fetch fails)


![alt text](image-14.png)
