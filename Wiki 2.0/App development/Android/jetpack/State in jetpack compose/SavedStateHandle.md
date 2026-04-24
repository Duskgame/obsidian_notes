[[ViewModel]]

**SavedStateHandle** is a key-value store provided by the Android framework that survives both configuration changes (rotation) and process death. It is injected into a ViewModel as a constructor parameter.

## Core purpose

A regular ViewModel survives rotation but loses its data if the process is killed by the OS (e.g. the app is backgrounded for a long time). `SavedStateHandle` persists data across process death by writing to the saved instance state bundle.

It also serves as the bridge between the navigation back stack and a ViewModel — when a ViewModel is created in the context of a navigation destination, the framework automatically populates `SavedStateHandle` with that destination's route arguments.

## Basic usage

```kotlin
class DetailViewModel(
    savedStateHandle: SavedStateHandle
) : ViewModel() {
    val itemId: String = checkNotNull(savedStateHandle["itemId"])
}
```

The key `"itemId"` matches the navigation argument name defined in the route.

## With type-safe navigation

When using `@Serializable` route objects (Navigation 2.8+), you can deserialize the entire route directly:

```kotlin
class QuizViewModel(
    private val repository: KwizzRepository,
    savedStateHandle: SavedStateHandle
) : ViewModel() {
    private val quizId = savedStateHandle.toRoute<NavigationRoute.Quiz>().quizId
}
```

`toRoute<T>()` reconstructs the typed route object from the handle. This removes the need to pass navigation arguments through composable parameters down to the ViewModel.

## Getting it in a factory

When using `viewModelFactory { initializer {} }`, call `createSavedStateHandle()` inside the `initializer` block — it reads the current back stack entry's arguments:

```kotlin
initializer {
    QuizViewModel(
        repository = kwizzApplication().container.repository,
        savedStateHandle = createSavedStateHandle()
    )
}
```

This must be called inside the `initializer` block because it needs to know which destination is currently being created.

## Related

- [[ViewModel]]
- [[ViewModelProvider.Factory]]
- [[Type-safe Navigation]]
