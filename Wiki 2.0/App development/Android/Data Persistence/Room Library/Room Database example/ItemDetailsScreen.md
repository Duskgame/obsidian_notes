```kotlin

object ItemDetailsDestination : NavigationDestination {
    override val route = "item_details"
    override val titleRes = R.string.item_detail_title
    const val itemIdArg = "itemId"
    val routeWithArgs = "$route/{$itemIdArg}"
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ItemDetailsScreen(
    navigateToEditItem: (Int) -> Unit,
    navigateBack: () -> Unit,
    modifier: Modifier = Modifier,
    viewModel: ItemDetailsViewModel = viewModel(factory = AppViewModelProvider.Factory)
) {
    val uiState = viewModel.uiState.collectAsState()
    val coroutineScope = rememberCoroutineScope()
    Scaffold(
        topBar = {
            InventoryTopAppBar(
                title = stringResource(ItemDetailsDestination.titleRes),
                canNavigateBack = true,
                navigateUp = navigateBack
            )
        },
        floatingActionButton = {
            FloatingActionButton(
                onClick = { navigateToEditItem(uiState.value.itemDetails.id) },
                shape = MaterialTheme.shapes.medium,
                modifier = Modifier
                    .padding(
                        end = WindowInsets.safeDrawing.asPaddingValues()
                            .calculateEndPadding(LocalLayoutDirection.current)
                    )
            ) {
                Icon(
                    imageVector = Icons.Default.Edit,
                    contentDescription = stringResource(R.string.edit_item_title),
                )
            }
        },
        modifier = modifier,
    ) { innerPadding ->
        ItemDetailsBody(
            itemDetailsUiState = uiState.value,
            onSellItem = { viewModel.reduceQuantityByOne() },
            onDelete = {
                // Note: If the user rotates the screen very fast, the operation may get cancelled
                // and the item may not be deleted from the Database. This is because when config
                // change occurs, the Activity will be recreated and the rememberCoroutineScope will
                // be cancelled - since the scope is bound to composition.
                coroutineScope.launch {
                    viewModel.deleteItem()
                    navigateBack()
                }
            },
            modifier = Modifier
                .padding(
                    start = innerPadding.calculateStartPadding(LocalLayoutDirection.current),
                    top = innerPadding.calculateTopPadding(),
                    end = innerPadding.calculateEndPadding(LocalLayoutDirection.current),
                )
                .verticalScroll(rememberScrollState())
        )
    }
}

@Composable
private fun ItemDetailsBody(
    itemDetailsUiState: ItemDetailsUiState,
    onSellItem: () -> Unit,
    onDelete: () -> Unit,
    modifier: Modifier = Modifier
) {
    Column(
        modifier = modifier.padding(dimensionResource(id = R.dimen.padding_medium)),
        verticalArrangement = Arrangement.spacedBy(dimensionResource(id = R.dimen.padding_medium))
    ) {
        var deleteConfirmationRequired by rememberSaveable { mutableStateOf(false) }
        ItemDetails(
            item = itemDetailsUiState.itemDetails.toItem(), modifier = Modifier.fillMaxWidth()
        )
        Button(
            onClick = onSellItem,
            modifier = Modifier.fillMaxWidth(),
            shape = MaterialTheme.shapes.small,
            enabled = !itemDetailsUiState.outOfStock
        ) {
            Text(stringResource(R.string.sell))
        }
        OutlinedButton(
            onClick = { deleteConfirmationRequired = true },
            shape = MaterialTheme.shapes.small,
            modifier = Modifier.fillMaxWidth()
        ) {
            Text(stringResource(R.string.delete))
        }
        if (deleteConfirmationRequired) {
            DeleteConfirmationDialog(
                onDeleteConfirm = {
                    deleteConfirmationRequired = false
                    onDelete()
                },
                onDeleteCancel = { deleteConfirmationRequired = false },
                modifier = Modifier.padding(dimensionResource(id = R.dimen.padding_medium))
            )
        }
    }
}


@Composable
fun ItemDetails(
    item: Item, modifier: Modifier = Modifier
) {
    Card(
        modifier = modifier, colors = CardDefaults.cardColors(
            containerColor = MaterialTheme.colorScheme.primaryContainer,
            contentColor = MaterialTheme.colorScheme.onPrimaryContainer
        )
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .padding(dimensionResource(id = R.dimen.padding_medium)),
            verticalArrangement = Arrangement.spacedBy(dimensionResource(id = R.dimen.padding_medium))
        ) {
            ItemDetailsRow(
                labelResID = R.string.item,
                itemDetail = item.name,
                modifier = Modifier.padding(
                    horizontal = dimensionResource(
                        id = R.dimen
                            .padding_medium
                    )
                )
            )
            ItemDetailsRow(
                labelResID = R.string.quantity_in_stock,
                itemDetail = item.quantity.toString(),
                modifier = Modifier.padding(
                    horizontal = dimensionResource(
                        id = R.dimen
                            .padding_medium
                    )
                )
            )
            ItemDetailsRow(
                labelResID = R.string.price,
                itemDetail = item.formatedPrice(),
                modifier = Modifier.padding(
                    horizontal = dimensionResource(
                        id = R.dimen
                            .padding_medium
                    )
                )
            )
        }

    }
}

@Composable
private fun ItemDetailsRow(
    @StringRes labelResID: Int, itemDetail: String, modifier: Modifier = Modifier
) {
    Row(modifier = modifier) {
        Text(text = stringResource(labelResID))
        Spacer(modifier = Modifier.weight(1f))
        Text(text = itemDetail, fontWeight = FontWeight.Bold)
    }
}

@Composable
private fun DeleteConfirmationDialog(
    onDeleteConfirm: () -> Unit, onDeleteCancel: () -> Unit, modifier: Modifier = Modifier
) {
    AlertDialog(
        onDismissRequest = { /* Do nothing */ },
        title = { Text(stringResource(R.string.attention)) },
        text = { Text(stringResource(R.string.delete_question)) },
        modifier = modifier,
        dismissButton = {
            TextButton(onClick = onDeleteCancel) {
                Text(text = stringResource(R.string.no))
            }
        },
        confirmButton = {
            TextButton(onClick = onDeleteConfirm) {
                Text(text = stringResource(R.string.yes))
            }
        })
}

```

This code defines the “Item details” [[Navigation]] destination and the entire [[User Interface|UI]] for viewing, selling, and deleting a single item, wired into the [[Room]]‑backed architecture through [[ItemDetailsViewModel]] and the shared [[ItemsRepository]].[](https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room)​

## Destination and ID → DB lookup

`ItemDetailsDestination` defines:

- A base route: `"item_details"`.
- An argument name: `itemIdArg = "itemId"`.
- A pattern `routeWithArgs = "item_details/{itemId}"`.

In the navigation graph, when you navigate to `"item_details/5"`, that `5` becomes the `itemId` argument. The [[ItemDetailsViewModel]] receives this ID from a `SavedStateHandle` and uses it to query the specific row from the [[Room]] [[Database]] via [[ItemsRepository]] (for example [[Repository]]`.getItemStream(itemId)`). This connects [[Navigation]] [[Arguments]] directly to a [[Database]] record.[](https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room)​

## ItemDetailsScreen and ViewModel connection

`ItemDetailsScreen` gets its [[ItemDetailsViewModel]] from the shared [[Factory]] [[AppViewModelProvider]].Factory, which injects the app‑wide [[Repository]] (built in [[InventoryApplication]] and `AppDataContainer`). The pipeline is:

- UI calls: `val uiState = viewModel.uiState.collectAsState()`.
- `uiState` is a `StateFlow<ItemDetailsUiState>` produced by the [[ViewModel]].
- The [[ViewModel]] observes a Flow from [[Room]] (via the [[Repository]]), so any DB change for this item automatically updates `uiState`, causing the Composable to recompose.

This is classic [[Model-View-ViewModel|MVVM]]:  
UI (Composable) → ViewModel → Repository → [[Data Access Object|DAO]] → Room (SQLite), and back as Flows/StateFlows.

## Sell and delete: writing to the database

Two user actions modify the [[Database]] through the ViewModel:

- **Sell (reduce quantity)**  
    `onSellItem = { viewModel.reduceQuantityByOne() }`  
    Inside the ViewModel, this typically:
    - Gets the current `Item` from `uiState`.
    - Calls a repository [[Kotlin Class Method|Method]] like `repository.update(item.copy(quantity = quantity - 1))` on a background [[Dispatcher]].
    - Room updates the row; the query Flow emits a new `Item`; `uiState` updates; UI shows new quantity.
- **Delete item**  
    `onDelete` launches a coroutine in `rememberCoroutineScope` and calls `viewModel.deleteItem()` and then `navigateBack()`.  
    Inside the ViewModel, `deleteItem()` usually calls `repository.delete(item)` (Room `@Delete`/`DELETE FROM ...`). Once the row is removed, any screen observing the list of items (e.g., [[HomeViewModel]].getAllItemsStream()) is updated, so the deleted item disappears everywhere.

The comment warns that using `rememberCoroutineScope` ties the deletion coroutine to the composition; a fast rotation can cancel the coroutine before Room finishes the delete. Architecturally, the safer pattern is doing DB work in [[ViewModelScope]] so it survives configuration changes.

## ItemDetailsBody: mapping state to UI

`ItemDetailsBody` receives a pure [[UI State]] [[Kotlin Object|Object]]:

```kotlin
ItemDetails(
    item = itemDetailsUiState.itemDetails.toItem()
)
```

Here:

- `itemDetailsUiState` is produced by the ViewModel from Room’s Flow.
- `itemDetailsUiState.outOfStock` controls whether the “Sell” button is enabled, derived from database data (e.g., quantity <= 0).
- Clicking Sell/Delete triggers the callbacks that ultimately call repository methods.

So this Composable layer is **stateless regarding business logic**: it just renders data and emits events; the ViewModel coordinates with the repository/DB.

## ItemDetails and rows: presentational only

`ItemDetails` and `ItemDetailsRow` are purely presentational:

- They display `Item.name`, `Item.quantity`, and `Item.formatedPrice()`.
- `Item` itself is typically the Room [[Entity]] or a domain model derived from it.
- No direct database calls happen here; they depend entirely on the data provided by the ViewModel.

This aligns with clean architecture: lower layers (DB, [[Data Access Object|DAO]], repository) never leak into UI components; instead, they surface as simple models and [[State in Compose|State]] objects.

## DeleteConfirmationDialog and consistency

`DeleteConfirmationDialog` is a UI guard around a destructive DB operation:

- It ensures the user explicitly confirms before calling `onDelete()`.
- `onDelete()` eventually triggers `viewModel.deleteItem()` → repository → [[Room]] delete.

From an architecture perspective, this is a good separation:

- Dialog only knows “call `onDeleteConfirm`”.
- All real data changes (and therefore database writes) remain in the ViewModel/repository layers.

In summary, this code is the “details screen” of a Room‑based inventory app: navigation passes an item ID, the ViewModel uses that ID to stream a single record from the database, and UI actions call back into the ViewModel to perform `update` and `delete` operations through the repository, keeping all screens in sync with the underlying Room database.