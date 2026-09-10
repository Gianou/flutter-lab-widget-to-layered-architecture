# Stateful Widgets

## Learning Outcome

By the end of this chapter, you'll understand how to create stateful widgets, manage ephemeral state with `setState()`, and build an interactive `FilmCard` widget that toggles between showing film title/image and detailed information when tapped.

## Theory / Explanation

So far, all our widgets have been `StatelessWidget` because they don't need to change after they're created. But interactive applications need widgets that can change their appearance based on user actions. That's where `StatefulWidget` comes in.

## What is State?

**State** is data that can change during the lifetime of a widget. Examples include:
- Whether a button has been pressed
- Whether a menu is open or closed
- Whether a film's details are showing or hidden  


**React** has `useState()`, **Vue** usually uses `Ref()`, **Angular** has `signals()` and **Flutter** uses `StatefulWidget`

When state changes, the widget rebuilds to reflect the new state on screen. This automatic re-rendering of the UI is the concept of **declarative programming**.

## Ephemeral State vs App State

Not all state is the same:
- **Ephemeral state** (or UI state) affects only one widget and is temporary. Examples: whether a dropdown is expanded, whether a film's details are showing. This state doesn't need to be saved or shared globally across the application.
- **App state** affects the entire application, or a large portion of the application, and persists. We'll cover this later with MVVM.

For now, we're focusing on **ephemeral state**, using `StatefulWidget` to manage local UI changes.

## Creating a StatefulWidget

A `StatefulWidget` is actually two classes:
1. The widget class itself (extends `StatefulWidget`)
2. A state class (extends `State<WidgetName>`)

The state class contains:
- The mutable data (properties that can change)
- The `build()` method that describes the UI
- Methods to modify the state (using `setState()`)

## Managing State with setState()

When you need to change state and update the UI, you call `setState()`:
- Pass a **function** that updates your state variables
- Flutter rebuilds the widget automatically
- The screen displays the new state

## Example: A Simple Counter

Here's a basic `StatefulWidget` that increments a counter when a button is tapped:

```dart
class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int count = 0;

  void increment() {
    setState(() {
      count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Text('Count: $count'),
        ElevatedButton(
          onPressed: increment, 
          child: const Text('Increment')
        ),
      ],
    );
  }
}
```

Notice:
- The widget class (`Counter`) is simple and just creates the state
- The state class (`_CounterState`) holds the mutable `count` variable
- When the ElevatedButton is pressed, the function `increment()` is called. Note that `onPressed: increment,` does not use parentheses at the end of increment. `onPressed` expects a reference to a function.
- Note the syntax of `setState()` call inside of `increment()`. `setState()` expects a function as parameter. 
  ![alt text](image-17.png)
- Here we are using the [anonymous function](https://dart.dev/language/functions#anonymous-functions) syntax from Dart, which is very similar to arrow functions from JavaScript:
  ```dart
    setState(() {
      count++;
    });
  ```

`int count = 0;` looks like a normal variable, but since it is defined in `class _CounterState extends State<Counter>` it is actually a state.  
This count state should only be modified inside a setState() call; otherwise, the value will change in memory, but the UI will not update to reflect it.


## Practice

Create a `FilmCard` widget that combines `FilmTitle()` and `FilmDetails()`:

### Exercise 1: Create FilmCard as a StatefulWidget

Make FilmCard a StatefulWidget that accepts a `Film` object.
- Add a boolean state variable `showDetails` (initially false)
- This tracks whether we're showing the title or details view

<details>
<summary>Solution</summary>

```dart
// lib/views/films/widgets/film_card.dart
class FilmCard extends StatefulWidget {
  final Film film;
  
  const FilmCard({
    super.key,
    required this.film,
  });

  @override
  State<FilmCard> createState() => _FilmCardState();
}

class _FilmCardState extends State<FilmCard> {
  bool showDetails = false;

  @override
  Widget build(BuildContext context) {
    // TODO: Implement the build method (see Solution 2)
    return Container();
  }
}
```

</details>

### Exercise 2: Implement Tap-to-Toggle Interaction

Add tap interaction using `GestureDetector`.
- Wrap your widget with `GestureDetector` and provide an `onTap` callback (so the whole card acts like a button)
- In the callback, use `setState()` to toggle `showDetails`
- Each tap switches between showing the title and the details

<details>
<summary>Solution</summary>

```dart
// lib/views/films/widgets/film_card.dart
class _FilmCardState extends State<FilmCard> {
  bool showDetails = false;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          showDetails = !showDetails;
        });
      },
      // TODO: Add child widget (see Solution 3)
      child: Container(),
    );
  }
}
```

</details>

### Exercise 3: Conditional Rendering Based on State

Conditionally display different views.
- If `showDetails` is false, show `FilmTitle(film: film)`
- If `showDetails` is true, show `FilmDetails(film: film)`

<details>
<summary>Solution</summary>

```dart
// lib/views/films/widgets/film_card.dart
class _FilmCardState extends State<FilmCard> {
  bool showDetails = false;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          showDetails = !showDetails;
        });
      },
      child: showDetails
          ? FilmDetails(film: widget.film)
          : FilmTitle(film: widget.film),
    );
  }
}
```

</details>

### Exercise 4: Add Sizing Constraints

Elevate the container if any display logic is duplicated between FilmTitle and FilmDetails.
The new FilmCard can also include logic to enforce a specific size to its children.

<details>
<summary>Solution</summary>

```dart
// lib/views/films/widgets/film_card.dart
// TODO: Add sizing constraints (width/height) to maintain consistent card size
// TODO: Consider wrapping child in a Container with fixed dimensions
// TODO: Optionally add animation or transition effects when toggling between views

class _FilmCardState extends State<FilmCard> {
  bool showDetails = false;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          showDetails = !showDetails;
        });
      },
      child: SizedBox(
        width: 200,  // TODO: Adjust sizing as needed
        height: 300,
        child: showDetails
            ? FilmDetails(film: widget.film)
            : FilmTitle(film: widget.film),
      ),
    );
  }
}
```

</details>


## Recap

- ✓ Learned the difference between stateless and stateful widgets
- ✓ Understood ephemeral state vs app state
- ✓ Mastered the StatefulWidget pattern (widget class + state class)
- ✓ Learned how `setState()` triggers widget rebuilds
- ✓ Created an interactive `FilmCard` that toggles between two views
- ✓ Practiced conditional rendering based on state

## Next Steps

Now that your film cards are interactive, the next chapter introduces the **View layer** of MVVM architecture. You'll create a `FilmsView` widget that displays multiple film cards in a grid, bringing us closer to building a complete app structure.  
