```kotlin

object ItemEntryDestination : NavigationDestination {
    override val route = "item_entry"
    override val titleRes = R.string.item_entry_title
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ItemEntryScreen(
    navigateBack: () -> Unit,
    onNavigateUp: () -> Unit,
    canNavigateBack: Boolean = true,
    viewModel: ItemEntryViewModel = viewModel(factory = AppViewModelProvider.Factory)
) {
    val coroutineScope = rememberCoroutineScope()
    Scaffold(
        topBar = {
            InventoryTopAppBar(
                title = stringResource(ItemEntryDestination.titleRes),
                canNavigateBack = canNavigateBack,
                navigateUp = onNavigateUp
            )
        }
    ) { innerPadding ->
        ItemEntryBody(
            itemUiState = viewModel.itemUiState,
            onItemValueChange = viewModel::updateUiState,
            onSaveClick = {
                // Note: If the user rotates the screen very fast, the operation may get cancelled
                // and the item may not be saved in the Database. This is because when config
                // change occurs, the Activity will be recreated and the rememberCoroutineScope will
                // be cancelled - since the scope is bound to composition.
                coroutineScope.launch {
                    viewModel.saveItem()
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
                .fillMaxWidth()
        )
    }
}

@Composable
fun ItemEntryBody(
    itemUiState: ItemUiState,
    onItemValueChange: (ItemDetails) -> Unit,
    onSaveClick: () -> Unit,
    modifier: Modifier = Modifier
) {
    Column(
        modifier = modifier.padding(dimensionResource(id = R.dimen.padding_medium)),
        verticalArrangement = Arrangement.spacedBy(dimensionResource(id = R.dimen.padding_large))
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

@Composable
fun ItemInputForm(
    itemDetails: ItemDetails,
    modifier: Modifier = Modifier,
    onValueChange: (ItemDetails) -> Unit = {},
    enabled: Boolean = true
) {
    Column(
        modifier = modifier,
        verticalArrangement = Arrangement.spacedBy(dimensionResource(id = R.dimen.padding_medium))
    ) {
        OutlinedTextField(
            value = itemDetails.name,
            onValueChange = { onValueChange(itemDetails.copy(name = it)) },
            label = { Text(stringResource(R.string.item_name_req)) },
            colors = OutlinedTextFieldDefaults.colors(
                focusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                unfocusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                disabledContainerColor = MaterialTheme.colorScheme.secondaryContainer,
            ),
            modifier = Modifier.fillMaxWidth(),
            enabled = enabled,
            singleLine = true
        )
        OutlinedTextField(
            value = itemDetails.price,
            onValueChange = { onValueChange(itemDetails.copy(price = it)) },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Decimal),
            label = { Text(stringResource(R.string.item_price_req)) },
            colors = OutlinedTextFieldDefaults.colors(
                focusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                unfocusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                disabledContainerColor = MaterialTheme.colorScheme.secondaryContainer,
            ),
            leadingIcon = { Text(Currency.getInstance(Locale.getDefault()).symbol) },
            modifier = Modifier.fillMaxWidth(),
            enabled = enabled,
            singleLine = true
        )
        OutlinedTextField(
            value = itemDetails.quantity,
            onValueChange = { onValueChange(itemDetails.copy(quantity = it)) },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
            label = { Text(stringResource(R.string.quantity_req)) },
            colors = OutlinedTextFieldDefaults.colors(
                focusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                unfocusedContainerColor = MaterialTheme.colorScheme.secondaryContainer,
                disabledContainerColor = MaterialTheme.colorScheme.secondaryContainer,
            ),
            modifier = Modifier.fillMaxWidth(),
            enabled = enabled,
            singleLine = true
        )
        if (enabled) {
            Text(
                text = stringResource(R.string.required_fields),
                modifier = Modifier.padding(start = dimensionResource(id = R.dimen.padding_medium))
            )
        }
    }
}

```

This `ItemEntryScreen` provides the UI for creating new inventory items with real-time form validation, connecting user input directly to [[Room]] [[Database]] insertion through the [[ItemEntryViewModel]].

## Two-Way Data Binding Pattern

The form uses classic [[Model-View-ViewModel|MVVM]] two-way binding between UI and [[ViewModel]] [[State in Compose|State]]:
```kotlin
OutlinedTextField(
    value = itemDetails.name,
    onValueChange = { onValueChange(itemDetails.copy(name = it)) }
)
```

Each keystroke calls `viewModel::updateUiState(itemDetails.copy(name = it))`, which:

1. Triggers `validateInput()` (non-blank checks)
2. Updates `itemUiState.isEntryValid`
3. Enables/disables Save button reactively

**No [[Database]] access during typing**—validation stays in memory for instant UI feedback.

## Database Save Flow

Save button triggers the complete **Create** operation (C in [[CRUD]]):

```
UI: onSaveClick() → coroutineScope.launch {
    ↓
ViewModel: saveItem() → validateInput() → itemsRepository.insertItem()
    ↓
Repository: Room @Insert(onConflict = OnConflictStrategy.REPLACE) 
    ↓
Database: New row in items table, auto-generates ID
    ↓
HomeViewModel: getAllItemsStream() emits updated list → HomeScreen auto-refreshes
```


## Reactive State Synchronization

Critical architecture connection—**single source of truth**:
```
ItemEntryScreen observes → itemUiState (ViewModel) ← ItemsRepository (shared)
                                                      ↓
                                                 HomeScreen observes → getAllItemsStream()
```

When new item saves, [[Room]]'s Flow automatically notifies **all observing screens** (Home list shows new item instantly).

## Same Configuration Warning as DetailsScreen

Identical issue to [[ItemDetailsScreen]]: `rememberCoroutineScope.launch { viewModel.saveItem() }` cancels during screen rotation.

```
Problem: Composition scope → UI recreation cancels insert before Room completes
Solution: Move save to ViewModel: viewModelScope.launch { repository.insertItem() }
```

## Form Layer Separation

Clean [[Separation of concerns]]:

- **ItemInputForm**: Pure UI, handles TextField keyboard types (Decimal/Number), currency symbols
- **ItemEntryBody**: Orchestrates form + Save button, uses `isEntryValid` from ViewModel
- **[[ItemEntryViewModel]]**: Business logic (validation) + DB access
- **[[Repository]]**: [[Database]] [[OOP|abstraction]]

## Complete Architecture Picture

```
Navigation → ItemEntryScreen → ItemEntryViewModel → ItemsRepository → Room DAO
     ↑ Save success auto-updates                                        ↑
HomeViewModel ← getAllItemsStream() ← Room Flow emissions ← Items table
```

**Result**: User adds item → taps Save → navigates back → HomeScreen shows new item immediately. No event buses, callbacks, or manual refresh needed—pure reactive MVVM with [[Room]] Flows.

The pattern scales perfectly: add [[ItemEditScreen]] with identical structure but `updateItem()` instead of `insertItem()`.