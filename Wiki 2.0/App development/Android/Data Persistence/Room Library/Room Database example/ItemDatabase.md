```kotlin

/**
 * Database class with a singleton Instance object.
 */
@Database(entities = [Item::class], version = 1, exportSchema = false)
abstract class InventoryDatabase : RoomDatabase() {

    abstract fun itemDao(): ItemDao

    companion object {
        @Volatile
        private var Instance: InventoryDatabase? = null

        fun getDatabase(context: Context): InventoryDatabase {
            // if the Instance is not null, return it, otherwise create a new database instance.
            return Instance ?: synchronized(this) {
                Room.databaseBuilder(context, InventoryDatabase::class.java, "item_database")
                    .build()
                    .also { Instance = it }
            }
        }
    }
}

```

This [[Kotlin Class|Class]] is your **[[Room]] [[Database]] definition** plus a **singleton creator** so the app has exactly one [[Database]] [[Kotlin Object|Instance]].

## Database role

- `@Database(entities = [Item::class], version = 1, exportSchema = false)`
    - Marks this as the [[Room]] [[Database]] class and tells [[Room]] which **entities** (tables) it manages – here, only `Item`, so the DB has an `items` table.
    - `version = 1` is the schema version, used for migrations when you change the schema in future versions.
    - `exportSchema = false` disables schema export to a file; in production apps you often set this to `true` for version control of schema.
- `abstract class InventoryDatabase : RoomDatabase()`
    - Must extend `RoomDatabase`.
    - Room generates the implementation at compile time; you never instantiate this class directly, you only get it through the builder.
- `abstract fun `[[ItemDao]](): [[ItemDao]]
    - Exposes your [[Data Access Object|DAO]].
    - Room generates the implementation so you can call db.[[ItemDao]]() to get an `ItemDao` and then run `insert`, `update`, `getAllItems()`, etc.

This class is the **main entry point** to all persistent data in your app: UI → [[ViewModel]]/[[Repository]] → `InventoryDatabase.itemDao()` → SQLite.

## Singleton pattern

The [[Companion Object]] ensures there is **only one [[Database Instance]]**:

- `@Volatile private var Instance: InventoryDatabase? = `[[null]]
    - `@Volatile` ensures that changes to `Instance` are visible across threads immediately.
    - Room DBs are expensive to create; you should not build them more than once.
- `fun getDatabase(context: Context): InventoryDatabase`
    - Checks if `Instance` is already set:
        - If yes, returns the existing DB (fast path).
        - If no, enters `synchronized(this)` to make the initialization **thread-safe**:
            - `Room.databaseBuilder(context, InventoryDatabase::class.java, "item_database")` creates the database with the given name and schema.
            - `.build()` actually creates the DB file (if needed) and the in-memory objects.
            - `.also { Instance = it }` caches the created DB in `Instance` for future calls.

This “double-checked lock” style is the standard pattern recommended for Room so that:

- The whole app uses **one shared DB connection**, avoiding corruption or performance issues.
- Any component (repository, ViewModel, etc.) can call `InventoryDatabase.getDatabase(context)` and safely get the same instance.

In a typical architecture:

- An `Application` or a [[Dependency Injection|DI]] container calls `InventoryDatabase.getDatabase(applicationContext)` once and then passes `itemDao()` into a repository.
- The repository is used by ViewModels, which expose Flows/LiveData of `Item` data to the UI.