```kotlin

/**
 * Repository that provides insert, update, delete, and retrieve of [Item] from a given data source.
 */
interface ItemsRepository {
    /**
     * Retrieve all the items from the the given data source.
     */
    fun getAllItemsStream(): Flow<List<Item>>

    /**
     * Retrieve an item from the given data source that matches with the [id].
     */
    fun getItemStream(id: Int): Flow<Item?>

    /**
     * Insert item in the data source
     */
    suspend fun insertItem(item: Item)

    /**
     * Delete item from the data source
     */
    suspend fun deleteItem(item: Item)

    /**
     * Update item in the data source
     */
    suspend fun updateItem(item: Item)
}
```

This [[Interface]] is a **[[Repository]] contract** that sits between your UI/ViewModel layer and your data source ([[Room]] [[Database]], or potentially others). It defines _what_ operations the app can perform with `Item` data, without exposing _how_ they’re implemented.

## Role in the architecture

In a typical [[Room]]-based app, the layers are:
- **[[Room]] [[Database]]** → provides [[ItemDao]], which talks directly to SQLite
- **Repository** → wraps [[ItemDao]] and possibly other sources; exposes a clean [[API]] to the rest of the app
- **ViewModel** → depends on `ItemsRepository`, not on the [[Data Access Object|DAO]] directly
- **UI ([[Jetpack Compose|Compose]]/Activities)** → observes Flows from the [[ViewModel]]

This interface is that repository boundary: the ViewModel knows only `ItemsRepository`, so you can change the underlying storage (Room, network, in‑memory) without touching UI code. This follows the Repository pattern recommended for [[Android]] apps to separate [[Data Layer]] from presentation.[](https://dev.to/rodrassilva/android-repository-pattern-using-room-retrofit-and-coroutines-58kb)​​

## Meaning of each method

- `fun getAllItemsStream(): Flow<List<Item>>`
    - Returns a **reactive stream** of all items.
    - In a Room-based implementation, this would delegate to [[ItemDao]].getAllItems() and emit a new list whenever the underlying table changes, keeping the UI automatically up to date.
- `fun getItemStream(id: Int): Flow<Item?>`
    - Reactive stream for a single item by id.
    - Used by detail screens: when the row changes in the DB, the UI gets the update via the Flow.
- `suspend fun insertItem(item: Item)` / `updateItem(item: Item)` / `deleteItem(item: Item)`
    - Suspend functions so they can be called from a coroutine ([[ViewModelScope]].launch { ... }) off the main thread.
    - In a Room-backed repository, they directly call the corresponding [[Data Access Object|DAO]] `@Insert`, `@Update`, `@Delete` methods.

The ViewModel then depends on `ItemsRepository`, not on `ItemDao`, which:
- Keeps [[Database]] details out of the UI layer
- Makes it easy to swap implementations (e.g., use a fake repository for tests)
- Centralizes data access logic in one place

So in the context of database usage, this interface is the **clean, testable [[OOP|abstraction]]** that encapsulates all item-related database operations and exposes them as coroutine/Flow-friendly APIs to the rest of your app.