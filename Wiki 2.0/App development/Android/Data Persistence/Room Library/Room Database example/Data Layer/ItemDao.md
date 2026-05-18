```kotlin
@Dao
interface ItemDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(item: Item)

    @Update
    suspend fun update(item: Item)

    @Delete
    suspend fun delete(item: Item)

    @Query("SELECT * from items WHERE id = :id")
    fun getItem(id: Int): Flow<Item>

    @Query("SELECT * from items ORDER BY name ASC")
    fun getAllItems(): Flow<List<Item>>
}

```

This [[Interface]] is a **[[Room]] [[Data Access Object|DAO]] (Data Access Object)** that defines how the rest of your app reads and writes `Item` rows in the `items` table, without talking to SQLite directly.

## Role in the database layer

- `@Dao` marks this interface as a DAO; [[Room]] generates the actual implementation at compile time.[](https://developer.android.com/training/data-storage/room/accessing-data)​
- It sits between your **RoomDatabase** and the rest of your app, providing a type‑safe [[API]] instead of raw SQL calls scattered through the code.[](https://developer.android.com/training/data-storage/room/accessing-data)​

In a typical architecture:

- **[[Entity]]**: `Item` defines the table schema.
- **[[Data Access Object|DAO]]**: `ItemDao` defines operations on that table. 
- **RoomDatabase**: holds the DB and exposes `fun itemDao(): ItemDao`.
- **[[Repository]]/ViewModel/UI**: call [[Data Access Object|DAO]] methods to persist and observe data.[](https://daily.dev/blog/android-room-persistence-library-complete-guide)

## CRUD operations

- `@Insert(onConflict = OnConflictStrategy.IGNORE)`
    - Inserts a new `Item` row.
    - If a conflict occurs on a unique/primary key (same `id`), the insert is ignored, and the existing row stays unchanged.[](https://www.geeksforgeeks.org/android/android-data-access-object-in-room-database/)
    - Marked `suspend` so it runs off the main thread when called from a coroutine.
- `@Update`
    - Updates the matching row based on the `Item.id` primary key.
    - Only changes columns for that row; if no row with that `id` exists, nothing happens.[](https://www.geeksforgeeks.org/android/android-data-access-object-in-room-database/)
- `@Delete`
    - Deletes the row corresponding to the given `Item` (again matched by primary key).[](https://developer.android.com/training/data-storage/room/accessing-data)​

## Query methods with Flow

- `@Query("`[[SQL SELECT|SELECT]]` * from items WHERE id = :id") fun getItem(id: Int): Flow<Item>`
    - Returns a cold `Flow<Item>` that emits the row with that `id`.
    - When the row changes in the [[Database]], [[Room]] automatically re-emits the updated `Item`, which works nicely with [[ViewModel]] + [[Jetpack Compose|Compose]]/LiveData for reactive UI.[](https://daily.dev/blog/android-room-persistence-library-complete-guide)
- `@Query("SELECT * from items `[[SQL ORDER BY|order by]]` name ASC") fun getAllItems(): Flow<List<Item>>`
    - Returns a `Flow<List<Item>>` with all items sorted by name.
    - Any insert/update/delete affecting `items` will trigger a new emission, so the UI list stays in sync with the DB without manual refresh logic.[](https://daily.dev/blog/android-room-persistence-library-complete-guide)

In summary, `ItemDao` is the **single, centralized API** your app uses to manipulate `Item` data in the Room [[Database]], while Room handles the SQL, threading (via `suspend`), and reactive updates (via `Flow`) behind the scenes.