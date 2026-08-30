# Stateless Widgets
Now that we know how to use the widgets provided by Flutter, let's see how to create our own widgets. We will start with **stateless** widgets.

A **StatelessWidget** is a Dart class that extends `StatelessWidget`.  

The following examples are taken from [Flutter's official documentation](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html#widgets.StatelessWidget.1)

## Simple StatelessWidget (No Parameters)

This is the most basic a widget can be:
- It is a Dart class
- It extends StatelessWidget
- It has a constructor
- It overrides the inherited `build()` method, that returns a `Widget`

```dart
class GreenFrog extends StatelessWidget {
  const GreenFrog({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(color: const Color(0xFF2DBD3A));
  }
}
```

## StatelessWidget with Parameters

This is an example of a widget that takes parameters. 


```dart
class Frog extends StatelessWidget {
  const Frog({
    super.key,
    this.color = const Color(0xFF2DBD3A),
    this.child,
  });

  final Color color;
  final Widget? child;

  @override
  Widget build(BuildContext context) {
    return ColoredBox(color: color, child: child);
  }
}
```

### Usage

Once defined, you can use the `Frog` widget in different ways:

```dart
// Using default color (green)
const Frog()

// With a custom color
const Frog(color: Color(0xFFFF0000))

// With custom color and a child widget
const Frog(
  color: Color(0xFFFFFF00),
  child: Text("Yellow Frog"),
)
```

## Convention

By convention, widget constructors only use **named arguments** (in curly braces `{}`):
- **`super.key`** is always first
- **`child`** or **`children`** is always last (if present)
- Parameters are stored as `final` properties

## Practice
Let's now practice by extracting the UI logic we have created to display film titles.  
This step should result in a much cleaner main.dart:
```dart
...

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const FilmTitle(title: "Castle in The Sky"),
              const FilmTitle(title: "Kiki's Delivery Service"),
            ],
          ),
        ),
      ),
    );
  }
}
```

1. **Create a FilmTitle widget** to remove the duplicate code of `Container(Text())`:
   - Create a new folder in `lib` named `views`. In this folder, add a `film_title.dart` file
   - Create a class `FilmTitle()` that extends `StatelessWidget`
   - Add a `final String filmTitle` property
   - In the `build()` method, return the previously defined `Container(Text())`
   - Update the rest of the project to use this new widget to display film titles
