```kotlin
class InventoryApplication : Application() {

    /**
     * AppContainer instance used by the rest of classes to obtain dependencies
     */
    lateinit var container: AppContainer

    override fun onCreate() {
        super.onCreate()
        container = AppDataContainer(this)
    }
}

```

This `InventoryApplication` class extends Android's `Application` class to initialize a centralized dependency container during app startup, providing app-wide access to the database repository used by ViewModels.

## Application Lifecycle Role

Android's `Application.onCreate()` runs once when the app process starts—before any Activity or Service lifecycle methods—making it ideal for heavy one-time initialization like database setup. Here, it creates `AppDataContainer` (likely holding Room database, DAO, and `itemsRepository`), storing it in the `container` property for singleton-like access across the app.[](https://stackoverflow.com/questions/4585627/android-application-class-lifecycle)

## Database Integration Pattern

The `container: AppContainer` acts as a manual dependency injection (DI) container following Google's recommended architecture:

- `AppDataContainer(this)` receives the `Application` Context to build Room database instance (thread-safe, singleton scoped to app process).
- `itemsRepository` inside abstracts data sources (local Room DB + optional network), exposing Flow/StateFlow for reactive UI updates.  
    This pattern avoids Context leaks in ViewModels and enables testing by swapping containers.[](https://developer.android.com/guide/components/activities/activity-lifecycle)​

## Connection to ViewModel Factory

Links directly to previous code: ViewModels access `inventoryApplication().container.itemsRepository` via the factory extension. Flow: App starts → `onCreate()` builds container → Activities/Fragments use `AppViewModelProvider.Factory` → ViewModels get shared repository instance for CRUD operations (insert/update items, query lists).[ from prior]

## Architecture Benefits

Enforces MVVM with Repository pattern:
- **UI Layer** (Activities/Fragments): Observe ViewModel state.
- **ViewModel Layer**: Exposes repository Flows, survives config changes.
- **Repository Layer** (in container): Handles DB transactions on background threads via coroutines/Dispatchers.IO.  
    Prevents direct database access from UI, improves testability over raw DAO injection.[ from prior]

## Production Considerations

For larger apps, migrate to Hilt/Dagger (generates similar containers automatically) or Koin. Room database creation happens here to ensure single instance; typical impl: `Room.databaseBuilder(context, ItemsDatabase::class.java, "items_db").build()`.