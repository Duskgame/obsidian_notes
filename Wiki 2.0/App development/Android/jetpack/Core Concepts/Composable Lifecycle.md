[[Jetpack Compose]]

The [[User Interface|UI]] of your app is initially built from running composable functions in a process called Composition.

When the [[State in Compose|State]] of your app changes, a recomposition is scheduled. Recomposition is when [[Jetpack Compose|Compose]] re-executes the composable functions whose state might have changed and creates an updated UI. The Composition is updated to reflect these changes.

The only way to create or update a Composition is by its initial composition and subsequent recompositions.

Composable functions have their own lifecycle that is independent of the [[Activity Lifecycle]]. Its lifecycle is composed of the events: enters the Composition, recomposing 0 or more times, and then leaving the Composition.

In order for Compose to track and trigger a recomposition, it needs to know when state has changed. To indicate to Compose that it should track an [[Kotlin Object|Object]]'s state, the object needs to be of type [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State) or [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState). The `State` type is immutable and can only be read. A `MutableState` type is mutable and allows reads and writes.