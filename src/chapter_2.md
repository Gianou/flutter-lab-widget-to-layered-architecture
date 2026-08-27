# Widget Composition

Now that our project is set-up, let's look at the very basics of Flutter: How to Compose the UI.

## Widgets Introduction

In Flutter, everything that appears on the screen, and dictate how it appears on the screens, is called a Widget. Widgets are Dart classes. Some are defined as part of the Flutter's <a href="https://docs.flutter.dev/ui/widgets/material" target="_blank">Material Components package</a>, and you can also create your own Widgets.

In the default empty project, there are already five widgets:
- `Text()`: A leaf widget responsible for rendering a specific string of characters on the screen.
- `Center()`: A layout widget that forces its single child to be positioned in the middle of the available space.
- `Scaffold()`: Provides the fundamental visual structure for a page, including background, app bars, and body placement.
- `MaterialApp()`: The root widget that initializes the application with Material Design standards and navigation.
- `MainApp()`: The custom root widget defined in your code that wraps the MaterialApp to start the widget tree.


```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(body: Center(child: Text('Hello World!'))),
    );
  }
}
```

### Practice
It is time to practice, let's change the "Hello World!" message to a movie title and add some styling:  
    ![alt text](image-5.png)


1. Change the `String` parameter passed to the `Text()` widget and save the change to see the application hot reload to show the changes.
2. `Text()` can take optional arguments in the form of `key: value`. Add a second parameter `style: TextStyle()` and complete `TextStyle()` to change the color and font weight.

    >[!TIP] 
    >You don't need to memorize properties. Before typing, hover over TextStyle in your IDE to see all available parameters. Then, inside the parentheses, start typing color or fontWeight and let auto-completion guide you.  



### Solution
#@todo, hide by default

```dart

          child: Text(
            "Castle in The Sky",
            style: TextStyle(color: Colors.red, fontWeight: FontWeight.bold),
          ),
     
```
## Widgets Composition
