#jetpack_compose 
[`StateFlow`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-state-flow/) is a data holder observable flow that emits the current and new state updates. Its [`value`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-state-flow/value.html) property reflects the current state value. To update state and send it to the flow, assign a new value to the value property of the [`MutableStateFlow`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-mutable-state-flow/index.html) class.

In Android, `StateFlow` works well with classes that must maintain an observable immutable state.

A `StateFlow` can be exposed from the `GameUiState` so that the composables can listen for UI state updates and make the screen state survive configuration changes.

In the `GameViewModel` class, add the following `_uiState` property.

```
import kotlinx.coroutines.flow.MutableStateFlow

// Game UI state
private val _uiState = MutableStateFlow(GameUiState())
```






**StateFlow** is a Kotlin Coroutines **state holder** that combines Flow's power with LiveData's state observation for UI state management.

## Core concept

StateFlow is a **hot Flow** that:

- Always holds a **current value** (requires initial value)
    
- Emits **only when value changes** (built-in `distinctUntilChanged`)
    
- Can be **collected** like any Flow
    

## Basic usage in ViewModel

```
class TipViewModel : ViewModel() {
    // StateFlow holds UI state
    private val _uiState = MutableStateFlow(TipState())
    val uiState: StateFlow<TipState> = _uiState.asStateFlow()
    
    fun onAmountChanged(amount: String) {
        _uiState.value = _uiState.value.copy(amountInput = amount)
    }
}

```
## Collecting in Compose

```
@Composable
fun TipScreen(viewModel: TipViewModel) {
    val state by viewModel.uiState.collectAsState()
    
    // UI reads state reactively
    TextField(value = state.amountInput, onValueChange = viewModel::onAmountChanged)
}

```

## Key characteristics

- **Always has value**: `MutableStateFlow(initialValue)` - no nulls needed
- **Stateful**: `value` property for current state, `collect()` for updates
- **Lifecycle-aware**: Use `collectAsStateWithLifecycle()` or `repeatOnLifecycle`
- **Thread-safe**: Can emit from any thread

## StateFlow vs LiveData

| Feature           | StateFlow     | LiveData  |
| ----------------- | ------------- | --------- |
| Initial value     | Required      | Optional  |
| Emits same values | No (distinct) | Yes       |
| Flow operators    | Full Flow API | Limited   |
| Lifecycle-aware   | Manual        | Automatic |

**Use StateFlow** for modern Kotlin Compose/MVVM apps - it's the recommended replacement for LiveData.

