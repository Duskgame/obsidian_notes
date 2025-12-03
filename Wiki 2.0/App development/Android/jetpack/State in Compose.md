You use the [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State) and [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState) types in Compose to make state in your app observable, or tracked, by Compose. The `State` type is immutable, so you can only read the value in it, while the [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState) type is mutable. You can use the [`mutableStateOf()`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#mutableStateOf\(kotlin.Any,androidx.compose.runtime.SnapshotMutationPolicy\)) function to create an observable `MutableState`. It receives an initial value as a parameter that is wrapped in a `State` object, which then makes its `value` observable.

The value returned by the `mutableStateOf()` function:

- Holds state, which is the bill amount.
- Is mutable, so the value can be changed.
- Is observable, so Compose observes any changes to the value and triggers a recomposition to update the UI.

==Compose keeps track of each composable that reads state `value` properties and triggers a recomposition when its `value` changes.==

```
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
   var amountInput = mutableStateOf("0")
   TextField(
       value = amountInput.value,
       onValueChange = { amountInput.value = it },
       modifier = modifier
   )
}

```