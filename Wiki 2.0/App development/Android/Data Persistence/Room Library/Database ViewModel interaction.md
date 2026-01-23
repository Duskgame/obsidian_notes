To save the app's transient data and to also access the [[Database]], you need to update the [[ViewModel]]s. Your `ViewModel`s interact with the [[Database]] via the [[Data Access Object|DAO]] and provide data to the [[User Interface|UI]]. All [[Database]] operations need to be run away from the main UI thread; you do so with [[Coroutines]] and [`viewModelScope`](https://developer.android.com/topic/libraries/architecture/coroutines#viewmodelscope).

## UI state class walkthrough

Open the ui/item/[[ItemEntryViewModel]].kt file. The `ItemUiState` [[Data Class]] represents the [[UI State]] of an Item. The `ItemDetails` data class represents a single item.

The starter code provides you with three extension functions:

- The `ItemDetails.toItem()` extension [[Function]] converts the `ItemUiState` UI state [[Kotlin Object|Object]] to the `Item` [[Entity]] type.
- The `Item.toItemUiState()` extension function converts the `Item` [[Room]] [[Entity]] object to the `ItemUiState` UI state type.
- The `Item.toItemDetails()` extension function converts the `Item` [[Room]] [[Entity]] object to the `ItemDetails`.

```kotlin
// No need to copy, this is part of starter code
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
* Extension function to convert [ItemDetails] to [Item]. If the value of [ItemDetails.price] is
* not a valid [Double], then the price will be set to 0.0. Similarly if the value of
* [ItemDetails.quantity] is not a valid [Int], then the quantity will be set to 0
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

You use the above class in the view models to read and update the UI.

## Update ItemEntry ViewModel

In this task, you pass in the [[Repository]] to the [[ItemEntryViewModel]].kt file. You also save the item details entered in the **Add Item** screen into the database.

1. Notice the `validateInput()` private function in the [[ItemEntryViewModel]] class.

```kotlin
// No need to copy over, this is part of starter code
private fun validateInput(uiState: ItemDetails = itemUiState.itemDetails): Boolean {
    return with(uiState) {
        name.isNotBlank() && price.isNotBlank() && quantity.isNotBlank()
    }
}
```

The above function checks if the `name`, `price`, and `quantity` are empty. You use this function to verify user input before adding or updating the entity in the database.

2. Open the `ItemEntryViewModel` class and add a `private` default [[Kotlin Constructor|constructor]] [[Parameter]] of the type [[ItemsRepository]].

```kotlin
import com.example.inventory.data.ItemsRepository

class ItemEntryViewModel(private val itemsRepository: ItemsRepository) : ViewModel() {
}
```

3. Update the `initializer` for the item entry view model in the ui/[[AppViewModelProvider]].kt and pass in the [[Repository]] instance as a parameter.

```kotlin
object AppViewModelProvider {
    val Factory = viewModelFactory {
        // Other Initializers 
        // Initializer for ItemEntryViewModel
        initializer {
            ItemEntryViewModel(inventoryApplication().container.itemsRepository)
        }
        //...
    }
}
```

4. Go to the `ItemEntryViewModel.kt` file and at the end of the `ItemEntryViewModel` class and add a [[Suspend Function]] called `saveItem()` to insert an item into the [[Room]] database. This function adds the data to the database in a non-blocking way.

```kotlin
suspend fun saveItem() {
}
```

5. Inside the function, check if `itemUiState` is valid and convert it to `Item` type so Room can understand the data.
6. Call `insertItem()` on [[ItemsRepository]] and pass in the data. The UI calls this function to add Item details to the database.

```kotlin
suspend fun saveItem() {
    if (validateInput()) {
        itemsRepository.insertItem(itemUiState.itemDetails.toItem())
    }
}
```

You have now added all the required functions to add entities to the database. In the next task, you update the UI to use the above functions.

### ItemEntryBody() composable walkthrough

1. In the ui/item/[[ItemEntryScreen]].kt file, the `ItemEntryBody()` composable is partially implemented for you as part of the stater code. Look at the `ItemEntryBody()` composable in the [[ItemEntryScreen]]() function call.

```kotlin
// No need to copy over, part of the starter code
ItemEntryBody(
    itemUiState = viewModel.itemUiState,
    onItemValueChange = viewModel::updateUiState,
    onSaveClick = { },
    modifier = Modifier
        .padding(innerPadding)
        .verticalScroll(rememberScrollState())
        .fillMaxWidth()
)
```

2. Note that the UI state and the `updateUiState` [[Lambda]] are being passed as function parameters. Look at the function definition to see how the UI state is being updated.

```kotlin
// No need to copy over, part of the starter code
@Composable
fun ItemEntryBody(
    itemUiState: ItemUiState,
    onItemValueChange: (ItemUiState) -> Unit,
    onSaveClick: () -> Unit,
    modifier: Modifier = Modifier
) {
    Column(
        // ...
    ) {
        ItemInputForm(
             itemDetails = itemUiState.itemDetails,
             onValueChange = onItemValueChange,
             modifier = Modifier.fillMaxWidth()
         )
        Button(
             onClick = onSaveClick,
             enabled = itemUiState.isEntryValid,
             shape = MaterialTheme.shapes.small,
             modifier = Modifier.fillMaxWidth()
         ) {
             Text(text = stringResource(R.string.save_action))
         }
    }
}

```

You are displaying `ItemInputForm` and a **Save** button in this composable. In the `ItemInputForm()` composable, you are displaying three text fields. The **Save** is only enabled if text is entered in the text fields. The _`isEntryValid`_ value is true if the text in all the text fields is valid (not empty).

3. Take a look at the `ItemInputForm()` [[Composable function]] implementation and notice the `onValueChange` function parameter. You are updating the _`itemDetails`_ value with the value entered by the user in the text fields. By the time the **Save** button is enabled, `itemUiState.itemDetails` has the values that need to be saved.

```kotlin
// No need to copy over, part of the starter code
@Composable
fun ItemEntryBody(
    //...
) {
    Column(
        // ...
    ) {
        ItemInputForm(
             itemDetails = itemUiState.itemDetails,
             //...
         )
        //...
    }
}

```

```kotlin
// No need to copy over, part of the starter code
@Composable
fun ItemInputForm(
    itemDetails: ItemDetails,
    modifier: Modifier = Modifier,
    onValueChange: (ItemUiState) -> Unit = {},
    enabled: Boolean = true
) {
    Column(modifier = modifier.fillMaxWidth(), verticalArrangement = Arrangement.spacedBy(16.dp)) {
        OutlinedTextField(
            value = itemUiState.name,
            onValueChange = { onValueChange(itemDetails.copy(name = it)) },
            //...
        )
        OutlinedTextField(
            value = itemUiState.price,
            onValueChange = { onValueChange(itemDetails.copy(price = it)) },
            //...
        )
        OutlinedTextField(
            value = itemUiState.quantity,
            onValueChange = { onValueChange(itemDetails.copy(quantity = it)) },
            //...
        )
    }
}
```

## Add click listener to the Save button

To tie everything together, add a click handler to the **Save** button. Inside the click handler, you launch a coroutine and call `saveItem()` to save the data in the Room database.

1. In the [[ItemEntryScreen]].kt, inside the `ItemEntryScreen` composable function, create a `val` named [[CoroutineScope]] with the `rememberCoroutineScope()` composable function.

**Note:** The `rememberCoroutineScope()` is a composable function that returns a `[[CoroutineScope]]` bound to the composition where it's called. You can use the `rememberCoroutineScope()` composable function when you want to launch a coroutine outside of a composable and ensure the coroutine is canceled after the scope leaves the composition. You can use this function when you need to control the lifecycle of coroutines manually, for example, to cancel an animation whenever a user event happens.

```kotlin
import androidx.compose.runtime.rememberCoroutineScope

val coroutineScope = rememberCoroutineScope()
```

2. Update the _`ItemEntryBody`_`()` function call and launch a coroutine inside `onSaveClick` lambda.

```kotlin
ItemEntryBody(
   // ...
    onSaveClick = {
        coroutineScope.launch {
        }
    },
    modifier = modifier.padding(innerPadding)
)
```

3. Look at the `saveItem()` function implementation in the `ItemEntryViewModel.kt` file to check if `itemUiState` is valid, converting `itemUiState` to `Item` type, and inserting it in the database using [[ItemsRepository]].insertItem().

```kotlin
// No need to copy over, you have already implemented this as part of the Room implementation 

suspend fun saveItem() {
    if (validateInput()) {
        itemsRepository.insertItem(itemUiState.itemDetails.toItem())
    }
}
```

4. In the `ItemEntryScreen.kt`, inside the `ItemEntryScreen` composable function, inside the coroutine, call `viewModel.saveItem()` to save the item in the database.

```kotlin
ItemEntryBody(
    // ...
    onSaveClick = {
        coroutineScope.launch {
            viewModel.saveItem()
        }
    },
    //...
)
```

Notice that you did not use [[ViewModelScope]].launch() for `saveItem()` in the `ItemEntryViewModel.kt` file, but it is necessary for _`ItemEntryBody`_`()` when you call a [[Repository]] [[Kotlin Class Method|Method]]. You can only call suspend functions from a coroutine or another suspend function. The function `viewModel.saveItem()` is a suspend function.

5. Build and run your app.
6. Tap the **+** FAB.
7. In the **Add Item** screen, add the item details and tap **Save**. Notice that tapping the **Save** button does not close the **Add Item** screen.
8. In the `onSaveClick` lambda, add a call to `navigateBack()`after the call to `viewModel.saveItem()` to navigate back to the previous screen. Your `ItemEntryBody()` function looks like the following code:

```kotlin
ItemEntryBody(
    itemUiState = viewModel.itemUiState,
    onItemValueChange = viewModel::updateUiState,
    onSaveClick = {
        coroutineScope.launch {
            viewModel.saveItem()
            navigateBack()
        }
    },
    modifier = modifier.padding(innerPadding)
)
```

9. Run the app again and perform the same steps to enter and save the data. Notice that this time, the app navigates back to the **Inventory** screen.

This action saves the data, but you cannot see the inventory data in the app. In the next task, you use the [Database Inspector](https://developer.android.com/studio/inspect/database) to view the data you saved.