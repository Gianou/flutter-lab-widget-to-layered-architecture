# Additional Theory

Congratulations on completing this introductory lab on Flutter.

The basics of Flutter Widgets, or how to build a UI in Flutter were covered, as well as a simple implementation of the MVVM pattern.  

However, some shortcuts have been taken to keep this lab succinct.  

This chapter is a collection of notes and links that go beyond the work done in this lab. 

## MVVM
The MVVM pattern is widely used across many frameworks, not only Flutter. And while the name of this pattern is widely used, its implementation may vary from one source to another.  

Our implementation was basic, covering each layer, its components, and how to link them together. This is based on the [Set Up State Project](https://docs.flutter.dev/learn/pathway/tutorial/set-up-state-project) tutorial from Flutter.  

### Flutter Case Study
Flutter also has training material on a more advanced MVVM implementation in their [Architecture Case Study](https://docs.flutter.dev/app-architecture/case-study). The main differences are about the components of the Model or "Data layer" and how layers are connected. 

#### Data Layer
In Flutter's study case, the data layer includes services and repositories. In this case, the repositories are used as Single Source Of Truth (SSOT) to host the data that is needed by multiple ViewModels.

#### Dependency Injection
Instead of instantiating the ViewModel directly in the View, the study case uses Dependency Injection. To learn more about dependency injection, see [Flutter Study Case, dependency injection](https://docs.flutter.dev/app-architecture/case-study/dependency-injection)


## Navigation
Navigation was not covered in this tutorial. Flutter includes a basic built-in navigation API, but the [`GoRouter`](https://pub.dev/packages/go_router) library is often recommended over the use of the built-in `Navigator`.

## Folder Structure
The folder structure used in this lab is simplistic and was chosen to highlight the explanation of MVVM.
For a more complex example of folder structure, see [Flutter Study Case, package structure](https://docs.flutter.dev/app-architecture/case-study#package-structure)

## State Management

In this lab, we used `ChangeNotifier` for state management. As your apps grow, 
other state management solutions become valuable:

- [`Provider`](https://pub.dev/packages/provider) - Simple wrapper around ChangeNotifier
- [`Riverpod`](https://riverpod.dev/) - Modern, type-safe alternative
- [`GetX`](https://pub.dev/packages/get) - All-in-one solution

Each has different philosophies and trade-offs. Explore them as your needs grow.


## Flutter BuildContext

You may have noticed that BuildContext context is a parameter in all widgets we created. To learn more about the underlying of widgets and BuildContext, check this video from Flutter: [BuildContext?! | Decoding Flutter](https://www.youtube.com/watch?v=rIaaH87z1-g)


## UX & UI

UX and UI are important parts of any frontend framework. If you are interested in these topics, see Flutter's documentation:

- [Adaptive and responsive design](https://docs.flutter.dev/ui/adaptive-responsive)
- [Animations](https://docs.flutter.dev/ui/animations)
