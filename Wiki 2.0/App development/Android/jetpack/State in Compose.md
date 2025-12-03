---
aliases:
  - " "
  - State
---
- State in an app is any value that can change over time.
- The _Composition_ is a description of the [[User Interface|UI]] built by [[Jetpack Compose|Compose]] when it executes composables. Compose apps call composable functions to transform data into UI.
- Initial composition is a creation of the UI by Compose when it executes composable functions the first time.
- Recomposition is the process of running the same composables again to update the tree when their data changes.
- State hoisting is a pattern of moving state to its caller to make a component stateless.

You use the [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State) and [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState) types in Compose to make state in your app observable, or tracked, by Compose. The `State` type is immutable, so you can only read the value in it, while the [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState) type is mutable. You can use the [`mutableStateOf()`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#mutableStateOf\(kotlin.Any,androidx.compose.runtime.SnapshotMutationPolicy\)) [[Function]] to create an observable `MutableState`. It receives an initial value as a [[Parameter]] that is wrapped in a `State` [[Kotlin Object|Object]], which then makes its `value` observable.

The value returned by the `mutableStateOf()` function:

- Holds state, which is the bill amount.
- Is mutable, so the value can be changed.
- Is observable, so Compose observes any changes to the value and triggers a recomposition to update the UI.

==Compose keeps track of each composable that reads state `value` [[Kotlin Class Properties|properties]] and triggers a recomposition when its `value` changes.==

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

Composable functions can store an object across recompositions with the [`remember`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#remember\(kotlin.Function0\)). A value computed by the `remember` function is stored in the Composition during initial composition and the stored value is returned during recomposition. Usually `remember` and `mutableStateOf` functions are used together in composable functions to have the state and its updates be reflected properly in the UI.

```
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
   var amountInput by remember { mutableStateOf("") }
   TextField(
       value = amountInput,
       onValueChange = { amountInput = it },
       modifier = modifier
   )
}

```



![[Pasted image 20251203160548.png]]

This pattern is called _state hoisting_. In the next section, you _hoist_, or lift, the state from a composable to make it stateless.

**Note:** A stateless composable is a composable ​​that doesn't store its own state. It displays whatever state it's given as input [[Arguments]].

## Understand stateful versus stateless **composables**

You should hoist the state when you need to:

- Share the state with multiple composable functions.
- Create a stateless composable that can be reused in your app.

When you extract state from a [[Composable function]], the resulting composable function is called stateless. That is, composable functions can be made stateless by extracting state from them.

==A _stateless_ composable is a composable that doesn't have a state, meaning it doesn't hold, define, or modify a new state. 
On the other hand, a _stateful_ composable is a composable that owns a piece of state that can change over time.==


State hoisting is a pattern of moving state to its caller to make a component stateless.

When applied to composables, this often means introducing two parameters to the composable:

- A `value: T` parameter, which is the current value to display.
- An `onValueChange: (T) -> [[Unit]]` – callback [[Lambda]], which is triggered when the value changes so that the state can be updated elsewhere, such as when a user enters some text in the text box.

## **Learn more**

- [State and Jetpack Compose](https://developer.android.com/jetpack/compose/state)
- [State in Jetpack Compose codelab](https://developer.android.com/codelabs/jetpack-compose-state)
- [Thinking in Compose](https://developer.android.com/jetpack/compose/mental-model)
- [Jetpack Compose: State](https://youtu.be/mymWGMy9pYI)
- [A Compose state of mind: Using Compose's automatic state observation](https://www.youtube.com/watch?v=rmv2ug-wW4U)
- [Where to hoist state | Compose | Android Developers](https://developer.android.com/jetpack/compose/state-hoisting)