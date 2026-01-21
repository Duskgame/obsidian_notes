```kotlin
/**
 * App container for Dependency injection.
 */
interface AppContainer {
    val itemsRepository: ItemsRepository
}

/**
 * [AppContainer] implementation that provides instance of [OfflineItemsRepository]
 */
class AppDataContainer(private val context: Context) : AppContainer {
    /**
     * Implementation for [ItemsRepository]
     */
    override val itemsRepository: ItemsRepository by lazy {
        OfflineItemsRepository(InventoryDatabase.getDatabase(context).itemDao())
    }
}
```

This code defines a **manual dependency injection container** for your app’s data layer, specifically for providing a single `ItemsRepository` that is backed by your Room database.

## What this is in the architecture

- `AppContainer` is an **interface** that declares the dependencies the rest of the app can request. Here, it exposes one thing: `itemsRepository`.
- `AppDataContainer` is the **concrete implementation** that knows _how_ to build those dependencies (Room DB → DAO → Repository). This pattern is Google’s recommended way to do **manual dependency injection** in small apps without Hilt/Dagger.

In a typical setup:

- Your custom `Application` class owns a single `AppDataContainer` instance
- Activities/ViewModels obtain `itemsRepository` through that container, instead of constructing it themselves. This keeps construction logic in one place and aligns with DI best practices.

## How it wires the database into the repository

The `by lazy` block is where the database stack is built:
```kotlin
override val itemsRepository: ItemsRepository by lazy {
    OfflineItemsRepository(
        InventoryDatabase.getDatabase(context).itemDao()
    )
}
```

Conceptually:

1. `InventoryDatabase.getDatabase(context)`
    - Returns the singleton Room database instance (`InventoryDatabase`).
    - This DB is configured with your `Item` entity and knows how to create `ItemDao`.        
2. `.itemDao()`
    - Gets the DAO that can perform SQL operations on the `items` table (insert, update, delete, queries).
3. `OfflineItemsRepository(itemDao)`
    - Wraps the DAO in a repository implementation that matches the `ItemsRepository` interface.
    - ViewModels talk to `ItemsRepository`, not directly to the DAO or DB, which keeps the UI layer independent of Room APIs.
4. `by lazy { ... }`
    - Ensures the repository (and underlying DB + DAO) is created **only once**, on first use, and then reused.
    - This avoids repeatedly creating expensive database objects.
## Why this is useful for DB usage

- **Separation of concerns**:
    - Room/SQLite details (DB, DAO) live in the data layer and in `AppDataContainer`.
    - ViewModels and UI only depend on `ItemsRepository`, making them easier to test and change.
- **Single source of truth**:
    - There is exactly one `ItemsRepository` (and one `InventoryDatabase`) shared across the app, preventing multiple DB instances and inconsistent state.
- **Manual dependency injection**:
    - Rather than letting each class create its own database and DAO, the container builds them once and “injects” them where needed via constructor parameters. This is the core idea of DI and matches the manual container pattern shown in Android’s architecture docs.[](https://developer.android.com/training/dependency-injection)

So in the context of database usage, `AppDataContainer` is the object that **wires the Room database into your repository** and **exposes that repository to the rest of the app** in a clean, centralized, and testable way.