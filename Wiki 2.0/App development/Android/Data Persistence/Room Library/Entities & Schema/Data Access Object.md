---
aliases:
  - " "
  - DAO
---
- #@Insert
- #@Update
- #@Delete
- #@Query
- #Completed `[[ItemDao]]`:



The [Data Access Object](https://developer.android.com/reference/androidx/room/Dao) (DAO) is a pattern you can use to separate the persistence layer from the rest of the application by providing an abstract interface. This isolation follows the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle), which you have seen in previous codelabs.

The functionality of the DAO is to hide all the complexities involved in performing Database operations in the underlying persistence layer, separate from the rest of the application. This lets you change the [[Data Layer]] independently of the code that uses the data.




In this task, you define a DAO for Room. ==DAOs are the [[Main Components]] of [[Room]] that are responsible for defining the Interface that accesses the database.==

The DAO you create is a custom interface that provides convenience methods for querying/retrieving, inserting, deleting, and updating the database. Room generates an implementation of this Kotlin Class at compile time.

The [[Room]] library provides convenience annotations, such as `@Insert`, `@Delete`, and `@Update`, for defining methods that perform simple inserts, deletes, and updates without requiring you to write a SQL statement.

If you need to define more complex operations for insert, delete, update, or if you need to query the data in the database, use a `@Query` annotation instead.

As an added bonus, as you write your queries in [[Android]] Studio, the compiler checks your SQL queries for syntax errors.

For the Inventory app, you need the ability to do the following:

- **Insert** or add a new item.
- **Update** an existing item to update the name, price, and quantity.
- **Get** a specific item based on its primary key, `id`.
- **Get all items** so you can display them.
- **Delete** an entry in the database.

Complete the following steps to implement the item DAO in your app:

1. In the `data` package, create the [[Kotlin]] interface `ItemDao.kt`.
2. Annotate the `ItemDao` interface with `@Dao`.

```kotlin
import androidx.room.Dao

@Dao
interface ItemDao {
}
```


## @Insert

3. Inside the body of the interface, add an `@Insert` annotation.
4. Below the `@Insert`, add an `insert()` Function that takes an Kotlin Object of the `Entity` class `item` as its argument.
5. Mark the function with the `suspend` Keywords and to let it run on a separate thread.

The database operations can take a long time to execute, so they need to run on a separate thread. Room doesn't allow database access on the main thread.

```kotlin
import androidx.room.Insert

@Insert
suspend fun insert(item: Item)
```

When inserting items into the database, conflicts can happen. For example, multiple places in the code tries to update the [[Entity]] with different, conflicting, values such as the same primary key. An Entity is a row in DB. In the Inventory app, we only insert the entity from one place that is the **Add Item** screen so we are not expecting any conflicts and can set the conflict strategy to _Ignore_.

6. Add an argument `onConflict` and assign it a value of `OnConflictStrategy.`_`IGNORE`_.

The argument `onConflict` tells the Room what to do in case of a conflict. The `OnConflictStrategy.`_`IGNORE`_ strategy ignores a new item.

To know more about the available conflict strategies, check out the [`OnConflictStrategy`](https://developer.android.com/reference/androidx/room/OnConflictStrategy.html) documentation.

```kotlin
import androidx.room.OnConflictStrategy

@Insert(onConflict = OnConflictStrategy.IGNORE)
suspend fun insert(item: Item)
```

Now `Room` generates all the necessary code to insert the `item` into the database. When you call any of the DAO functions that are marked with Room annotations, Room executes the corresponding SQL query on the [[Database]]. For example, when you call the above Kotlin Class Method, `insert()` from your Kotlin code, `Room` executes a SQL query to insert the entity into the Database.

## @Update

7. Add a new function with `@Update` annotation that takes an `Item` as Parameter.

The entity that's updated has the same primary key as the Entity that's passed in. You can update some or all of the Entity's other Kotlin Class Properties.

8. Similar to the `insert()` method, mark this function with the `suspend` Keyword.

```kotlin
import androidx.room.Update

@Update
suspend fun update(item: Item)
```

## @Delete

Add another function with the `@Delete` annotation to delete item(s), and make it a suspending function.

**Note**: The `@Delete` annotation deletes an item or a list of items. You need to pass the entities you want to delete. If you don't have the entity, you might have to fetch it before calling the `delete()` function.

```kotlin
import androidx.room.Delete

@Delete
suspend fun delete(item: Item)
```

## @Query

There is no convenience annotation for the remaining functionality, so you have to use the `@Query` annotation and supply SQLite queries.

9. Write a SQLite query to retrieve a particular item from the item table based on the given `id`. The following code provides a sample query that selects all columns from the `items`, where the `id` matches a specific value and `id` is a unique identifier.

**Example:**
```kotlin
// Example, no need to copy over
SELECT * from items WHERE id = 1
```

10. Add a `@Query` annotation.
11. Use the SQLite query from the previous step as a string parameter to the `@Query` annotation.
12. Add a `String` parameter to the `@Query` that is a SQLite query to retrieve an item from the item table.

The query now says to select all columns from the `items`, where the `id` matches the :`id` argument. Notice the `:id` uses the colon notation in the query to reference Arguments in the function.

```kotlin
@Query("SELECT * from items WHERE id = :id")
```

13. After the `@Query` annotation, add a `getItem()` function that takes an `Int` argument and returns a `Flow<Item>`.

```kotlin
import androidx.room.Query
import kotlinx.coroutines.flow.Flow

@Query("SELECT * from items WHERE id = :id")
fun getItem(id: Int): Flow<Item>
```

It is recommended to use `Flow` in the persistence layer. With `Flow` as the return type, you receive notification whenever the data in the Database changes. The `Room` keeps this `Flow` updated for you, which means you only need to explicitly get the data once. This setup is helpful to update the inventory list, which you implement in the next codelab. Because of the `Flow` return type, Room also runs the query on the background thread. You don't need to explicitly make it a `suspend` function and call it inside a coroutine scope.

**Note**: `Flow` in Room [[Database]] can keep the data _up-to-date_ by emitting a notification whenever the data in the database changes. This allows you to observe the data and update your User Interface accordingly.

14. Add a `@Query` with a `getAllItems()` function.
15. Have the SQLite query return all columns from the `item` table, ordered in ascending order.
16. Have `getAllItems()` return a list of `Item` entities as `Flow`. `Room` keeps this `Flow` updated for you, which means you only need to explicitly get the data once.

```kotlin
@Query("SELECT * from items ORDER BY name ASC")
fun getAllItems(): Flow<List<Item>>
```

## Completed `ItemDao`:

```kotlin
import androidx.room.Dao
import androidx.room.Delete
import androidx.room.Insert
import androidx.room.OnConflictStrategy
import androidx.room.Query
import androidx.room.Update
import kotlinx.coroutines.flow.Flow

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