project directory
	data
		[[ItemAppContainer]]
		[[ItemDao]]
		[[ItemEntity]]
		[[ItemDatabase]]
		[[ItemsRepository]]
		[[OfflineItemsRepository]]
	ui
		home
			[[ItemHomeScreen]]
			[[HomeViewModel]]
		item
			[[ItemDetailsScreen]]
			[[ItemDetailsViewModel]]
			[[ItemEditScreen]]
			[[ItemEditViewModel]]
			[[ItemEntryScreen]]
			[[ItemEntryViewModel]]
		navigation
			[[InventoryNavGraph]]
			[[InventoryNavigationDestination]]
		theme
		[[AppViewModelProvider]]
	[[InventoryApplication]]
[[InventoryApp]]



![[android_startup_flowchart.png|816x544]]


## Executive Summary

This inventory app implements Google's recommended modern [[Android]] architecture combining **[[Model-View-ViewModel|MVVM]] (Model-View-ViewModel)**, **[[Repository]] Pattern**, **[[Jetpack Compose]]**, **[[Kotlin]] Flows**, and **[[Room]] [[Database]]**. Every component works in concert to ensure **single source of truth**, **automatic UI synchronization**, and **zero stale [[State in Compose]]**. Understanding this architecture unlocks scalable, testable Android development.

---
## Architecture Layers

### Layer 1: UI Layer (Jetpack Compose)

- **InventoryApp** - Root composable
- **InventoryNavHost** - Navigation orchestration
- **InventoryTopAppBar** - Shared navigation UI
- **Screens** - ItemHomeScreen, ItemEntryScreen, ItemDetailsScreen, ItemEditScreen
- **Responsibility**: Render state, emit user events, collect ViewModels

### Layer 2: Navigation & Dependency Injection

- **NavigationDestination** - Route definitions
- **InventoryApplication** - Custom Application class, singleton container
- **AppViewModelProvider.Factory** - ViewModel instantiation with dependencies
- **Responsibility**: Wire UI to ViewModels, inject shared repository

### Layer 3: ViewModel Layer

- **HomeViewModel** - Manages home screen state (all items list)
- **ItemDetailsViewModel** - Manages details screen state (single item)
- **ItemEntryViewModel** - Manages form state for new items
- **ItemEditViewModel** - Manages form state for editing items
- **Responsibility**: Hold UI state, perform validation, coordinate with repository

### Layer 4: Repository Layer (Data Abstraction)

- **ItemsRepository** - Interface defining data operations
- **OfflineItemsRepository** - Implementation delegating to DAO
- **AppDataContainer** - Dependency injection container
- **Responsibility**: Abstract data sources, coordinate CRUD operations

### Layer 5: Data Access Layer (Room)

- **InventoryDatabase** - Room database singleton
- **ItemDao** - DAO with @Query/@Insert/@Update/@Delet


## Complete Android MVVM + Room Architecture: End-to-End Integration

This inventory app demonstrates the **industry-standard modern Android architecture** that powers production apps at Google, Netflix, Spotify, and enterprise organizations. Understanding this system transforms development from fragile single-screen prototypes to scalable, testable, maintainable applications serving millions of users.

## How All Components Work Together: The Complete Picture

## Foundation: Single App Startup Sequence

The architecture begins before any UI renders. When Android launches the app:

**Step 1: Application Class Initialization**
```
MainActivity created → InventoryApplication.onCreate() called
```

This is where dependency injection begins. Before a single Composable renders, `InventoryApplication.onCreate()` creates `AppDataContainer`, which lazily initializes `InventoryDatabase` (Room's singleton). This singleton uses double-checked locking (`@Volatile + synchronized`) to ensure exactly one database connection exists for the entire app process—no matter how many screens open or how many config changes occur.[](https://developer.android.com/training/data-storage/room)​

**Step 2: Dependency Container Bootstrap**
```
AppDataContainer(context) 
  → InventoryDatabase.getDatabase(context) [singleton]
  → ItemDao instance
  → OfflineItemsRepository wraps ItemDao
```

The repository becomes the single source of truth for all data access. Every ViewModel receives the same repository instance via AppViewModelProvider.Factory.

Step 3: UI Composition
```
MainActivity.setContent { InventoryApp() }
  → InventoryNavHost(navController)
  → HomeScreen (default destination)
```

## Read Operation: Displaying Data (HomeScreen)

When `HomeScreen` renders, it requests a ViewModel with the factory:
```kotlin
viewModel(factory = AppViewModelProvider.Factory)
```

Jetpack Compose's `viewModel()` function calls the factory's initializer, which:

1. Calls `inventoryApplication()` to access the app singleton
2. Retrieves `.container.itemsRepository` (the shared repository)
3. Passes it to `HomeViewModel` constructor

`HomeViewModel` immediately subscribes to the database:

```kotlin
itemsRepository.getAllItemsStream()
  → ItemDao.getAllItems()  
  → Room executes: SELECT * FROM items ORDER BY name ASC
  → Wrapped in Flow<List<Item>>
  → .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5000))
  → Creates StateFlow<HomeUiState>
```

**Critical insight**: `SharingStarted.WhileSubscribed(5000)` means Room **stops** emitting when nobody's watching (5 second grace period), then **resumes** when screens return—battery efficient and reactive.[](https://developer.android.com/develop/ui/compose/navigation)

`HomeScreen` collects: `val uiState by viewModel.homeUiState.collectAsState()` → automatically recomposes with list.

## Navigation & ID Routing: From List to Details

When user clicks an item, `HomeScreen` navigates:

```kotlin
navController.navigate("${ItemDetailsDestination.route}/${itemId}")
// Becomes: "item_details/5"
```

`InventoryNavHost` matches this route pattern: `"item_details/{itemId}"` and extracts `itemId = 5`. This ID enters `ItemDetailsScreen` and becomes accessible to **SavedStateHandle**:

```kotlin
viewModel: ItemDetailsViewModel = viewModel(factory = AppViewModelProvider.Factory)
```

The factory creates `ItemDetailsViewModel(savedStateHandle, repository)`, which immediately queries:

```kotlin
private val itemId: Int = checkNotNull(savedStateHandle[ItemDetailsDestination.itemIdArg])
// itemId = 5

itemsRepository.getItemStream(itemId)
  → ItemDao.getItem(5)
  → Room executes: SELECT * FROM items WHERE id = 5
  → Wrapped in Flow<Item>
  → Creates StateFlow<ItemDetailsUiState>
```

**SavedStateHandle survives configuration changes**—if user rotates device, the ViewModel recreates but recovers the same `itemId` from saved state, querying the same item automatically.[](https://developer.android.com/codelabs/basic-android-kotlin-compose-update-data-room)​

## Update Operation: Edit → Save → Database → Propagate

When user clicks Edit button, another navigation passes the ID:

```kotlin
navController.navigate("${ItemEditDestination.route}/$itemId")
// Becomes: "item_edit/5"
```

`ItemEditScreen` creates `ItemEditViewModel(savedStateHandle, repository)` which loads one snapshot:

```kotlin
viewModelScope.launch {
    itemUiState = itemsRepository.getItemStream(itemId)
        .filterNotNull()
        .first()  // ← Get ONE snapshot, not continuous updates
        .toItemUiState(true)
}
```

User edits form fields. Each keystroke calls:

```kotlin
viewModel.updateUiState(itemDetails.copy(name = "Banana"))
```

This validates locally (`.isNotBlank()`) but **does not touch the database**—pure in-memory state in `mutableStateOf`. Save button remains disabled until all fields valid.

When user clicks Save:

```kotlin
coroutineScope.launch {
    viewModel.updateItem()  // suspend function
    navigateBack()
}
```

Inside `updateItem()`:
```kotlin
suspend fun updateItem() {
    if (validateInput()) {
        itemsRepository.updateItem(itemUiState.itemDetails.toItem())
    }
}
```

This calls `OfflineItemsRepository.updateItem()` which delegates to:

```kotlin
override suspend fun updateItem(item: Item) = itemDao.update(item)

@Update
suspend fun update(item: Item)  // Room auto-generates: UPDATE items SET ... WHERE id=?
```

**Thread magic**: Room automatically runs this on `Dispatchers.IO` (background thread), preventing UI freeze.

## The Reactive Cascade: Single Change → All Screens Update

This is where MVVM's power emerges. The moment `@Update` completes on SQLite:

**Room's Flow subscription mechanism triggers**:

- `ItemDao.getAllItems()` → Room detects row changed at `id=5`, re-executes query
    
- Emits: `[Item(1, ...), Item(5, name="Banana"), Item(2, ...)]`
    
- `HomeViewModel.homeUiState` receives new list
    
- `HomeScreen.LazyColumn` rebuilds with "Banana" instead of "Apple"
    

**Simultaneously**:

- `ItemDao.getItem(5)` → Room detects this specific row changed
    
- Emits: `Item(5, "Banana", ...)`
    
- `ItemDetailsViewModel.uiState` receives update
    
- If `ItemDetailsScreen` still exists in back stack (user navigated immediately), it shows updated data instantly
    

**Other instances unaffected**:

- `ItemDao.getItem(3)` → Not this row, no emission
    
- That screen continues showing original data (correct!)
    

This is **automatic consistency without manual refresh**—no callbacks, no event buses, no duplicate state management code.[](https://developer.android.com/codelabs/basic-android-kotlin-compose-update-data-room)​​

## Create Operation: Form → Insert → New Item Propagates

`ItemEntryScreen` uses the same `ItemEntryBody` component as edit (DRY principle). `ItemEntryViewModel(repository)` manages form state for new items. User fills form, clicks Save:

```kotlin
suspend fun saveItem() {
    if (validateInput()) {
        itemsRepository.insertItem(itemUiState.itemDetails.toItem())
    }
}
```

This executes:

```kotlin
@Insert(onConflict = OnConflictStrategy.IGNORE)
suspend fun insert(item: Item)
```

SQLite auto-generates `id = 6` for the new row. Room's Flow emission chain activates:

- `HomeViewModel.getAllItemsStream()` emits 51 items (new item included)
    
- `HomeScreen.LazyColumn` rebuilds with new item in alphabetical order
    
- User navigates back → sees new item instantly without manual refresh
    

## Delete Operation: With Confirmation Dialog

`ItemDetailsScreen` provides delete button wrapped in confirmation dialog (prevents accidental loss):

```kotlin
onDelete = {
    coroutineScope.launch {
        viewModel.deleteItem()
        navigateBack()
    }
}
```

`deleteItem()` executes:

Reactive cascade again:

- `HomeViewModel.getAllItemsStream()` emits list with 50 items (removed)
- `HomeScreen` updates instantly

## Thread Safety & Composition Change Resilience

## Dispatchers: The Hidden Thread Manager

Users never see it, but Room automatically handles thread dispatchers:

- **UI operations** (mutableStateOf, Compose recomposition): Main thread
- **Database queries** (getAllItems, getItem): Room switches to Dispatchers.IO
- **Flow emission**: Room switches back to Main thread for UI collectio

This prevents ANR (app not responding) crashes from blocking the main thread during long database operations.

## Configuration Change Survival

When user rotates device mid-edit:

```
Activity destroyed → Compose recomposition → SavedStateHandle restored → 
ItemEditViewModel recreated → Form state recovered → Screen shows exact same state
```

The magic ingredients:

- `SavedStateHandle` persists navigation arguments (`itemId`)
- `ViewModels` scope tied to screen lifecycle (survives Activity destruction)
- `InventoryDatabase` singleton unaffected (one connection for entire app)
- `mutableStateOf(itemUiState)` recomposition restores previous typed values

**One caveat**: `rememberCoroutineScope` is composition-bound. If user rotates during save, the scope cancels. Production code moves database operations to `viewModelScope` to survive rotations:

```kotlin
// Better:
class ItemEditViewModel {
    fun updateItem() = viewModelScope.launch { 
        repository.updateItem(item)
    }
}
```

## Why This Architecture Solves Real Problems

| Real Problem                                                                              | This Architecture's Solution                                                                    |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Stale [[State in Compose]]**: Different screens show different values for same item     | Single repository [[Kotlin Object\|Instance]] queried by all ViewModels → automatic consistency |
| **Lost form data during rotation**: User's edits disappear                                | SavedStateHandle + [[ViewModel]] scope survive lifecycle                                        |
| **Manual refresh buttons**: User forgets to refresh                                       | [[Room]] Flows automatically emit on change                                                     |
| **Race conditions**: Two screens update same item simultaneously                          | [[Room]] transactions (SQLite) handle concurrency                                               |
| **ANR crashes**: [[Database]] queries block [[User Interface]] thread                     | Room suspend functions auto-dispatch to IO thread                                               |
| **Memory leaks**: Context held in ViewModels                                              | Repository takes Context only once during init, no leaks                                        |
| **Tight coupling**: [[User Interface]] directly queries database                          | Repository abstraction enables testing with mock implementations                                |
| **Configuration change crashes**: Screen [[State in Compose\|State]] lost during rotation | ViewModel + SavedStateHandle survive                                                            |

## Scalability Pattern

This exact pattern scales to apps with **dozens of screens and tables**:

- Add new [[Entity]] → define @Entity, @[[Data Access Object|DAO]], update repository [[Interface]]
- New ViewModel → inject repository, observe relevant Flow
- New screen → use [[AppViewModelProvider]].[[Factory]] (works automatically)
- New [[Database]] operation → all screens auto-update via reactive Flows

Companies like **Google I/O, Netflix, Spotify** use variations of this pattern for production apps serving millions of concurrent users.[](https://www.youtube.com/watch?v=bgS9VpNIP8s)[](https://www.geeksforgeeks.org/android/how-to-build-a-simple-note-android-app-using-mvvm-and-room-database/)​

## Conclusion

The inventory app is a masterclass in modern Android architecture. Every component serves a specific purpose: Jetpack Compose renders UI, [[Navigation]] orchestrates screens, ViewModels hold reactive [[State in Compose]], Repository abstracts data, Room handles persistence, and [[Kotlin]] Flows provide automatic synchronization. Together they eliminate entire classes of bugs (stale state, configuration crashes, race conditions) and compress hundreds of lines of synchronization code into zero. This is why Google recommends this pattern—it works.[](https://developer.android.com/training/data-storage/room)