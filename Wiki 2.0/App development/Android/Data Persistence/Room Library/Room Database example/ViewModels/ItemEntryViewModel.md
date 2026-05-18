```kotlin

/**
 * ViewModel to validate and insert items in the Room database.
 */
class ItemEntryViewModel(private val itemsRepository: ItemsRepository) : ViewModel() {

    /**
     * Holds current item ui state
     */
    var itemUiState by mutableStateOf(ItemUiState())
        private set

    /**
     * Updates the [itemUiState] with the value provided in the argument. This method also triggers
     * a validation for input values.
     */
    fun updateUiState(itemDetails: ItemDetails) {
        itemUiState =
            ItemUiState(itemDetails = itemDetails, isEntryValid = validateInput(itemDetails))
    }

    /**
     * Inserts an [Item] in the Room database
     */
    suspend fun saveItem() {
        if (validateInput()) {
            itemsRepository.insertItem(itemUiState.itemDetails.toItem())
        }
    }

    private fun validateInput(uiState: ItemDetails = itemUiState.itemDetails): Boolean {
        return with(uiState) {
            name.isNotBlank() && price.isNotBlank() && quantity.isNotBlank()
        }
    }
}

/**
 * Represents Ui State for an Item.
 */
data class ItemUiState(
    val itemDetails: ItemDetails = ItemDetails(),
    val isEntryValid: Boolean = false
)

data class ItemDetails(
    val id: Int = 0,
    val name: String = "",
    val price: String = "",
    val quantity: String = "",
)

/**
 * Extension function to convert [ItemUiState] to [Item]. If the value of [ItemDetails.price] is
 * not a valid [Double], then the price will be set to 0.0. Similarly if the value of
 * [ItemUiState] is not a valid [Int], then the quantity will be set to 0
 */
fun ItemDetails.toItem(): Item = Item(
    id = id,
    name = name,
    price = price.toDoubleOrNull() ?: 0.0,
    quantity = quantity.toIntOrNull() ?: 0
)

fun Item.formatedPrice(): String {
    return NumberFormat.getCurrencyInstance().format(price)
}

/**
 * Extension function to convert [Item] to [ItemUiState]
 */
fun Item.toItemUiState(isEntryValid: Boolean = false): ItemUiState = ItemUiState(
    itemDetails = this.toItemDetails(),
    isEntryValid = isEntryValid
)

/**
 * Extension function to convert [Item] to [ItemDetails]
 */
fun Item.toItemDetails(): ItemDetails = ItemDetails(
    id = id,
    name = name,
    price = price.toString(),
    quantity = quantity.toString()
)

```

This `ItemEntryViewModel` handles form input validation and new item insertion into the [[Room]] [[Database]], completing the **create** operation (C in [[CRUD]]) within the [[Model-View-ViewModel|MVVM]] [[Repository]] pattern established in prior code.

## Form State Management and Validation

The [[ViewModel]] maintains `itemUiState: ItemUiState` as mutable [[State in Compose|State]] that [[Jetpack Compose|Compose]] UI binds to directly:

- `updateUiState(itemDetails)` validates user input on every keystroke (`name.isNotBlank() && price.isNotBlank() && quantity.isNotBlank()`)
- `isEntryValid` immediately reflects validation result, enabling/disabling the Save button in UI    
- No [[Database]] access during typing—purely client-side validation prevents invalid writes


## Database Insert Operation

```kotlin
suspend fun saveItem() {
    if (validateInput()) {
        itemsRepository.insertItem(itemUiState.itemDetails.toItem())
    }
}
```

**Flow**: UI calls `viewModel.saveItem()` → [[Repository]] executes [[Room]] `@Insert(onConflict = OnConflictStrategy.REPLACE)` → SQLite row created → **[[HomeViewModel]]'s `getAllItemsStream()` automatically emits updated list** → HomeScreen refreshes instantly.

## Layer Transformation Pattern

Critical separation between UI and [[Database]] layers:

```
UI Layer: ItemDetails (String fields for TextFields)
       ↓ .toItem()
Domain:  Item (id: Int, name: String, price: Double, quantity: Int) ← Room @Entity
       ↓ repository.insertItem()
Database: Room table row
```

**Safe conversion**: `price.toDoubleOrNull() ?: 0.0` prevents crashes from malformed input while [[Logging]] invalid data as zero.

## Complete CRUD Architecture Integration

With prior code, this completes the full inventory workflow:

| Screen    | ViewModel                | Repository Call        | [[Room]] [[Data Access Object                  | DAO]] |
| --------- | ------------------------ | ---------------------- | ---------------------------------------------- | ----- |
| Home      | [[HomeViewModel]]        | `getAllItemsStream()`  | `@Query("`[[SQL SELECT]]` * FROM items")`      |       |
| Details   | [[ItemDetailsViewModel]] | `getItemStream(id)`    | `@Query("SELECT * FROM items WHERE id = :id")` |       |
| **Entry** | **`ItemEntryViewModel`** | **`insertItem(item)`** | **`@Insert`**                                  |       |
| Edit      | [[ItemEditViewModel]]    | `updateItem(item)`     | `@Update`                                      |       |

**Key benefit**: Single [[ItemsRepository]] [[Kotlin Object|Instance]] (from `AppDataContainer`) ensures all screens stay synchronized via Room's reactive Flows—no manual refresh or event buses needed.

## UI Integration Pattern

In [[ItemEntryScreen]] (not shown), you'd see:

```kotlin
// Two-way binding
var name by remember { mutableStateOf(viewModel.itemUiState.itemDetails.name) }
OutliningTextField(
    value = name,
    onValueChange = { 
        viewModel.updateUiState(itemDetails.copy(name = it))
        name = it 
    }
)
Button(
    enabled = viewModel.itemUiState.isEntryValid,
    onClick = { viewModel.saveItem(); navigateBack() }
)
```

Real-time validation + database insert through clean layers, following Google's recommended MVVM+Room pattern from [[InventoryApplication]] down to SQLite.