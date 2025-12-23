#jetpack_compose 

**ViewModel** in Kotlin/Android is a lifecycle-aware class that holds UI-related data and survives configuration changes like screen rotations.

## Core purpose

- **Survives config changes**: Unlike Activities/Fragments, ViewModels live until the Activity is permanently destroyed.
- **Holds UI state**: Manages data like text inputs, loading states, lists that the UI needs.
- **No UI references**: Pure Kotlin class, no direct access to views or Context.

## Basic structure

```
class TipViewModel : ViewModel() {
    // State holders (StateFlow/LiveData)
    private val _tipAmount = MutableStateFlow("")
    val tipAmount: StateFlow<String> = _tipAmount.asStateFlow()
    
    // Business logic
    fun calculateTip(amount: String, percent: String) {
        val tip = calculateTipLogic(amount.toDoubleOrNull() ?: 0.0, percent.toDoubleOrNull() ?: 15.0)
        _tipAmount.value = tip
    }
}

```

## Usage in Activity/Fragment/Compose

```
// In Compose
@Composable
fun TipScreen(viewModel: TipViewModel = viewModel()) {
    val tip by viewModel.tipAmount.collectAsState()
    Text("Tip: $tip")
}

// Getting ViewModel
class MainActivity : ComponentActivity() {
    private val viewModel: TipViewModel by viewModels()
}

```

## Key lifecycle

```
Activity created → ViewModel created → Activity destroyed (rotation) → 
ViewModel survives → New Activity observes same ViewModel → Activity finished → ViewModel destroyed
```

## Fits MVVM + UDF

```
UI (View) ← observes StateFlow (read-only)
UI → calls ViewModel functions (events)
ViewModel → updates internal state
ViewModel → calls business logic/Repository
```





The `ViewModel` component holds and exposes the state the UI consumes. The UI state is application data transformed by `ViewModel`. `ViewModel` lets your app follow the architecture principle of driving the UI from the model.

`ViewModel` stores the app-related data that isn't destroyed when the activity is destroyed and recreated by the Android framework. Unlike the activity instance, `ViewModel` objects are not destroyed. The app automatically retains `ViewModel` objects during configuration changes so that the data they hold is immediately available after the recomposition.

To implement `ViewModel` in your app, extend the `ViewModel` class, which comes from the architecture components library and stores app data within that class.


To  save the game data in the `ViewModel`
1. Open `build.gradle.kts (Module :app)`, scroll to the `dependencies` block and add the following dependency for `ViewModel`. This dependency is used for adding the lifecycle aware viewmodel to your compose app.

```
dependencies {
// other dependencies

    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1")
//...
}

```

2. In the `ui` package, create a Kotlin class/file called `GameViewModel`. Extend it from the `ViewModel` class.

```
import androidx.lifecycle.ViewModel

class GameViewModel : ViewModel() {
}
```


## Backing property

A backing property lets you return something from a getter other than the exact object.

For `var` property, the Kotlin framework generates getters and setters.

For getter and setter methods, you can override one or both of these methods and provide your own custom behavior. To implement a backing property, you override the getter method to return a read-only version of your data. The following example shows a backing property:

```
// Declare private mutable variable that can only be modified
// within the class it is declared.
private var _count = 0 

// Declare another public immutable field and override its getter method. 
// Return the private property's value in the getter method.
// When count is accessed, the get() function is called and
// the value of _count is returned. 
val count: Int
    get() = _count

```

As another example, say that you want the app data to be private to the `ViewModel`:

Inside the `ViewModel` class:

- The property `_count` is `private` and mutable. Hence, it is only accessible and editable within the `ViewModel` class.

Outside the `ViewModel` class:

- The default visibility modifier in Kotlin is `public`, so `count` is public and accessible from other classes like UI controllers. A `val` type cannot have a setter. It is immutable and read-only so you can only override the `get()` method. When an outside class accesses this property, it returns the value of `_count` and its value can't be modified. This backing property protects the app data inside the `ViewModel` from unwanted and unsafe changes by external classes, but it lets external callers safely access its value.

1. In the `GameViewModel.kt` file, add a backing property to `uiState` named `_uiState`. Name the property `uiState` and is of the type `StateFlow<GameUiState>`.

Now `_uiState` is accessible and editable only within the `GameViewModel`. The UI can read its value using the read-only property, `uiState`. You can fix the initialization error in the next step.

```
import kotlinx.coroutines.flow.StateFlow

// Game UI state

// Backing property to avoid state updates from other classes
private val _uiState = MutableStateFlow(GameUiState())
val uiState: StateFlow<GameUiState> 
```

2. Set `uiState` to `_uiState.asStateFlow()`.

The `asStateFlow()` makes this mutable state flow a _read-only_ state flow.

```
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asStateFlow

// Game UI state
private val _uiState = MutableStateFlow(GameUiState())
val uiState: StateFlow<GameUiState> = _uiState.asStateFlow()
```


