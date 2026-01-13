you can only call a suspend [[Function]] from another suspend function. To call suspend functions safely from inside a composable, you need to use the `LaunchedEffect()` composable. `LaunchedEffect()` composable runs the provided suspending function for as long as it remains in the composition. You can use the `LaunchedEffect()` composable [[Function]] to accomplish all of the following:

- The `LaunchedEffect()` composable allows you to safely call suspend functions from composables.
- When the `LaunchedEffect()` function enters the Composition, it launches a coroutine with the code block passed as a [[Parameter]]. It runs the provided suspend function as long as it remains in the composition. When a user clicks the **Start** button in the RaceTracker app, the `LaunchedEffect()` enters the composition and launches a coroutine to update progress.
- The coroutine is canceled when the `LaunchedEffect()` exits the composition. In the app, if the user clicks the **Reset**/**Pause** button, `LaunchedEffect()` is removed from the composition and the underlying [[Coroutines]] are canceled.

```
if (raceInProgress) {
        LaunchedEffect(playerOne, playerTwo) {
            playerOne.run()
            playerTwo.run()
            raceInProgress = false
        }
    }
```

