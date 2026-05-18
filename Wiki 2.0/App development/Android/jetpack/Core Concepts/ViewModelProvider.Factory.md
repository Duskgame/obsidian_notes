By default, Android creates ViewModels using a no-argument constructor. A **ViewModelProvider.Factory** is needed when your ViewModel requires constructor parameters (like a repository or an ID).

## The problem it solves

```kotlin
// This works — no parameters needed
val vm: SimpleViewModel = viewModel()

// This does NOT work — Android doesn't know how to pass the repository
val vm: QuizViewModel = viewModel()  // QuizViewModel(repository: KwizzRepository)
```

Without a factory, Android can't construct a ViewModel that needs arguments.

## How to implement one

```kotlin
class EditQuizViewModelFactory(
    private val repository: KwizzRepository,
    private val quizId: String?
) : ViewModelProvider.Factory {

    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(EditQuizViewModel::class.java)) {
            @Suppress("UNCHECKED_CAST")
            return EditQuizViewModel(repository, quizId) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```

## How to use it in Compose

```kotlin
@Composable
fun EditQuizScreen(quizId: String?) {
    val app = LocalContext.current.applicationContext as KwizzApplication
    val viewModel: EditQuizViewModel = viewModel(
        factory = EditQuizViewModelFactory(app.container.repository, quizId)
    )
}
```

The `factory` parameter tells the `viewModel()` composable how to construct the ViewModel.

## Pattern

Each ViewModel that needs constructor arguments gets its own factory class. The factory is typically placed in the same package as the ViewModel.

```
editQuizScreen/
├── EditQuizUiState.kt
├── EditQuizViewModel.kt       ← the ViewModel
├── EditQuizViewModelFactory.kt ← the factory
└── EditQuizScreen.kt
```

## Consolidated factory with viewModelFactory

Instead of a separate factory class per ViewModel, all factories can be merged into a single `AppViewModelProvider` object using the `viewModelFactory { initializer {} }` API. This avoids boilerplate and keeps all wiring in one place.

```kotlin
object AppViewModelProvider {
    val Factory = viewModelFactory {
        initializer {
            StartViewModel(kwizzApplication().container.repository)
        }
        initializer {
            QuizViewModel(
                repository = kwizzApplication().container.repository,
                savedStateHandle = createSavedStateHandle()
            )
        }
    }
}

fun CreationExtras.kwizzApplication(): KwizzApplication =
    (this[AndroidViewModelFactory.APPLICATION_KEY] as KwizzApplication)
```

- `CreationExtras` is a key-value bag the framework attaches to every ViewModel creation call. `APPLICATION_KEY` gives you the `Application` instance without needing `LocalContext` in the composable.
- `createSavedStateHandle()` builds the `SavedStateHandle` for the ViewModel from the current navigation back stack entry. Must be called inside `initializer {}`.

Usage in any composable:

```kotlin
val viewModel: StartViewModel = viewModel(factory = AppViewModelProvider.Factory)
```

The composable no longer needs to cast `LocalContext` to the application class.

## Related

- [[ViewModel]]
- [[SavedStateHandle]]
- [[Database AppContainer]]
