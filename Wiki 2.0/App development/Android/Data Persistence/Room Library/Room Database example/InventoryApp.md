```kotlin
  
/**  
 * Top level composable that represents screens for the application. */@Composable  
fun InventoryApp(navController: NavHostController = rememberNavController()) {  
    InventoryNavHost(navController = navController)  
}  
  
/**  
 * App bar to display title and conditionally display the back navigation. */@OptIn(ExperimentalMaterial3Api::class)  
@Composable  
fun InventoryTopAppBar(  
    title: String,  
    canNavigateBack: Boolean,  
    modifier: Modifier = Modifier,  
    scrollBehavior: TopAppBarScrollBehavior? = null,  
    navigateUp: () -> Unit = {}  
) {  
    CenterAlignedTopAppBar(  
        title = { Text(title) },  
        modifier = modifier,  
        scrollBehavior = scrollBehavior,  
        navigationIcon = {  
            if (canNavigateBack) {  
                IconButton(onClick = navigateUp) {  
                    Icon(  
                        imageVector = Filled.ArrowBack,  
                        contentDescription = stringResource(string.back_button)  
                    )  
                }  
            }  
        }  
    )  
}
```

`InventoryApp` serves as the root composable orchestrating the entire [[Navigation]] graph, while `InventoryTopAppBar` provides consistent navigation UI across all screens—both operate **above** the [[Database]] layer but enable the [[Room]]-backed [[CRUD]] workflow.

## App Entry Point Architecture

```kotlin
@Composable
fun InventoryApp(navController: NavHostController = rememberNavController()) {
    InventoryNavHost(navController = navController)
}
```

**Sets up navigation host** that contains all [[Database]]-driven screens:

```
InventoryApp → NavHost → [Home → Details → Edit/Entry] 
                    ↓ All screens use AppViewModelProvider.Factory
                    ↓ Injects shared ItemsRepository → Room database
```

The `rememberNavController()` survives recompositions, maintaining back stack across config changes while ViewModels (with [[Database]] [[State in Compose|State]]) survive independently.

## TopAppBar: Cross-Cutting Navigation Concern

`InventoryTopAppBar` provides **consistent back navigation** across the entire app:

- `canNavigateBack`: Controls arrow visibility (Home=false, Details/Edit/Entry=true)
    
- `navigateUp()`: Pops back stack, revealing previous screen with **fresh database state**
    
- Used by: [[ItemDetailsScreen]], [[ItemEntryScreen]], [[ItemEditScreen]]
    

**Database impact**: When user navigates back from Edit→Details→Home, each screen automatically shows latest [[Room]] data via their [[ViewModel]] Flows—no stale state.

## Navigation + Database State Survival

Complete lifecycle preservation pattern:

```
Config Change / Navigation:
Activity recreates → Composables rebuild → NavController state restored
                                         ↓
ViewModels survive (via factory + SavedStateHandle) → Repository Flows active
                                         ↓
All screens show latest Room data instantly
```

## Architecture Layer Interaction

```
UI Layer: InventoryApp + TopAppBar + NavHost (navigation orchestration)
    ↓ Passes navController callbacks
Screen Layer: Home/Details/Entry/Edit (collect ViewModel state)
    ↓ viewModel(factory = AppViewModelProvider.Factory)
ViewModel Layer: Observe ItemsRepository Flows
    ↓ AppDataContainer (from InventoryApplication.onCreate())
Repository Layer → Room DAO → SQLite
```

## Production Navigation Pattern

This **single activity** architecture with [[Jetpack Compose|Compose]] Navigation is Google's recommended modern pattern:

```kotlin
MainActivity {
    setContent { InventoryApp() }
}
```

**Benefits for database apps**:

- ViewModels scoped to NavGraph survive screen transitions
- Shared [[Repository]] ensures all screens see same [[Room]] data
- TopAppBar provides consistent UX across CRUD operations
- No Fragment lifecycle complexity

**Result**: User edits item → navigates anywhere → back button always returns to screen with current database state. Perfect [[Model-View-ViewModel|MVVM]]+Room integration with zero manual state management.