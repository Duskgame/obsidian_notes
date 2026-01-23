```kotlin
package com.example.inventory.data

import androidx.room.Entity
import androidx.room.PrimaryKey


/**
 * Entity data class represents a single row in the database.
 */
@Entity(tableName = "items")
data class Item(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val name: String,
    val price: Double,
    val quantity: Int
)
```

This `Item` [[Kotlin Class|Class]] is a **[[Room]] [[Entity]]**, which means it describes one table and its rows in your app’s local SQLite [[Database]].

## What this represents in the database

- `@Entity(tableName = "items")`
    - Tells [[Room]] to create a table named `items` in the app’s SQLite [[Database]].
    - Each [[Kotlin Object|Instance]] of `Item` corresponds to one row in that table.[](https://developer.android.com/training/data-storage/room)
- Columns in the `items` table:
    - `id` (primary key, `INTEGER`)
    - `name` (`TEXT`)
    - `price` (`REAL` / `DOUBLE`)
    - `quantity` (`INTEGER`)  
        [[Room]] [[Maps]] each property in the [[Data Class]] to a column in the table by default.[](https://developer.android.com/training/data-storage/room/defining-data.html)
- `@PrimaryKey(autoGenerate = true)` on `id`
    - Marks `id` as the unique identifier for each row.
    - `autoGenerate = true` lets SQLite assign the `id` automatically when inserting a new `Item` (you normally pass `id = 0`, Room/SQLite replaces it with the real value).​[](https://developer.android.com/training/data-storage/room/defining-data.html)​
## How it fits into app architecture

In a typical Room-based app:

- **Entity (this class)**
    - Defines the data schema for the table (`Item` → `items` table).
    - Used as the type for data you read/write to the database.[](https://200oksolutions.com/blog/exploring-android-room-database-with-kotlin/)
- **[[Data Access Object|DAO]] (Data Access Object)**
    - An [[Interface]] with methods like `insertItem(item: Item)`, `getAllItems(): Flow<List<Item>>`, `updateItem(item: Item)`, `deleteItem(item: Item)`.
    - Room generates the implementation and uses `Item` to map query results to objects.[](https://developer.android.com/training/data-storage/room/accessing-data)
- **RoomDatabase subclass**
    - Annotated with `@Database(entities = [Item::class], …)` to register `Item` as a table.
    - Provides access to the [[Data Access Object|DAO]](s). The app gets the [[Database]] instance and then uses the [[Data Access Object|DAO]] to interact with `Item` rows.[](https://daily.dev/blog/android-room-persistence-library-complete-guide)
- **[[Repository]] + [[ViewModel]] + UI**
    - Repository calls DAO methods to read/write `Item` rows.
    - ViewModel exposes `List<Item>` (often as `Flow` / `[[StateFlow]]`) to the UI.
    - Composables observe that [[State in Compose|State]] and display or edit items (e.g., inventory list, detail screen).

So in context: this `Item` class is the **core data model** for your inventory table in the local database; all database operations related to inventory items (listing, inserting, updating, deleting) will use this type and its mapping to the `items` table.