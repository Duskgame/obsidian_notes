### Add a function in the `ViewModel`

1. In `ItemDetailsViewModel.kt`, inside the `ItemDetailsViewModel` class, add a function called `reduceQuantityByOne()` with no parameters.

```kotlin
fun reduceQuantityByOne() {
}
```

2. Inside the function, start a coroutine with `viewModelScope.launch{}`.

**Note:** You must run database operations inside a coroutine.

```kotlin
import kotlinx.coroutines.launch
import androidx.lifecycle.viewModelScope


viewModelScope.launch {
}
```

3. Inside the `launch` block, create a `val` called `currentItem` and set it to `uiState.value.toItem()`.

```kotlin
val currentItem = uiState.value.toItem()
```

The `uiState.value` is of the type **`ItemUiState`**. You convert it to the `Item` entity type with the extension function _`toItem`_`()`.

4. Add an `if` statement to check if the `quality` is greater than `0`.
5. Call `updateItem()` on `itemsRepository` and pass in the updated `currentItem`. Use `copy()` to update the `quantity` value so that the function looks like the following:

```kotlin
fun reduceQuantityByOne() {
    viewModelScope.launch {
        val currentItem = uiState.value.itemDetails.toItem()
        if (currentItem.quantity > 0) {
    itemsRepository.updateItem(currentItem.copy(quantity = currentItem.quantity - 1))
       }
    }
}
```

6. Go back to `ItemDetailsScreen.kt`.
7. In the `ItemDetailsScreen` composable, go to the `ItemDetailsBody()` function call.
8. In the `onSellItem` lambda, call `viewModel.reduceQuantityByOne()`.

```kotlin
ItemDetailsBody(
    itemUiState = uiState.value,
    onSellItem = { viewModel.reduceQuantityByOne() },
    onDelete = { },
    modifier = modifier.padding(innerPadding)
)
```

9. Run the app.
10. On the **Inventory** screen, click a list element. When the **Item Details** screen displays, tap the **Sell** and notice that the quantity value decreases by one.
11. In the **Item Details** screen, continuously tap the **Sell** button until the quantity is zero.

**Tip:** To save time, you might want to use an item for this task with a low quantity. If none of your items have a low quantity, you can create a new one with a low quantity.

After the quantity reaches zero, tap **Sell** again. There is no visual change because the function `reduceQuantityByOne()` checks if the quantity is greater than zero before updating the quantity.

To give users better feedback, you might want to disable the **Sell** button when there is no item to sell.

12. In the `ItemDetailsViewModel` class, set `outOfStock` value based on the **`it`**`.quantity` in the `map` transformation.
13. Run your app. Notice that the app disables the **Sell** button when the quantity in stock is zero.

(Needed to update button logic with uistate. outOfStock)