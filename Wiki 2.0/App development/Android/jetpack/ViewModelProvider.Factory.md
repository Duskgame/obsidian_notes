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

## Related

- [[ViewModel]]
- [[Database AppContainer]]
