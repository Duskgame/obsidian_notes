Some ViewModel operations need to signal something to the UI exactly once — not a persistent state value, but a momentary event like "navigate away" or "put this new item into edit mode". Handling these correctly in a StateFlow-based architecture requires care.

## The problem

StateFlow always holds a value. If you set `isSaved = true` and the UI collects it, everything works — but if the composable recomposes for any reason while `isSaved` is still `true`, the effect triggers again. For navigation this usually doesn't matter (you're already gone), but for things like setting focus or local UI state it can cause bugs.

## Pattern 1 — Boolean flag (simple cases)

For navigation triggers, a simple boolean is usually fine:

```kotlin
// UiState
val isSaved: Boolean = false

// ViewModel
_uiState.update { it.copy(isSaved = true) }

// Composable
LaunchedEffect(uiState.isSaved) {
    if (uiState.isSaved) onSave()
}
```

This works because `LaunchedEffect` only re-runs when the key changes. Once `isSaved` becomes `true` it stays true, so `LaunchedEffect` fires once. The screen then navigates away before any further recomposition matters.

## Pattern 2 — Nullable ID (communicating data back)

When the ViewModel needs to hand a value back to the composable once — such as the ID of a newly added item so the UI can put it into edit mode — use a nullable field:

```kotlin
// UiState
val newAddedQuestionId: String? = null

// ViewModel — set when item is created
_uiState.update { it.copy(newAddedQuestionId = newQuestion.questionId) }

// ViewModel — reset after consumption (via a ConsumeNewQuestion action)
_uiState.update { it.copy(newAddedQuestionId = null) }
```

```kotlin
// Composable — consume and immediately reset
LaunchedEffect(uiState.newAddedQuestionId) {
    uiState.newAddedQuestionId?.let { id ->
        editingQuestionId = id               // use the value
        viewModel.onAction(ConsumeNewQuestion) // reset to null
    }
}
```

The `LaunchedEffect` key is the nullable value itself. It fires when it changes from `null` → `"some-id"`, the composable consumes it and dispatches the reset action, which changes it back to `null`. This prevents the effect from triggering again on recomposition.

## Why not return values from ViewModel functions?

ViewModel functions are fire-and-forget by design — the state flows one way (ViewModel → UI via StateFlow). Returning a value from a ViewModel function breaks this pattern and makes the composable depend on the timing of the call rather than observing state. Use UiState fields instead.

## Related

- [[UI Action Pattern]]
- [[StateFlow]]
- [[State in Compose]]
- [[remember() key]]
