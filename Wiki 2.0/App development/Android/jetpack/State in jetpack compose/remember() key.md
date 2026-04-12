`remember` accepts an optional key (or multiple keys). When the key changes, the remembered value is discarded and recalculated from scratch.

## Syntax

```kotlin
var text by remember(someKey) { mutableStateOf(initialValue) }
```

Without a key, the value is remembered for the entire lifetime of the composable in the composition. With a key, it resets whenever the key value changes.

## Why this matters in lists

In a `LazyColumn`, composable slots get reused as items scroll on and off screen. If you remember local state inside an item composable without a key, that state can "leak" from one item to another as slots are recycled.

Use the item's stable ID as the key to tie the state to the specific item:

```kotlin
@Composable
fun QuestionEditItem(question: Question) {
    var questionText by remember(question.questionId) { mutableStateOf(question.question) }
    var answerText   by remember(question.questionId) { mutableStateOf(question.answer) }
    // ...
}
```

When a different question is bound to this composable slot (different `questionId`), the remembered text resets to that question's actual values. Without the key, the old text from a previous question would remain.

## Another use case — resetting on data change

If you save a question via the ViewModel and the question object in `uiState` updates, passing the updated question to the composable changes the key, which resets the local state to the new saved values:

```kotlin
// After viewModel.updateQuestion(updated), the question in the list changes.
// remember(question.questionId) resets, picking up the new question.question value.
```

## Multiple keys

```kotlin
var value by remember(keyA, keyB) { mutableStateOf(default) }
```

Resets if either key changes.

## Related

- [[State in Compose]]
- [[LazyColumn]]
