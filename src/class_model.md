# Class Model
#@todo, in this chapter we'll create a `Film()` class to represent the future api response we'll get, and add a new widget `FilmDetails()`

So far, we've been hardcoding our data as simple strings for movie titles that are passed to the `FilmTitle()` widget. But we are eventually going to use the Studio Ghibli API at `https://ghibliapi.vercel.app/` that returns much more information about each film. To use this data in our code, we need to define a **class** so that we can create instances of Films and work with them in Dart.

Here is a simplified example of what the API returns for a single film:

```json
{
  "id": "ea660b10-85c4-4ae3-8a5f-41cea3648e3e",
  "title": "Kiki's Delivery Service",
  "original_title": "魔女の宅急便",
  "original_title_romanised": "Majo no takkyūbin",
  "image": "https://image.tmdb.org/t/p/w600_and_h900_bestv2/7nO5DUMnGUuXrA4r2h6ESOKQRrx.jpg",
  "movie_banner": "https://image.tmdb.org/t/p/original/h5pAEVma835u8xoE60kmLVopLct.jpg",
  "description": "A young witch, on her mandatory year of independent life, finds fitting into a new community difficult while she supports herself by running an air courier service.",
  "director": "Hayao Miyazaki",
  "producer": "Hayao Miyazaki",
  "release_date": "1989",
  "running_time": "102",
  "rt_score": "96",
  "url": "https://ghibliapi.vercel.app/films/ea660b10-85c4-4ae3-8a5f-41cea3648e3e",
  ...
}
```


## Dart Classes for Type Safety

Dart is a **strongly typed language**. This means we can't just treat API responses as generic objects—we need to define the exact structure of our data.

We create a **Film class** that represents the structure of this data:
- Each property (title, description, director, etc.) has a defined type (String, int, etc.)
- When we create a `Film` object from API data, Dart validates that the data matches our class structure
- This gives us **type safety** to catch errors early instead of at runtime



## Practice

1. **Create a Film model class** with properties for the key film data:
  - Create the `lib/models` folder in your project, and in this folder, create a file named `film_model.dart`
  - Paste the code below for the model
    ``` dart
    class Film {
        final String id;
        final String title;
        final String originalTitle;
        final String originalTitleRomanised;
        final String image;
        final String movieBanner;
        final String description;
        final String director;
        final String producer;
        final String releaseDate;
        final String runningTime;
        final String rtScore;
        final List<String> people;
        final List<String> species;
        final List<String> locations;
        final List<String> vehicles;
        final String url;

        const Film({
          required this.id,
          required this.title,
          required this.originalTitle,
          required this.originalTitleRomanised,
          required this.image,
          required this.movieBanner,
          required this.description,
          required this.director,
          required this.producer,
          required this.releaseDate,
          required this.runningTime,
          required this.rtScore,
          required this.people,
          required this.species,
          required this.locations,
          required this.vehicles,
          required this.url,
        });

        factory Film.fromJson(Map<String, dynamic> json) {
          return Film(
            id: json['id'] as String,
            title: json['title'] as String,
            originalTitle: json['original_title'] as String,
            originalTitleRomanised: json['original_title_romanised'] as String,
            image: json['image'] as String,
            movieBanner: json['movie_banner'] as String,
            description: json['description'] as String,
            director: json['director'] as String,
            producer: json['producer'] as String,
            releaseDate: json['release_date'] as String,
            runningTime: json['running_time'] as String,
            rtScore: json['rt_score'] as String,
            people:
                (json['people'] as List<dynamic>?)
                    ?.map((e) => e as String)
                    .toList() ??
                [],
            species:
                (json['species'] as List<dynamic>?)
                    ?.map((e) => e as String)
                    .toList() ??
                [],
            locations:
                (json['locations'] as List<dynamic>?)
                    ?.map((e) => e as String)
                    .toList() ??
                [],
            vehicles:
                (json['vehicles'] as List<dynamic>?)
                    ?.map((e) => e as String)
                    .toList() ??
                [],
            url: json['url'] as String,
          );
        }
      }
    ``` 
  - Notice the content of the Film Class:
 
    - All attribute that can be found in the ghibli api response, strongly typed
    - A constructor
    - A factory method that returns a Film instance from a json input
2. Instantiate a film object in the main.dart file
   It is not best practice to place an application's data in the main, this will be corrected in later chapters.
    ```dart
      static const mockMovie = Film(
        id: 'ea660b10-85c4-4ae3-8a5f-41cea3648e3e',
        title: "Kiki's Delivery Service",
        originalTitle: '魔女の宅急便',
        originalTitleRomanised: 'Majo no takkyūbin',
        image: 'https://image.tmdb.org/t/p/w600_and_h900_bestv2/7nO5DUMnGUuXrA4r2h6ESOKQRrx.jpg',
        movieBanner:
            'https://image.tmdb.org/t/p/original/h5pAEVma835u8xoE60kmLVopLct.jpg',
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
      );
    ```
3. **Update your FilmTitle() widget** to accept a Film object instead of just a String title
   - Notice how auto-complete now shows you all available properties on the Film object
4. Create a new widget named FilmDetails()
   - Try to replicate this result on your own
