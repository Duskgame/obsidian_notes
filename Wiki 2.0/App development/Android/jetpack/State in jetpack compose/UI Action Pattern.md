The UI Action Pattern is a way of structuring how the UI communicates user events to the ViewModel. Instead of having a separate public function for every possible user action, all events go through a single `onAction()` function that accepts a sealed interface.

## Why

Without this pattern, a ViewModel accumulates many public functions:
```kotlin
fun onNameChanged(name: String) { ... }
fun addQuestion() { ... }
fun deleteQuestion(q: Question) { ... }
fun save() { ... }
fun upload() { ... }
```

Each composable that calls the ViewModel needs to hold a reference to it, and the composable's parameter list grows with each new callback. This also makes it harder to track what a composable can trigger.

## How it works

Define a sealed interface with one entry per possible user action:

```kotlin
sealed interface EditQuizAction {
    data class QuizNameChanged(val name: String) : EditQuizAction
    data object AddQuestion : EditQuizAction
    data class DeleteQuestion(val question: Question) : EditQuizAction
    data object Save : EditQuizAction
    data object Upload : EditQuizAction
}
```

The ViewModel exposes a single public function:

```kotlin
fun onAction(action: EditQuizAction) {
    when (action) {
        is EditQuizAction.QuizNameChanged -> updateName(action.name)
        EditQuizAction.AddQuestion -> addEmptyQuestion()
        is EditQuizAction.DeleteQuestion -> deleteQuestion(action.question)
        EditQuizAction.Save -> save()
        EditQuizAction.Upload -> upload()
    }
}
```

All other functions become `private` — the only public surface is `onAction`.

The composable takes a single callback instead of many:

```kotlin
@Composable
fun EditQuizContent(
    uiState: EditQuizUiState,
    onAction: (EditQuizAction) -> Unit,
    ...
)
```

And triggers actions like:
```kotlin
Button(onClick = { onAction(EditQuizAction.Save) }) { Text("Save") }
OutlinedTextField(onValueChange = { onAction(EditQuizAction.QuizNameChanged(it)) })
```

## Benefits

- **Single entry point** — one function to mock or test instead of many
- **Exhaustive `when`** — if you add a new action, the compiler forces you to handle it in `onAction`
- **Simpler previews** — the content composable just takes `onAction = {}` for previews
- **Consistent with UDF** — fits Unidirectional Data Flow: UI sends events up, ViewModel sends state down

## data object vs data class

Use `data object` for actions with no parameters (like `Save`, `Upload`, `AddQuestion`) and `data class` for actions that carry data (like `QuizNameChanged(val name: String)`). `data object` is a singleton — there's only one instance — which is correct for parameter-less actions.

## Related

- [[ViewModel]]
- [[UI State]]
- [[One-time Events in UiState]]
