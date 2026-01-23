In this task, you implement the [[ItemsRepository]] [[Interface]] and [[OfflineItemsRepository]] [[Kotlin Class|Class]] to provide `get`, `insert`, `delete`, and `update` entities from the [[Database]].

1. Open the [[ItemsRepository]].kt file under the `data` package.
2. Add the following functions to the interface, which map to the [[Data Access Object|DAO]] implementation.

```kotlin
import kotlinx.coroutines.flow.Flow

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

3. Open the [[OfflineItemsRepository]].kt file under the `data` package.
4. Pass in a [[Kotlin Constructor|constructor]] [[Parameter]] of the type [[ItemDao]].

```kotlin
class OfflineItemsRepository(private val itemDao: ItemDao) : ItemsRepository
```

5. In the [[OfflineItemsRepository]] class, override the functions defined in the [[ItemsRepository]] interface and call the corresponding functions from the [[ItemDao]].

```kotlin
import kotlinx.coroutines.flow.Flow

class OfflineItemsRepository(private val itemDao: ItemDao) : ItemsRepository {
    override fun getAllItemsStream(): Flow<List<Item>> = itemDao.getAllItems()

    override fun getItemStream(id: Int): Flow<Item?> = itemDao.getItem(id)

    override suspend fun insertItem(item: Item) = itemDao.insert(item)

    override suspend fun deleteItem(item: Item) = itemDao.delete(item)

    override suspend fun updateItem(item: Item) = itemDao.update(item)
}
```

