```kotlin

class OfflineItemsRepository(private val itemDao: ItemDao) : ItemsRepository {
    override fun getAllItemsStream(): Flow<List<Item>> = itemDao.getAllItems()

    override fun getItemStream(id: Int): Flow<Item?> = itemDao.getItem(id)

    override suspend fun insertItem(item: Item) = itemDao.insert(item)

    override suspend fun deleteItem(item: Item) = itemDao.delete(item)

    override suspend fun updateItem(item: Item) = itemDao.update(item)
}
```

This [[Kotlin Class|Class]] is a **[[Repository]] implementation** that connects your app’s [[Data Layer]] to the local [[Room]] [[Database]], using the `[[ItemDao]]` as its data source.

## Role in the architecture

- [[ItemsRepository]] is an **interface** that defines the operations the rest of the app needs: load all items, load one item, insert, delete, update. This decouples the UI/ViewModel from the concrete data source ([[Room]], network, etc.).
- `OfflineItemsRepository` is a **concrete implementation** that uses [[Room]] via [[ItemDao]] as an **offline/local data source**. The name “Offline” usually signals “backed by local storage like Room, no network”. In a more complex app you might have another implementation that combines Room with a remote [[API]].

The typical dependency chain looks like:

UI → [[ViewModel]] → [[ItemsRepository]] → [[ItemDao]] → InventoryDatabase → SQLite

The ViewModel only knows about [[ItemsRepository]], so you can swap this class with a fake in tests or with a different data source without touching UI code.

## How each method maps to the database

This class mostly forwards calls to the [[Data Access Object|DAO]], but that forwarding is what enforces [[Separation of concerns]]:

- `getAllItemsStream(): Flow<List<Item>> = itemDao.getAllItems()`
    - Returns a **reactive stream** of all items.
    - The [[Data Access Object|DAO]] returns a `Flow<List<Item>>`, which emits a new list whenever the `items` table changes. The repository just passes that through. Your ViewModel collects this Flow and exposes it to the UI, so any insert/update/delete automatically updates the UI.
- `getItemStream(id: Int): Flow<Item?> = itemDao.getItem(id)`
    - Same idea for a **single item** by id.
    - `Item?` allows for the possibility that no row with that id exists.
- `insertItem(item: Item) = itemDao.insert(item)`
- `deleteItem(item: Item) = itemDao.delete(item)`
- `updateItem(item: Item) = itemDao.update(item)`
    - All are `suspend` functions in the [[Interface]], and the implementation simply delegates to the [[Data Access Object|DAO]]’s suspend methods.
    - These are called from a coroutine context (for example [[ViewModelScope]].launch { repository.insertItem(...) }) so they run off the main thread, which is required for Room write operations.

## Why this repository layer is useful

- **[[OOP|abstraction]]**: ViewModels don’t depend on Room-specific APIs (`@Query`, `ItemDao`), only on the repository interface.
- **Testability**: In unit tests you can provide a fake `ItemsRepository` that returns in-memory data instead of hitting a real [[Database]].
- **Extensibility**: If later you add a remote data source (e.g., server sync), you can keep the same `ItemsRepository` interface but change the implementation to coordinate Room + network, without touching the UI.
- **Single responsibility**: The DAO handles SQL and table details; the repository handles “what the app considers an item operation” and how it is provided to the rest of the app.

In short, `OfflineItemsRepository` is the **Room-backed implementation** of your repository boundary: it turns high-level “insert/update/delete/fetch items” calls from the ViewModel into concrete DAO operations on the local SQLite [[Database]], while preserving a clean architecture separation.