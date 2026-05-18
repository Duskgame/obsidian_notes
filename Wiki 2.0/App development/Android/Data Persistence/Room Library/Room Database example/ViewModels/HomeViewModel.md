```kotlin
/**
 * ViewModel to retrieve all items in the Room database.
 */
class HomeViewModel(itemsRepository: ItemsRepository) : ViewModel() {

    /**
     * Holds home ui state. The list of items are retrieved from [ItemsRepository] and mapped to
     * [HomeUiState]
     */
    val homeUiState: StateFlow<HomeUiState> =
        itemsRepository.getAllItemsStream().map { HomeUiState(it) }
            .stateIn(
                scope = viewModelScope,
                started = SharingStarted.WhileSubscribed(TIMEOUT_MILLIS),
                initialValue = HomeUiState()
            )

    companion object {
        private const val TIMEOUT_MILLIS = 5_000L
    }
}

/**
 * Ui State for HomeScreen
 */
data class HomeUiState(val itemList: List<Item> = listOf())

```

This `HomeViewModel` fetches and exposes the full inventory list from [[Room]] [[Database]] as reactive [[UI State]], following [[Android]]'s [[Model-View-ViewModel|MVVM]] architecture with [[Kotlin]] Flows for automatic [[Database]]-to-UI synchronization.

## Reactive Data Flow Explained

The core line [[ItemsRepository]].getAllItemsStream().map { HomeUiState(it) }.stateIn(...) creates a reactive pipeline:
- `getAllItemsStream()` returns a `Flow<List<Item>>` from [[Room]] [[Data Access Object|DAO]] (`@Query("`[[SQL SELECT|SELECT]]` * FROM items") Flow<List<Item>>`)
- [[Room]] automatically emits new results whenever the [[Database]] table changes (insert/update/delete)
- `.map()` transforms raw items into `HomeUiState` for UI consumption
- `.stateIn()` converts to [[StateFlow]] with lifecycle-aware collection and initial empty [[State in Compose|State]][ from prior]

## StateFlow Configuration Details
```kotlin
.stateIn(
    scope = viewModelScope,           // Cancels on ViewModel clearance
    started = WhileSubscribed(5000),  // Stops collecting after 5s inactivity
    initialValue = HomeUiState()      // Shows empty list immediately
)
```

This prevents memory leaks and battery drain—collection pauses when HomeScreen is off-screen but resumes instantly on return with fresh data.

## Database Architecture Integration

Completes the full stack from prior code:

```
InventoryApplication → AppDataContainer → ItemsRepository → Room DAO → SQLite
                                                           ↑
                                                      HomeViewModel → HomeScreen observes

```

**HomeScreen** collects `homeUiState` in `LaunchedEffect` or `collectAsState()`, rebuilding UI automatically when inventory changes elsewhere (ItemEntry/Edit screens). [[Repository]] typically runs queries on `Dispatchers.IO` for thread safety.

## Benefits Over LiveData

Modern Kotlin-first approach replaces LiveData:

- Type-safe, composable with other Flows (filter, combine, etc.)
- Works natively with [[Jetpack Compose]]
- Survives [[ViewModel]] lifecycle, config changes via SavedStateHandle (seen in other ViewModels)  
    Repository pattern centralizes data logic, enabling offline-first with network sync later.

Any database mutation (via other ViewModels) instantly updates HomeScreen—no manual refresh needed.