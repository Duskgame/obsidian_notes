### Add delete function in the `ItemDetailsViewModel`

1. In [[ItemDetailsViewModel]], add a new [[Function]] called `deleteItem()` that takes no parameters and returns nothing.
2. Inside the `deleteItem()` function, add an [[ItemsRepository]].deleteItem() function call and pass in `uiState.value.`_`toItem`_`()`.

```kotlin
suspend fun deleteItem() {
    itemsRepository.deleteItem(uiState.value.itemDetails.toItem())
}
```

In this function, you convert the `uiState` from `itemDetails` type to `Item` [[Entity]] type using the _`toItem`_`()` extension function.

3. In the [[User Interface|UI]]/item/[[ItemDetailsScreen]] composable, add a `val` called [[CoroutineScope]] and set it to `rememberCoroutineScope()`. This approach returns a coroutine scope bound to the composition where it's called ([[ItemDetailsScreen]] composable).

```kotlin
import androidx.compose.runtime.rememberCoroutineScope


val coroutineScope = rememberCoroutineScope()
```

4. Scroll to the `ItemDetailsBody()`function.
5. Launch a coroutine with `coroutineScope` inside the `onDelete` [[Lambda]].
6. Inside the `launch` block, call the `deleteItem()` [[Kotlin Class Method|Method]] on [[ViewModel]].

```kotlin
import kotlinx.coroutines.launch

ItemDetailsBody(
    itemUiState = uiState.value,
    onSellItem = { viewModel.reduceQuantityByOne() },
    onDelete = {
        coroutineScope.launch {
           viewModel.deleteItem()
        }
    },
    modifier = modifier.padding(innerPadding)
)
```

7. After deleting the item, navigate back to the inventory screen.
8. Call `navigateBack()` after the `deleteItem()` function call.

```kotlin
onDelete = {
    coroutineScope.launch {
        viewModel.deleteItem()
        navigateBack()
    }

```

9. Still within the [[ItemDetailsScreen]].kt file, scroll to the `ItemDetailsBody()`function.

This function is part of the starter code. This composable displays an [[Alert Dialog]] to get the user's confirmation before deleting the item and calls the `deleteItem()` function when you tap **Yes**.

```kotlin
// No need to copy over

@Composable
private fun ItemDetailsBody(
    itemUiState: ItemUiState,
    onSellItem: () -> Unit,
    onDelete: () -> Unit,
    modifier: Modifier = Modifier
) {
    Column(
        /*...*/
    ) {
        //...
       
        if (deleteConfirmationRequired) {
            DeleteConfirmationDialog(
                onDeleteConfirm = {
                    deleteConfirmationRequired = false
                    onDelete()
                },
                //...
            )
        }
    }
}
```

10. Run the app.
11. Select a list element on the **Inventory** screen.
12. In the **Item Details** screen, tap **Delete**.
13. Tap **Yes** in the alert dialog, and the app navigates back to the **Inventory** screen.
14. Confirm that the [[Entity]] you deleted is no longer in the app [[Database]].

Congratulations on implementing the delete feature!