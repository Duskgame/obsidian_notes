In this task, you create a [`RoomDatabase`](https://developer.android.com/reference/androidx/room/RoomDatabase) that uses your [[Entity]] and [[Data Access Object|DAO]] from the previous tasks. The Database [[Kotlin]] Class defines the list of entities and DAOs.

The [`Database`](https://developer.android.com/reference/androidx/room/Database) class provides your app with instances of the DAOs you define. In turn, the app can use the DAOs to retrieve data from the database as instances of the associated data Entity objects. The app can also use the defined data entities to update rows from the corresponding tables or to create new rows for insertion.

==You need to create an abstract `RoomDatabase` class and annotate it with @Database. This class has one Kotlin Class Method that returns the existing Kotlin Object of the `RoomDatabase` if the database doesn't exist.==

Here's the general process for getting the `RoomDatabase` instance:

- Create a `public abstract` class that extends `RoomDatabase`. The new abstract class you define acts as a database holder. The class you define is abstract because [[Room]] creates the implementation for you.
- Annotate the class with @Database. In the Arguments, list the entities for the [[Database]] and set the version number.
- Define an abstract method or property that returns an [[ItemDao]] instance, and the Room generates the implementation for you.
- You only need one instance of the `RoomDatabase` for the whole app, so make the `RoomDatabase` a singleton.
- Use Room's [`Room.databaseBuilder`](https://developer.android.com/reference/androidx/room/Room#databaseBuilder%28android.content.Context,java.lang.Class,kotlin.String%29) to create your (`item_database`) database only if it doesn't exist. Otherwise, return the existing database

## Create the Database

1. In the `data` package, create a Kotlin class `InventoryDatabase.kt`.
2. In the `InventoryDatabase.kt` file, make `InventoryDatabase` class an `abstract` class that extends `RoomDatabase`.
3. Annotate the class with `@Database`. Disregard the missing parameters error, which you fix in the next step.

```kotlin
import androidx.room.Database
import androidx.room.RoomDatabase

@Database
abstract class InventoryDatabase : RoomDatabase() {}
```

The `@Database` annotation requires several arguments so that `Room` can build the Database.

4. Specify the `Item` as the only class with the list of `entities`.
5. Set the `version` as `1`**.** Whenever you change the schema of the database table, you have to increase the version number.
6. Set `exportSchema` to `false` so as not to keep schema version history backups.

```kotlin
@Database(entities = [Item::class], version = 1, exportSchema = false)
```

7. Inside the body of the class, declare an abstract Function that returns the ItemDao so that the database knows about the Data Access Object.

```kotlin
abstract fun itemDao(): ItemDao
```

8. Below the abstract function, define a companion Kotlin Object, which allows access to the methods to create or get the database and uses the class name as the qualifier.

```kotlin
 companion object {}
```

9. Inside the [[Companion Object]], declare a private nullable variable `Instance` for the database and initialize it to `null`.

The `Instance` variable keeps a reference to the database, when one has been created. This helps maintain a single instance of the database opened at a given time, which is an expensive resource to create and maintain.

10. Annotate `Instance` with `@Volatile`.

The value of a volatile variable is never cached, and all reads and writes are to and from the main memory. These features help ensure the value of `Instance` is always up to date and is the same for all execution threads. It means that changes made by one thread to `Instance` are immediately visible to all other threads.

```kotlin
@Volatile
private var Instance: InventoryDatabase? = null
```

11. Below `Instance`, while still inside the `companion` object, define a `getDatabase()`method with a `Context` Parameter that the database builder needs.
12. Return a type `InventoryDatabase`. An error message appears because `getDatabase()` isn't returning anything yet.

```kotlin
import android.content.Context

fun getDatabase(context: Context): InventoryDatabase {}
```

Multiple threads can potentially ask for a Database instance at the same time, which results in two databases instead of one. This issue is known as a [race condition](https://en.wikipedia.org/wiki/Race_condition). Wrapping the code to get the Database inside a `synchronized` block means that only one thread of execution at a time can enter this block of code, which makes sure the Database only gets initialized once. Use `synchronized{}` block to avoid the race condition.

13. Inside `getDatabase()`, return the `Instance` variable—or, if `Instance` is null, initialize it inside a `synchronized{}` block. Use the Elvis Operator(`?:`) to do this.
14. Pass in `this`, the Companion Object. You fix the error in later steps.

```kotlin
return Instance ?: synchronized(this) { }
```

15. Inside the synchronized block, use the Database builder to get the Database. Continue to ignore the errors, which you fix in the next steps.

```kotlin
import androidx.room.Room

Room.databaseBuilder()
```

16. Inside the `synchronized` block, use the database builder to get a database. Pass in the application context, the database class, and a name for the database- `item_database` to the `Room.databaseBuilder()`.

```kotlin
Room.databaseBuilder(context, InventoryDatabase::class.java, "item_database")
```

Android Studio generates a Type Mismatch error. To remove this error, you have to add a `build()` in the following steps.

17. Add the required migration strategy to the builder. Use `.` [`fallbackToDestructiveMigration()`](https://developer.android.com/reference/androidx/room/RoomDatabase.Builder#fallbackToDestructiveMigration\(\)).

```kotlin
.fallbackToDestructiveMigration()
```

**Note**: Normally, you would provide a migration object with a migration strategy for when the schema changes. A _migration object_ is an object that defines how you take all rows with the old schema and convert them to rows in the new schema, so that no data is lost. [Migration](https://medium.com/androiddevelopers/understanding-migrations-with-room-f01e04b07929) is beyond the scope of this codelab, but the term refers to when the schema is changed and you need to move your date without losing the data. Since this is a sample app, a simple alternative is to destroy and rebuild the database, which means that the inventory data is lost. For example, if you change something in the [[Entity]] class, like adding a new parameter, you can allow the app to delete and re-initialize the database.

18. To create the database instance, call `.build()`. This call removes the [[Android]] Studio errors.

```kotlin
.build()
```

19. After `build()`, add an `also` block and assign `Instance = it` to keep a reference to the recently created db instance.

```kotlin
.also { Instance = it }
```

20. At the end of the `synchronized` block, return `instance`. Your final code looks like the following code:

```kotlin
import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase

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

**Tip:** You can use this code as a template for your future projects. The way you create the `RoomDatabase` instance is similar to the process in the previous steps. You might have to replace the entities and DAOs specific to your app.