```kotlin

object HomeDestination : NavigationDestination {
    override val route = "home"
    override val titleRes = R.string.app_name
}

/**
 * Entry route for Home screen
 */
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HomeScreen(
    navigateToItemEntry: () -> Unit,
    navigateToItemUpdate: (Int) -> Unit,
    modifier: Modifier = Modifier,
    viewModel: HomeViewModel = viewModel(factory = AppViewModelProvider.Factory)
) {
    val homeUiState by viewModel.homeUiState.collectAsState()
    val scrollBehavior = TopAppBarDefaults.enterAlwaysScrollBehavior()

    Scaffold(
        modifier = modifier.nestedScroll(scrollBehavior.nestedScrollConnection),
        topBar = {
            InventoryTopAppBar(
                title = stringResource(HomeDestination.titleRes),
                canNavigateBack = false,
                scrollBehavior = scrollBehavior
            )
        },
        floatingActionButton = {
            FloatingActionButton(
                onClick = navigateToItemEntry,
                shape = MaterialTheme.shapes.medium,
                modifier = Modifier
                    .padding(
                        end = WindowInsets.safeDrawing.asPaddingValues()
                            .calculateEndPadding(LocalLayoutDirection.current)
                    )
            ) {
                Icon(
                    imageVector = Icons.Default.Add,
                    contentDescription = stringResource(R.string.item_entry_title)
                )
            }
        },
    ) { innerPadding ->
        HomeBody(
            itemList = homeUiState.itemList,
            onItemClick = navigateToItemUpdate,
            modifier = modifier.fillMaxSize(),
            contentPadding = innerPadding,
        )
    }
}

@Composable
private fun HomeBody(
    itemList: List<Item>,
    onItemClick: (Int) -> Unit,
    modifier: Modifier = Modifier,
    contentPadding: PaddingValues = PaddingValues(0.dp),
) {
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = modifier,
    ) {
        if (itemList.isEmpty()) {
            Text(
                text = stringResource(R.string.no_item_description),
                textAlign = TextAlign.Center,
                style = MaterialTheme.typography.titleLarge,
                modifier = Modifier.padding(contentPadding),
            )
        } else {
            InventoryList(
                itemList = itemList,
                onItemClick = { onItemClick(it.id) },
                contentPadding = contentPadding,
                modifier = Modifier.padding(horizontal = dimensionResource(id = R.dimen.padding_small))
            )
        }
    }
}

@Composable
private fun InventoryList(
    itemList: List<Item>,
    onItemClick: (Item) -> Unit,
    contentPadding: PaddingValues,
    modifier: Modifier = Modifier
) {
    LazyColumn(
        modifier = modifier,
        contentPadding = contentPadding
    ) {
        items(items = itemList, key = { it.id }) { item ->
            InventoryItem(item = item,
                modifier = Modifier
                    .padding(dimensionResource(id = R.dimen.padding_small))
                    .clickable { onItemClick(item) })
        }
    }
}

@Composable
private fun InventoryItem(
    item: Item, modifier: Modifier = Modifier
) {
    Card(
        modifier = modifier, elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
    ) {
        Column(
            modifier = Modifier.padding(dimensionResource(id = R.dimen.padding_large)),
            verticalArrangement = Arrangement.spacedBy(dimensionResource(id = R.dimen.padding_small))
        ) {
            Row(
                modifier = Modifier.fillMaxWidth()
            ) {
                Text(
                    text = item.name,
                    style = MaterialTheme.typography.titleLarge,
                )
                Spacer(Modifier.weight(1f))
                Text(
                    text = item.formatedPrice(),
                    style = MaterialTheme.typography.titleMedium
                )
            }
            Text(
                text = stringResource(R.string.in_stock, item.quantity),
                style = MaterialTheme.typography.titleMedium
            )
        }
    }
}
```

This `HomeScreen` [[Jetpack Compose|Compose]] UI consumes the reactive `homeUiState` from [[HomeViewModel]], displaying inventory items from [[Room]] [[Database]] in a scrollable list while enabling [[Navigation]] to entry/update screens.

## Navigation Architecture Integration

`HomeDestination` defines the screen's route (`"home"`) as a navigation destination, used in [[NavHost]] elsewhere in the app. The callbacks `navigateToItemEntry()` and `navigateToItemUpdate(Int)` handle routing to other screens via [[NavController]], passing item IDs for detail/edit views. This single-activity pattern keeps database-unaware UI composables decoupled from navigation logic.[](https://blog.mobcoder.com/android-navigation-architecture/)

## Reactive UI Consumption

```kotlin
`val homeUiState by viewModel.homeUiState.collectAsState()`
```

Automatically rebuilds the UI when [[Room]] [[Database]] emits new item [[Lists]] through the [[ViewModel]]'s [[StateFlow]]. `HomeBody` renders empty [[State in Compose|State]] or `LazyColumn` of items—efficient for large inventories since only visible items compose. Clicking items extracts `it.id` from the database-backed list, navigating to update screen with the ID.

## Complete MVVM Data Flow

```
`Room DB → Repository Flow → HomeViewModel StateFlow → collectAsState() → LazyColumn rebuilds`
```

**Key benefits**: No manual refresh needed. Adding items via [[ItemEntryViewModel]] (FAB click) instantly updates HomeScreen list. Updates/deletes propagate similarly. ViewModel survives navigation via [[AppViewModelProvider]].[[Factory]], preserving state across back/forth navigation.

## UI Layer Patterns

- **Scaffold** provides Material3 structure (TopAppBar, FAB positioned safely).
- **LazyColumn** with stable `key = { it.id }` optimizes recomposition—critical for [[Database]] lists.
- **InventoryItem** displays `Item` fields (`name`, `quantity`, `formatedPrice()`), unaware of data source.  
    Navigation triggers database operations in destination ViewModels, feeding back to HomeScreen reactively.

## Architecture Summary

**UI** observes state → **ViewModel** transforms [[Repository]] Flows → **Repository** (in container) queries [[Room]] on IO [[Dispatcher]] → **SQLite** persists locally. Unidirectional flow ensures database changes anywhere instantly reflect in all screens without coupling.