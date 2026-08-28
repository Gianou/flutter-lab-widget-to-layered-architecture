# Stateless Widgets

3. Refactor to extract a new custom `FilmTitle()` widget:

  Notice that we've duplicated the same `Container()` with `Text()` code twice. In software development, repeating the same code is generally a bad practice. Instead, we can create our own reusable widget called `FilmTitle()`.

   >[!TIP]
   >Your IDE can help with this too. Select the `Container()` code you want to extract,  
   > **right-click, select "Refactor", then "Extract widget"**  
   > The IDE will create a new widget class for you.  

   Since we want to display different movie titles, the `FilmTitle()` widget needs to accept a `title` parameter as input. This way, each `FilmTitle()` instance can display a different movie name.

   
   

  
### Solution  
#@todo, hide by default
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MainApp());
}

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
              FilmTitle(title: "Castle in The Sky"),
              FilmTitle(title: "Kiki's Delivery Service"),
            ],
          ),
        ),
      ),
    );
  }
}

class FilmTitle extends StatelessWidget {
  final String title;
  const new({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(8),
      decoration: BoxDecoration(
        border: Border.all(color: Colors.red, width: 2),
        borderRadius: BorderRadius.circular(12),
      ),
      child: Text(
        title,
        style: TextStyle(color: Colors.red, fontWeight: FontWeight.bold),
      ),
    );
  }
}
```
