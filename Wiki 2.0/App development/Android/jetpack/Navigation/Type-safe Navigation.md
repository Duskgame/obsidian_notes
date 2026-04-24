[[Navigation]]

Type-safe navigation (introduced in Navigation 2.8) replaces the old string-based routing with `@Serializable` Kotlin objects. Arguments are passed as constructor parameters and deserialized automatically — no more manual string templates or `arguments?.getString()`.

## Route definition

Each destination is a `@Serializable` data object or data class. Wrap them in a sealed interface for a closed, compile-time-known set:

```kotlin
sealed interface NavigationRoute {

    @Serializable
    data object StartScreen : NavigationRoute

    @Serializable
    data class Quiz(val quizId: String, val quizName: String) : NavigationRoute

    @Serializable
    data class EditQuiz(val quizId: String?, val quizName: String?) : NavigationRoute

    @Serializable
    data object Browse : NavigationRoute
}
```

No `route: String` property, no `fun route()` helper — the serialization handles it.

## NavHost setup

Use `composable<T>` instead of `composable("string")`:

```kotlin
NavHost(
    navController = navController,
    startDestination = NavigationRoute.StartScreen  // pass the object, not a string
) {
    composable<NavigationRoute.StartScreen> {
        StartScreen(...)
    }

    composable<NavigationRoute.Quiz> { backStackEntry ->
        val route = backStackEntry.toRoute<NavigationRoute.Quiz>()
        QuizScreen(quizId = route.quizId)
    }

    composable<NavigationRoute.EditQuiz> { backStackEntry ->
        val route = backStackEntry.toRoute<NavigationRoute.EditQuiz>()
        EditQuizScreen(quizId = route.quizId)
    }
}
```

## Navigating

Pass the route object directly — no string building:

```kotlin
// Old way
navController.navigate("quiz/$quizId")

// New way
navController.navigate(NavigationRoute.Quiz(quizId = quiz.quizId, quizName = quiz.name))
```

## Reading the current destination

`hasRoute<T>()` checks if the current destination matches a route type:

```kotlin
val destination = navController.currentBackStackEntry?.destination

val title = when {
    destination?.hasRoute<NavigationRoute.StartScreen>() == true -> "Home"
    destination?.hasRoute<NavigationRoute.Quiz>() == true ->
        navController.currentBackStackEntry?.toRoute<NavigationRoute.Quiz>()?.quizName ?: "Quiz"
    else -> "Kwizz"
}
```

## In a ViewModel via SavedStateHandle

The route arguments are automatically available in the ViewModel's `SavedStateHandle`:

```kotlin
class QuizViewModel(savedStateHandle: SavedStateHandle) : ViewModel() {
    private val quizId = savedStateHandle.toRoute<NavigationRoute.Quiz>().quizId
}
```

This means composables don't need to receive `quizId` as a parameter and pass it down — the ViewModel reads it directly.

## Requirements

- Navigation 2.8+
- `kotlinx.serialization` plugin applied
- `@Serializable` on every route class

## Related

- [[Sealed Classes Interfaces]]
- [[NavHost]]
- [[SavedStateHandle]]
