[[Coroutines]] are typically launched into a `CoroutineScope`. This ensures that we don't have coroutines that are unmanaged and get lost, which could waste resources.

`launch()` and `async()` are [extension functions](https://kotlinlang.org/docs/extensions.html) on `CoroutineScope`. Call `launch()` or `async()` on the scope to create a new coroutine within that scope.

A `CoroutineScope` is tied to a lifecycle, which sets bounds on how long the coroutines within that scope will live. If a scope gets cancelled, then its [[Job]] is cancelled, and the cancellation of that propagates to its child jobs. If a child job in the scope fails with an exception, then other child jobs get cancelled, the parent job gets cancelled, and the exception gets re-thrown to the caller.

## CoroutineScope in Kotlin Playground

`runBlocking()` provides a `CoroutineScope` for your program. You also learned how to use `coroutineScope { }` to create a new scope within the `getWeatherReport()` [[Function]].

## CoroutineScope in Android apps

[[Android]] provides coroutine scope support in entities that have a well-defined lifecycle, such as `Activity` (`lifecycleScope`) and `ViewModel` (`viewModelScope`). Coroutines that are started within these scopes will adhere to the lifecycle of the corresponding entity, such as `Activity` or `ViewModel`.

For example, say you start a coroutine in an `Activity` with the provided coroutine scope called `lifecycleScope`. If the activity gets destroyed, then the `lifecycleScope` will get canceled and all its child coroutines will automatically get canceled too. You just need to decide if the coroutine following the lifecycle of the `Activity` is the behavior you want.

## Implementation Details of CoroutineScope

If you check the source code for how [`CoroutineScope.kt`](https://cs.android.com/android/platform/superproject/+/master:external/kotlinx.coroutines/kotlinx-coroutines-core/common/src/CoroutineScope.kt?q=coroutinescope) is implemented in the [[Kotlin]] coroutines library, you can see that `CoroutineScope` is declared as an [[Interface]] and it contains a `[[CoroutineContext]]` as a variable.

The `launch()` and `async()` functions create a new child coroutine within that scope and the child also inherits the context from the scope. 