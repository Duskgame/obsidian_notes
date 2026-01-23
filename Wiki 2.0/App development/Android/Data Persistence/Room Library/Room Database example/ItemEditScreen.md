```kotlin

object ItemEditDestination : NavigationDestination {
    override val route = "item_edit"
    override val titleRes = R.string.edit_item_title
    const val itemIdArg = "itemId"
    val routeWithArgs = "$route/{$itemIdArg}"
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ItemEditScreen(
    navigateBack: () -> Unit,
    onNavigateUp: () -> Unit,
    modifier: Modifier = Modifier,
    viewModel: ItemEditViewModel = viewModel(factory = AppViewModelProvider.Factory)
) {
    val coroutineScope = rememberCoroutineScope()
    Scaffold(
        topBar = {
            InventoryTopAppBar(
                title = stringResource(ItemEditDestination.titleRes),
                canNavigateBack = true,
                navigateUp = onNavigateUp
            )
        },
        modifier = modifier
    ) { innerPadding ->
        ItemEntryBody(
            itemUiState = viewModel.itemUiState,
            onItemValueChange = viewModel::updateUiState,
            onSaveClick = {
                // Note: If the user rotates the screen very fast, the operation may get cancelled
                // and the item may not be updated in the Database. This is because when config
                // change occurs, the Activity will be recreated and the rememberCoroutineScope will
                // be cancelled - since the scope is bound to composition.
                coroutineScope.launch {
                    viewModel.updateItem()
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
```

This `ItemEditScreen` provides the [[User Interface|UI]] for editing existing inventory items, reusing the `ItemEntryBody` form component while triggering **[[Room]] [[Database]] UPDATE** operations through [[ItemEditViewModel]].

## Reused Form Components for Consistency

Key architecture decision—**shared UI components** across create/update flows:
```kotlin
ItemEntryBody(  // Same component as ItemEntryScreen!
    itemUiState = viewModel.itemUiState,
    onItemValueChange = viewModel::updateUiState,
    onSaveClick = { viewModel.updateItem() }
)
```

**Benefits**:

- Identical UX for "add new" vs "edit existing"
- Single form validation logic ([[ItemEntryViewModel]] vs [[ItemEditViewModel]])
- DRY principle across [[CRUD]] operations

## Database Update Flow

Save button triggers **Update** operation (U in CRUD):

```
UI Save → coroutineScope.launch → viewModel.updateItem() → 
validateInput() → ItemDetails.toItem() → 
ItemsRepository.updateItem() → Room @Update(entity = Item::class) → 
SQLite WHERE id = ? UPDATE
↓
HomeViewModel.getAllItemsStream() + ItemDetailsViewModel.getItemStream(id) 
instantly reflect changes → ALL screens auto-update
```

## Complete Navigation-to-Database Workflow

```
HomeScreen item click → ItemDetailsScreen → Edit FAB → "item_edit/{id}"
     ↓ Navigation extracts itemId
ItemEditViewModel(SavedStateHandle) → itemId → getItemStream(id).first()
     ↓ Loads into itemUiState for editing
User edits → updateUiState() → real-time validation
Save → updateItem() → Repository → Room UPDATE

```

## Same Configuration Change Risk

Identical issue as other screens: `rememberCoroutineScope` cancels during rotation:

```kotlin
// PROBLEM: Composition-bound scope
coroutineScope.launch { viewModel.updateItem() }

// SOLUTION: Move to ViewModel
class ItemEditViewModel {
    fun save() = viewModelScope.launch { updateItem() }
}
```

## Full CRUD Architecture Integration

This screen completes the inventory app's database operations:

| Screen    | Operation  | [[Repository]] Call   | [[Room]] Annotation                |
| --------- | ---------- | --------------------- | ---------------------------------- |
| Home      | Read All   | `getAllItemsStream()` | `@Query `[[SQL SELECT\|SELECT]]` * |
| Details   | Read One   | `getItemStream(id)`   | `@Query WHERE id`                  |
| **Entry** | **Create** | `insertItem()`        | `@Insert`                          |
| **Edit**  | **Update** | **`updateItem()`**    | **`@Update`**                      |
| Details   | Delete     | `deleteItem(id)`      | `@Delete`                          |

## Reactive Consistency Guarantee

**Single source of truth** via shared `AppDataContainer.`[[ItemsRepository]]:

```
Edit Screen: updateItem(id=5, quantity=10→15)
↓ Room emits via Flow
HomeViewModel: List updates (item 5 shows quantity=15)
ItemDetailsViewModel(id=5): Shows quantity=15
Other ItemDetails(id≠5): Unaffected
```

**Result**: Edit any item → Save → Navigate anywhere → ALL screens instantly show correct data. No stale [[State in Compose|State]], no manual refresh, pure reactive [[Model-View-ViewModel|MVVM]] with [[Room]] Flows.

The screen's simplicity (reusing `ItemEntryBody`) demonstrates excellent architecture—**same patterns scale across all CRUD operations**.