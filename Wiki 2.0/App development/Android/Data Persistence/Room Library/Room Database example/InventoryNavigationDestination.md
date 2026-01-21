```kotlin
/**
 * Interface to describe the navigation destinations for the app
 */
interface NavigationDestination {
    /**
     * Unique name to define the path for a composable
     */
    val route: String

    /**
     * String resource id to that contains title to be displayed for the screen.
     */
    val titleRes: Int
}

```

The `NavigationDestination` interface defines a type-safe contract for all app screens in the navigation graph, completely decoupling UI navigation from database operations while enabling consistent routing to screens that perform CRUD actions.

## Navigation Abstraction Purpose

This interface standardizes screen identity across the app:

- `route: String` provides unique paths (`"home"`, `"item_entry"`, `"item_detail/{id}"`) for `NavHost.composable(route)`.
- `titleRes: Int` supplies localized titles for TopAppBars (`stringResource(HomeDestination.titleRes)`).  
    Objects like `HomeDestination` implement it as compile-time constants, preventing typos and enabling refactor-safe navigation.
## Database Architecture Decoupling

**Zero database awareness**—this layer sits above MVVM:

```
NavHost("home") → HomeScreen → HomeViewModel → Repository → Room DB
       ↓
"item_entry" → ItemEntryViewModel → Repository → INSERT Item → HomeScreen auto-updates
```

Navigation triggers database mutations in destination ViewModels, but flows back reactively through shared Repository. Screens remain navigation-driven, not data-driven.

## Integration with Prior Code

Connects full stack:

1. **AppContainer** provides `itemsRepository` singleton
2. **AppViewModelProvider** injects it into ViewModels
3. **HomeScreen** uses `HomeDestination.route` + `AppViewModelProvider.Factory`
4. Navigation callbacks route to other destinations (ItemEntry, ItemUpdate)
5. **Repository Flow** automatically refreshes `HomeViewModel.homeUiState` after mutations

## Type Safety Benefits

```kotlin
// Instead of magic strings
navController.navigate("item_detail/$itemId")  // Error-prone

// Uses interface
object ItemDetailDestination : NavigationDestination {
    override val route = "item_detail/{itemId}"
    override val titleRes = R.string.item_details_title
}
navController.navigate("item_detail/$itemId")  // IDE validates route exists
```

Prevents navigation crashes. Database updates from any screen (Entry/Edit/Delete) instantly reflect everywhere via Room's Flow emissions—no refresh coordination needed.

## Single Activity Pattern

Enables modern architecture: one `MainActivity` hosts `NavHost`, swapping composables. Each destination gets its ViewModel (scoped via factory), accessing the same database instance. Perfect for offline-first apps with Room.