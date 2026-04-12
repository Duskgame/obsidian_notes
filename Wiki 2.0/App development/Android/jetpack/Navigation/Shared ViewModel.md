By default, each navigation destination gets its own ViewModel instance. But sometimes two screens need to share the same ViewModel — for example, an edit screen and an "add item" screen that both operate on the same in-memory data.

## How it works

You can scope a ViewModel to a specific back stack entry rather than the current destination. As long as that entry is on the back stack, the same ViewModel instance is returned:

```kotlin
// In the child destination's composable block inside NavHost:
val parentEntry = remember(backStackEntry) {
    navController.getBackStackEntry("editQuiz/{quizId}")
}

val sharedViewModel: EditQuizViewModel = viewModel(
    viewModelStoreOwner = parentEntry,
    factory = EditQuizViewModelFactory(repository, syncService, quizId)
)
```

`getBackStackEntry` retrieves the back stack entry for a specific route. By passing it as the `viewModelStoreOwner`, the ViewModel is tied to that entry's lifecycle — not the current screen's.

The factory is only called once (when the ViewModel is first created on the parent entry). All subsequent calls return the existing instance.

## When to use this

- A child screen needs to modify state that belongs to a parent screen's ViewModel
- You want changes made in the child to be immediately reflected in the parent when navigating back, without writing to the database first

## Kwizz example

`EditQuizScreen` holds the quiz being edited in `EditQuizViewModel`. An `AddQuestionScreen` that navigates on top of it can share the same `EditQuizViewModel`:

```
Back stack:
  editQuiz/q1    ← EditQuizViewModel lives here
  addQuestion/q1 ← accesses EditQuizViewModel via getBackStackEntry("editQuiz/{quizId}")
```

When `AddQuestionScreen` calls `editViewModel.addQuestion(...)`, the change immediately appears in `EditQuizScreen`'s `uiState` when the user navigates back.

## Alternative: inline approach

If adding items can be done within the same screen (e.g. appending an empty row to a list), a shared ViewModel may not be needed at all. This is simpler and avoids the parent entry coupling.

## Related

- [[ViewModel]]
- [[NavHost]]
- [[ViewModelProvider.Factory]]
