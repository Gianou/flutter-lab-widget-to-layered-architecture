# MVVM: View

As your application grows, managing all state, logic, and UI in one place becomes difficult. **MVVM (Model-View-ViewModel)** is an architecture pattern that separates these concerns into distinct layers. Let's start with the **View** layer.

## What is MVVM?

MVVM separates your application into three layers:

- **Model**: The data and business logic. It represents what your app knows (film data, APIs, databases)
- **ViewModel**: The intermediary. It holds the state and logic needed by the View, preparing data for display
- **View**: What the user sees. It displays data and captures user interactions

This separation makes your code cleaner, easier to test, and easier to maintain.

## The View Layer

Think of Views like pages in your application. Each page/route typically has its own View. When users navigate to different parts of your app, they're moving between different Views.


## Combining It All: The FilmsView

Now let's create a View that displays a list of films using the `FilmCard` widget you built in the previous chapter.

Your `FilmsView` will:
- Use `Scaffold` to provide the page structure (app bar, body)
- Display multiple `FilmCard` widgets in a grid or list

This View will later receive data from a ViewModel, but for now, you can pass sample `Film` objects.

## Practice

1. **Create a FilmsView widget** that extends `StatelessWidget`
   - Use `Scaffold` with an `appBar` showing "Ghibli Films"
   - Use a `GridView` or `ListView` to display multiple `FilmCard` widgets
   - Pass sample `Film` objects to each card



## Next Steps

The View is what users interact with, but it needs data and logic. The next chapters will introduce the ViewModel and Model layers to complete the MVVM pattern.
