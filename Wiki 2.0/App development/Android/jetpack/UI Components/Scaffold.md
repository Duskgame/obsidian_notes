`Scaffold` is a Material 3 layout composable that implements the standard visual structure of a screen — it handles the placement of common UI elements so you don't have to position them manually.

## Why use it

Without Scaffold, you have to manually account for system insets (status bar, navigation bar) and the space taken up by things like a bottom bar or FAB. Scaffold handles all of that via `innerPadding`.

## Basic structure

```kotlin
Scaffold(
    floatingActionButton = {
        FloatingActionButton(onClick = { /* ... */ }) {
            Icon(Icons.Default.Add, contentDescription = "Add")
        }
    }
) { innerPadding ->
    // Your screen content goes here
    Column(modifier = Modifier.padding(innerPadding)) {
        // ...
    }
}
```

The lambda parameter `innerPadding` is a `PaddingValues` object. Always apply it to your content — it accounts for the space the FAB, top bar, and system bars take up. If you ignore it, content may be obscured.

## Common slots

| Parameter | Purpose |
|---|---|
| `topBar` | App bar at the top (e.g. `TopAppBar`) |
| `bottomBar` | Navigation bar at the bottom |
| `floatingActionButton` | FAB, typically in the bottom-right corner |
| `content` | The main screen content (receives `innerPadding`) |

## FloatingActionButton (FAB)

A FAB is a prominent circular button for the primary action on a screen. The `+` (Add) FAB is the most common pattern:

```kotlin
FloatingActionButton(onClick = onAddClicked) {
    Icon(Icons.Default.Add, contentDescription = "Add item")
}
```

Place the FAB in the `floatingActionButton` slot of `Scaffold` rather than positioning it manually — Scaffold ensures it doesn't overlap content and respects the navigation bar.

## Related

- [[Jetpack Compose]]
- [[Composable function]]
- [[Navigation]]
