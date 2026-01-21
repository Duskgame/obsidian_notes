```kotlin
/**
 * Provides Navigation graph for the application.
 */
@Composable
fun InventoryNavHost(
    navController: NavHostController,
    modifier: Modifier = Modifier,
) {
    NavHost(
        navController = navController,
        startDestination = HomeDestination.route,
        modifier = modifier
    ) {
        composable(route = HomeDestination.route) {
            HomeScreen(
                navigateToItemEntry = { navController.navigate(ItemEntryDestination.route) },
                navigateToItemUpdate = {
                    navController.navigate("${ItemDetailsDestination.route}/${it}")
                }
            )
        }
        composable(route = ItemEntryDestination.route) {
            ItemEntryScreen(
                navigateBack = { navController.popBackStack() },
                onNavigateUp = { navController.navigateUp() }
            )
        }
        composable(
            route = ItemDetailsDestination.routeWithArgs,
            arguments = listOf(navArgument(ItemDetailsDestination.itemIdArg) {
                type = NavType.IntType
            })
        ) {
            ItemDetailsScreen(
                navigateToEditItem = { navController.navigate("${ItemEditDestination.route}/$it") },
                navigateBack = { navController.navigateUp() }
            )
        }
        composable(
            route = ItemEditDestination.routeWithArgs,
            arguments = listOf(navArgument(ItemEditDestination.itemIdArg) {
                type = NavType.IntType
            })
        ) {
            ItemEditScreen(
                navigateBack = { navController.popBackStack() },
                onNavigateUp = { navController.navigateUp() }
            )
        }
    }
}

```

This `InventoryNavHost` defines the complete navigation graph for the single-activity app, routing between screens that all share the same Room database repository via centralized ViewModel factory.

## Navigation Graph Structure

The `NavHost` declares four composable destinations forming the inventory workflow:

- **HomeDestination**: Entry point showing item list (`HomeScreen` with `HomeViewModel`)
- **ItemEntryDestination**: Add new item screen
- **ItemDetailsDestination**: View single item (receives `itemId: Int` argument)
- **ItemEditDestination**: Edit existing item (receives `itemId: Int` argument)

Routes use string templates for arguments: `"${ItemDetailsDestination.route}/{it}"` where `{it}` is the item ID from the list click.[](https://c1ctech.com/android-jetpack-compose-navigation-example/)​

## Database State Preservation Across Screens

Critical for Room database architecture—**ViewModels survive navigation** unlike Activities/Fragments:
```
HomeScreen ←→ ItemEntryScreen     (new item → Home list auto-updates via Flow)
     ↓
ItemDetailsScreen ←→ ItemEditScreen  (edit item → Home list instantly reflects via shared Repository)
```

When `ItemEntryScreen` calls `repository.insert(newItem)`, `HomeViewModel.homeUiState` automatically emits updated list due to Room's Flow reactivity—no manual refresh needed.

## Argument Passing Pattern

```kotlin
navArgument(ItemDetailsDestination.itemIdArg) { type = NavType.IntType }
```

Item ID flows through navigation to target ViewModels:

1. Home clicks item → navigates with ID
2. `ItemDetailsViewModel` extracts ID from `SavedStateHandle` (injected via factory)
3. `SavedStateHandle.get<Int>("itemId")` → `repository.getItem(id)` → displays item
4. Edit button → passes same ID to ItemEdit screen

## Architecture Layer Integration

```
UI Layer: NavHost coordinates screens
↓
ViewModel Layer: Each screen observes its ViewModel (HomeViewModel, ItemDetailsViewModel, etc.)
↓
Repository Layer: Single shared instance across all ViewModels → Room database
```

**No data duplication**—all screens read/write through `AppDataContainer.itemsRepository`. Mutations propagate instantly via Kotlin Flows.

## Back Navigation Handling

Multiple back strategies ensure proper stack management:

- `navigateUp()`: Goes to logical parent (Home → Details → Edit → Details)
- `popBackStack()`: Pops top screen (Entry/Edit → previous)  
	Prevents stack overflow and maintains database consistency during navigation.
