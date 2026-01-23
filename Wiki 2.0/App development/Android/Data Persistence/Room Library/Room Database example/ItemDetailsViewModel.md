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

This [[ItemDetailsScreen]] displays a single inventory item from [[Room]] [[Database]] while enabling sell/delete operations, maintaining reactive [[User Interface|UI]] updates through the shared `ItemDetailsViewModel` and [[Repository]] pattern.

## Database-Driven UI State

The screen collects [[ViewModel]]`.uiState (StateFlow<ItemDetailsUiState>)` which fetches item data by ID from [[Navigation]] args:

```kotlin
NavHost passes itemId → ItemDetailsViewModel(SavedStateHandle, repository) → 
repository.getItemStream(itemId) → Room @Query("SELECT * FROM items WHERE id = :id")
```

Any [[Database]] changes (quantity updates) automatically recompose the UI via [[Kotlin]] Flow reactivity, ensuring consistency across screens.

## Key Database Operations

**Sell Item**: `viewModel.reduceQuantityByOne()` calls [[Repository]]`.update(item.copy(quantity = quantity - 1))` on background thread. [[Room]]'s Flow immediately emits updated item to:

- Current screen (quantity decrements)
- HomeScreen list (via `getAllItemsStream()`)
- ItemDetails screens for other items (unaffected)

**Delete Item**: `viewModel.deleteItem()` executes [[Repository]]`.delete(item)` → [[Room]] `@Delete` or `@Query("DELETE FROM items WHERE id = :id")`. HomeScreen list auto-refreshes post-deletion.

## MVVM Architecture Flow

```
Navigation: HomeScreen clicks item → ItemDetailsScreen
            ↓
ViewModel:  Loads item by ID from SavedStateHandle.get<Int>("itemId")
            ↓
Repository: Room query on IO dispatcher → Flow<Item>
            ↓
Compose:    uiState.collectAsState() → automatic recomposition
```

## Configuration Change Safety Note

The code comments highlight a real risk: `rememberCoroutineScope().launch { viewModel.deleteItem() }` cancels during rapid rotations since it's composition-bound. Production fix uses [[ViewModelScope]]`.launch {}` in ViewModel for [[Database]] operations, ensuring completion despite UI recreation.

## Reactive UI Benefits

- **No manual refresh**: Sell/deletes instantly update HomeScreen via shared repository Flows
- **Config changes safe**: ViewModel + SavedStateHandle survives rotations/navigation
- **Single data source**: All screens use `AppDataContainer.`[[ItemsRepository]], preventing drift  
    This pattern scales to complex apps—add network sync to repository without UI changes.