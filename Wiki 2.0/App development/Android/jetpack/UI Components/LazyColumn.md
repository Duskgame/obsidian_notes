`LazyColumn` is a vertically scrolling list composable that only composes and renders items currently visible on screen — it's the Compose equivalent of `RecyclerView`.

## Why "lazy"

A regular `Column` renders all its children immediately, even those off-screen. For long or dynamic lists this wastes memory and causes slow composition. `LazyColumn` renders items on demand as the user scrolls.

## Basic usage

```kotlin
LazyColumn {
    items(myList) { item ->
        Text(item.name)
    }
}
```

`items()` takes a list and a composable lambda that receives one item at a time.

## item {} vs items {}

You can mix individual items and list items in the same `LazyColumn`:

```kotlin
LazyColumn {
    item {
        Text("Header")               // single item
    }
    items(questions) { question ->   // one item per list element
        QuestionRow(question)
    }
    item {
        // "Add question" button always at the bottom, scrolls with the list
        TextButton(onClick = onAddClicked) {
            Text("Add question")
        }
    }
}
```

This is useful for adding headers, footers, or action buttons that should scroll with the content rather than be fixed outside the list.

## key parameter

Providing a stable key for each item helps Compose track items across recompositions (e.g. after adding or deleting):

```kotlin
items(questions, key = { it.questionId }) { question ->
    QuestionRow(question)
}
```

Without a key, Compose uses position — adding an item at the top recomposes every item below it. With a key, only the new item is composed.

## contentPadding

Adds padding inside the scrollable area without cutting off content at the edges:

```kotlin
LazyColumn(contentPadding = PaddingValues(16.dp)) { ... }
```

Using `Modifier.padding()` instead would clip items at the edges when scrolling.

## Related

- [[Jetpack Compose]]
- [[Composable function]]
- [[remember() key]]
