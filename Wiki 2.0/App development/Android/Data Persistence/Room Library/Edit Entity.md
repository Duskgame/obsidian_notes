### Populate text fields

If you run the app, go to the **Item Details** screen, and then click the FAB, you can see that the title of the screen now is **Edit Item**. However, all the text fields are empty. In this step, you populate the text fields in the **Edit Item** screen with the entity details.

1. In `ItemDetailsScreen.kt`, scroll to the `ItemDetailsScreen` composable.
2. In `FloatingActionButton()`, change the `onClick` argument to include `uiState.value.itemDetails.id`, which is the `id` of the selected entity. You use this `id` to retrieve the entity details.

```kotlin
FloatingActionButton(
    onClick = { navigateToEditItem(uiState.value.itemDetails.id) },
    modifier = /*...*/
)
```

3. In the `ItemEditViewModel` class, add an `init` block.

```kotlin
init {
}
```

4. Inside the `init` block, launch a coroutine with _`viewModelScope`_`.`_`launch`_.

```kotlin
import kotlinx.coroutines.launch

viewModelScope.launch { }
```

5. Inside the `launch` block, retrieve the entity details with `itemsRepository.getItemStream(itemId)`.

```kotlin
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.filterNotNull
import kotlinx.coroutines.flow.first


init {
    viewModelScope.launch {
        itemUiState = itemsRepository.getItemStream(itemId)
            .filterNotNull()
            .first()
            .toItemUiState(true)
    }
}
```

In this launch block, you add a filter to return a flow that only contains values that are not null. With `toItemUiState()`, you convert the `item` entity to `ItemUiState`. You pass the `actionEnabled` value as `true` to enable the **Save** button.

To resolve the `Unresolved reference: itemsRepository` error, you need to pass in the _`ItemsRepository`_ as a dependency to the view model.

6. Add a constructor parameter to the `ItemEditViewModel` class.

```kotlin
class ItemEditViewModel(
    savedStateHandle: SavedStateHandle,
    private val itemsRepository: ItemsRepository
)
```

7. In the `AppViewModelProvider.kt` file, in the `ItemEditViewModel` initializer, add the `ItemsRepository` object as an argument.

```kotlin
initializer {
    ItemEditViewModel(
        this.createSavedStateHandle(),
        inventoryApplication().container.itemsRepository
    )
}
```

8. Run the app.
9. Go to **Item Details** and tap FAB.
10. Notice that the fields populate with the item details.
11. Edit the stock quantity, or any other field, and tap **Save**.

Nothing happens! This is because you are not updating the entity in the app database.

### Update the entity using Room

In this final task, you add the final pieces of the code to implement the update functionality. You define the necessary functions in the ViewModel and use them in the `ItemEditScreen`.

1. In `ItemEditViewModel` class, add a function called `updateUiState()` that takes an `ItemUiState` object and returns nothing. This function updates the `itemUiState` with new values that the user enters.

```kotlin
fun updateUiState(itemDetails: ItemDetails) {
    itemUiState =
        ItemUiState(itemDetails = itemDetails, isEntryValid = validateInput(itemDetails))
}
```

In this function, you assign the passed in `itemDetails` to the `itemUiState` and update the `isEntryValid` value. The app enables the **Save** button if `itemDetails` is `true`. You set this value to `true` _only_ if the input that the user enters is valid.

2. Go to the `ItemEditScreen.kt` file.
3. In the `ItemEditScreen` composable, scroll down to the `ItemEntryBody()` function call.
4. Set the `onItemValueChange` argument value to the new function `updateUiState`.

```kotlin
ItemEntryBody(
	itemUiState = viewModel.itemUiState,
	onItemValueChange = viewModel::updateUiState,
	onSaveClick = { },
	modifier = Modifier
		.padding(
			start = innerPadding.calculateStartPadding(LocalLayoutDirection.current),
			end = innerPadding.calculateEndPadding(LocalLayoutDirection.current),
			top = innerPadding.calculateTopPadding()
		)
		.verticalScroll(rememberScrollState())
)
```

5. Run the app.
6. Go to the **Edit Item** screen.
7. Make one of the entity values empty so that it is invalid. Notice how the **Save** button disables automatically.

Go back to the `ItemEditViewModel` class and add a `suspend` function called `updateItem()` that takes nothing. You use this function to save the updated entity to the Room database.

9. Inside the `getUpdatedItemEntry()` function, add an `if` condition to validate the user input by using the function `validateInput()`.
10. Make a call to the `updateItem()` function on the `itemsRepository`, passing in the `itemUiState.itemDetails.`_`toItem`_`()`. Entities that can be added to the Room database need to be of the type `Item`. The completed function looks like the following:

```kotlin
suspend fun updateItem() {
    if (validateInput(itemUiState.itemDetails)) {
        itemsRepository.updateItem(itemUiState.itemDetails.toItem())
    }
}
```

11. Go back to the `ItemEditScreen` composable.You need a coroutine scope to call the `updateItem()` function. Create a val called `coroutineScope` and set it to `rememberCoroutineScope()`.

```kotlin
import androidx.compose.runtime.rememberCoroutineScope

val coroutineScope = rememberCoroutineScope()
```

12. In the _`ItemEntryBody()`_ function call, update the `onSaveClick` function argument to start a coroutine in the `coroutineScope`.
13. Inside the `launch` block, call `updateItem()` on the `viewModel` and navigate back.

```kotlin
import kotlinx.coroutines.launch

onSaveClick = {
    coroutineScope.launch {
        viewModel.updateItem()
        navigateBack()
    }
},
```

The completed _`ItemEntryBody()`_ function call looks like the following:

```kotlin
ItemEntryBody(
    itemUiState = viewModel.itemUiState,
    onItemValueChange = viewModel::updateUiState,
    onSaveClick = {
        coroutineScope.launch {
            viewModel.updateItem()
            navigateBack()
        }
    },
    modifier = modifier.padding(innerPadding)
)
```

Run the app and try editing inventory items. You are now able to edit any item in the Inventory app database.