```
sealed interface AmphibianUiState {
    data class Success(val infos: List<AmphibianInfo>) : AmphibianUiState
    object Error : AmphibianUiState
    object Loading : AmphibianUiState
}
```

`AmphibianUiState` is the **[[UI State]] model** that represents all possible states a screen can be in: loading data, showing data, or showing an error. It's a **sealed [[Interface]]** designed specifically for [[jetpack]] Compose's declarative UI pattern.

## Role in architecture: The contract between ViewModel and UI
```
ViewModel.amphibianUiState ← UI Composable reads this
        ↓
AmphibianUiState sealed states
        ↓
when (uiState) { ... } → renders Loading/Success/Error UI
```

## Why sealed interface for UI state

**Exhaustive `when` expressions**:
```
@Composable
fun AmphibianScreen(uiState: AmphibianUiState) {
    when (uiState) {
        AmphibianUiState.Loading -> CircularProgressIndicator()
        is AmphibianUiState.Success -> LazyColumn { 
            items(uiState.infos) { AmphibianItem(it) } 
        }
        AmphibianUiState.Error -> Text("Network error")
        // Compiler error if new state added without handling
    }
}
```


**Type safety**: UI can only be in **one state at a time** - never "loading + error"
## Each state explained
```
sealed interface AmphibianUiState {
    data class Success(val infos: List<AmphibianInfo>) : AmphibianUiState  // Has data
    object Error : AmphibianUiState                                        // No data needed
    object Loading : AmphibianUiState                                      // No data needed
}
```

- **`Success`**: Contains the actual `List<AmphibianInfo>` from network
- **`Error`** / **`Loading`**: `[[Kotlin Object|Object]]` = no data needed, just the state itself

## Complete data flow
```
1. ViewModel init → getAmphibianInfos()
2. Repository → Network → Mars server  
3. ViewModel receives List<AmphibianInfo>
4. ViewModel emits: Success(list)
5. UI reads amphibianUiState → recomposes → shows list
6. On error → emits: Error → UI shows error screen
```

## Why this pattern wins

 **Predictable UI**: Exactly 3 possible screen states  
 **Exhaustive**: Compiler forces you to handle all cases  
 **Reactive**: `mutableStateOf` → automatic UI updates  
 **Simple**: No complex loading flags or nullable fields

This is the **standard pattern** from Google's [[Android]] architecture samples for single-data-source screens.