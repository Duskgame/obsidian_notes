```kotlin

/**
 * ViewModel to retrieve and update an item from the [ItemsRepository]'s data source.
 */
class ItemEditViewModel(
    savedStateHandle: SavedStateHandle,
    private val itemsRepository: ItemsRepository
) : ViewModel() {

    /**
     * Holds current item ui state
     */
    var itemUiState by mutableStateOf(ItemUiState())
        private set

    private val itemId: Int = checkNotNull(savedStateHandle[ItemEditDestination.itemIdArg])

    init {
        viewModelScope.launch {
            itemUiState = itemsRepository.getItemStream(itemId)
                .filterNotNull()
                .first()
                .toItemUiState(true)
        }
    }

    /**
     * Update the item in the [ItemsRepository]'s data source
     */
    suspend fun updateItem() {
        if (validateInput(itemUiState.itemDetails)) {
            itemsRepository.updateItem(itemUiState.itemDetails.toItem())
        }
    }

    /**
     * Updates the [itemUiState] with the value provided in the argument. This method also triggers
     * a validation for input values.
     */
    fun updateUiState(itemDetails: ItemDetails) {
        itemUiState =
            ItemUiState(itemDetails = itemDetails, isEntryValid = validateInput(itemDetails))
    }

    private fun validateInput(uiState: ItemDetails = itemUiState.itemDetails): Boolean {
        return with(uiState) {
            name.isNotBlank() && price.isNotBlank() && quantity.isNotBlank()
        }
    }
}

```

This `ItemEditViewModel` handles **updating** existing inventory items (U in [[CRUD]]), loading data from [[Room]] [[Database]] by ID and validating changes before persisting through the shared [[Repository]].

## Navigation ID → Database Lookup
```kotlin
private val itemId: Int = 
checkNotNull(savedStateHandle[ItemEditDestination.itemIdArg])
```

Extracts item ID from [[Navigation]] args (passed from [[ItemDetailsScreen]] → `"item_edit/{id}"`).  
**Flow**: Navigation route → `SavedStateHandle` → `itemId` → [[Database]] query.

## One-Time Data Load (vs. Continuous Stream)
```kotlin
init {
    viewModelScope.launch {
        itemUiState = itemsRepository.getItemStream(itemId)
            .filterNotNull()
            .first()           // ← Only first emission
            .toItemUiState(true)
    }
}
```

**Key difference from [[ItemDetailsViewModel]]**:

- [[ItemDetailsViewModel]]: Observes `Flow` continuously (reactive updates)
- `ItemEditViewModel`: Loads **once** with `.first()`, then allows editing

[[Room]]'s `getItemStream(itemId)` returns `Flow<Item?>`. Using `.first()` gets snapshot for editing; subsequent user changes stored in `itemUiState`, not tied to live DB updates during edit.

## Update Operation (Database Write)
```kotlin
suspend fun updateItem() {
    if (validateInput(itemUiState.itemDetails)) {
        itemsRepository.updateItem(itemUiState.itemDetails.toItem())
    }
}
```

**Complete flow**:
```
UI Save → validateInput() → ItemDetails.toItem() → 
Repository.updateItem() → Room @Update → SQLite row updated
↓
HomeViewModel.getAllItemsStream() + ItemDetailsViewModel.getItemStream(id) 
both auto-update → instant UI refresh across screens
```

## Same MVVM Layers as ItemEntryViewModel

Identical structure to [[ItemEntryViewModel]], just **update** vs **insert**:

```
ItemEditScreen TextFields ↔ itemUiState ↔ updateUiState() ↔ validateInput()
                                                            ↓
                                                        updateItem() → Repository
```

## Complete CRUD Coverage

This [[ViewModel]] completes full [[Database]] operations:

| Operation    | ViewModel                | [[Repository]]Class Method | Room [[Data Access Object\|DAO]]                  |
| ------------ | ------------------------ | -------------------------- | ------------------------------------------------- |
| **Read All** | [[HomeViewModel]]        | `getAllItemsStream()`      | `@Query("SQL `[[SQL SE\|SELECT]]` * FROM items")` |
| **Read One** | [[ItemDetailsViewModel]] | `getItemStream(id)`        | `@Query("SELECT * FROM items WHERE id = :id")`    |
| **Create**   | [[ItemEntryViewModel]]   | `insertItem(item)`         | `@Insert`                                         |
| **Update**   | **`ItemEditViewModel`**  | **`updateItem(item)`**     | **`@Update`**                                     |
| **Delete**   | `ItemDetailsViewModel`   | `deleteItem(id)`           | `@Delete`/`@Query DELETE`                         |

## Architecture Benefits

Single [[ItemsRepository]] [[Kotlin Object|Instance]] ensures ACID compliance:
- Edit item → ALL screens (Home list, other Details screens) update instantly
- No race conditions between concurrent edits ([[Room]] handles SQLite transactions)
- ViewModel survives config changes via SavedStateHandle

**Result**: Edit item → Save → Navigate back → HomeScreen shows updated values immediately. Pure reactive [[Model-View-ViewModel|MVVM]]—no manual synchronization needed.

## Production Improvement

The `.first()` load pattern works but could be enhanced:

```kotlin
// Better: Load once, then expose both current + pending state
private val _originalItem = MutableStateFlow<Item?>(null)
val currentItem = itemsRepository.getItemStream(itemId)  // Keep live for "Discard changes"
val pendingItem = combine(_originalItem, itemUiState) { ... }
```

This allows "Reset to original" functionality while maintaining edit [[State in Compose|State]].