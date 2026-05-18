In this task, you read and display the [[Entity]] details on the **Item Details** screen. You use the item [[UI State]], such as name, price, and quantity from the inventory app [[Database]] and display them on the **Item Details** screen with the [[ItemDetailsScreen]] composable. The [[ItemDetailsScreen]] composable [[Function]] is prewritten for you and contains three Text composables that display the item details.

### ui/item/ItemDetailsScreen.kt

This screen is part of the starter code and displays the details of the items, which you see in a later codelab. You do not work on this screen in this codelab. The [[ItemDetailsViewModel]].kt is the corresponding [[ViewModel]] for this screen.

1. In the `HomeScreen` composable function, notice the `HomeBody()` function call. `navigateToItemUpdate` is being passed to the `onItemClick` [[Parameter]], which gets called when you click on any item in your list.

```kotlin
// No need to copy over 
HomeBody(
    itemList = homeUiState.itemList,
    onItemClick = navigateToItemUpdate,
    modifier = modifier
        .padding(innerPadding)
        .fillMaxSize()
)
```

2. Open [[User Interface|UI]]/navigation/[[InventoryNavGraph]].kt and notice the `navigateToItemUpdate` parameter in the `HomeScreen` composable. This parameter specifies the destination for [[Navigation]] as the item details screen.

```kotlin
// No need to copy over 
HomeScreen(
    navigateToItemEntry = { navController.navigate(ItemEntryDestination.route) },
    navigateToItemUpdate = {
        navController.navigate("${ItemDetailsDestination.route}/${it}")
   }
```

This part of the `onItemClick` functionality is already implemented for you. When you click the list item, the app navigates to the item details screen.

3. Click any item in the inventory list to see the item details screen with empty fields.

To fill the text fields with item details, you need to collect the UI state in [[ItemDetailsScreen]]().

4. In `UI/Item/ItemDetailsScreen.kt`, add a new parameter to the `ItemDetailsScreen` composable of the type [[ItemDetailsViewModel]] and use the [[Factory]] [[Kotlin Class Method|Method]] to initialize it.

```kotlin
import androidx.lifecycle.viewmodel.compose.viewModel
import com.example.inventory.ui.AppViewModelProvider

@Composable
fun ItemDetailsScreen(
    navigateToEditItem: (Int) -> Unit,
    navigateBack: () -> Unit,
    modifier: Modifier = Modifier,
    viewModel: ItemDetailsViewModel = viewModel(factory = AppViewModelProvider.Factory)
)
```

5. Inside the `ItemDetailsScreen()` composable, create a `val` called `uiState` to collect the UI state. Use `collectAsState()` to collect `uiState` [[StateFlow]] and represent its latest value via [[State in Compose|State]]. [[Android]] Studio displays an unresolved reference error.

```kotlin
import androidx.compose.runtime.collectAsState

val uiState = viewModel.uiState.collectAsState()
```

6. To resolve the error, create a `val` called `uiState` of the type `StateFlow<ItemDetailsUiState>` in the [[ItemDetailsViewModel]] [[Kotlin Class|Class]].
7. Retrieve the data from the item [[Repository]] and map it to `ItemDetailsUiState` using the extension [[Function]] `toItemDetails()`. The extension [[Function]] `Item.toItemDetails()` is already written for you as part of the starter code.

```kotlin
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.SharingStarted
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.filterNotNull
import kotlinx.coroutines.flow.map
import kotlinx.coroutines.flow.stateIn

val uiState: StateFlow<ItemDetailsUiState> =
         itemsRepository.getItemStream(itemId)
             .filterNotNull()
             .map {
                 ItemDetailsUiState(itemDetails = it.toItemDetails())
             }.stateIn(
                 scope = viewModelScope,
                 started = SharingStarted.WhileSubscribed(TIMEOUT_MILLIS),
                 initialValue = ItemDetailsUiState()
             )
```

8. Pass [[ItemsRepository]] into the `ItemDetailsViewModel` to resolve the `Unresolved reference:` [[ItemsRepository]] error.

```kotlin
class ItemDetailsViewModel(
    savedStateHandle: SavedStateHandle,
    private val itemsRepository: ItemsRepository
    ) : ViewModel() {
```

9. In `ui/`[[AppViewModelProvider]]`.kt`, update the initializer for `ItemDetailsViewModel` as shown in the following code snippet:

```kotlin
initializer {
    ItemDetailsViewModel(
        this.createSavedStateHandle(),
        inventoryApplication().container.itemsRepository
    )
}
```

10. Go back to the `ItemDetailsScreen.kt` and notice the error in the `ItemDetailsScreen()` composable is resolved.
11. In the `ItemDetailsScreen()` composable, update the `ItemDetailsBody()` function call and pass in `uiState.value` to `itemUiState` argument.

```kotlin
ItemDetailsBody(
    itemUiState = uiState.value,
    onSellItem = {  },
    onDelete = { },
    modifier = modifier.padding(innerPadding)
)
```

12. Observe the implementations of `ItemDetailsBody()` and `ItemInputForm()`. You are passing the current selected `item` from `ItemDetailsBody()` to `ItemDetails()`.

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
       //...
    ) {
        var deleteConfirmationRequired by rememberSaveable { mutableStateOf(false) }
        ItemDetails(
             item = itemDetailsUiState.itemDetails.toItem(), modifier = Modifier.fillMaxWidth()
         )

      //...
    }
```

13. Run the app. When you click any list element on the **Inventory** screen, the **Item Details** screen displays.
14. Notice that the screen is not blank anymore. It displays the [[Entity]] details retrieved from the inventory database.
Tap the **Sell** button. Nothing happens!


