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

This `Item` class is a **Room entity**, which means it describes one table and its rows in your app’s local SQLite database.

## What this represents in the database

- `@Entity(tableName = "items")`
    - Tells Room to create a table named `items` in the app’s SQLite database.
    - Each instance of `Item` corresponds to one row in that table.[](https://developer.android.com/training/data-storage/room)
- Columns in the `items` table:
    - `id` (primary key, `INTEGER`)
    - `name` (`TEXT`)
    - `price` (`REAL` / `DOUBLE`)
    - `quantity` (`INTEGER`)  
        Room maps each property in the data class to a column in the table by default.[](https://developer.android.com/training/data-storage/room/defining-data.html)
- `@PrimaryKey(autoGenerate = true)` on `id`
    - Marks `id` as the unique identifier for each row.
    - `autoGenerate = true` lets SQLite assign the `id` automatically when inserting a new `Item` (you normally pass `id = 0`, Room/SQLite replaces it with the real value).​[](https://developer.android.com/training/data-storage/room/defining-data.html)​
## How it fits into app architecture

In a typical Room-based app:

- **Entity (this class)**
    - Defines the data schema for the table (`Item` → `items` table).
    - Used as the type for data you read/write to the database.[](https://200oksolutions.com/blog/exploring-android-room-database-with-kotlin/)
- **DAO (Data Access Object)**
    - An interface with methods like `insertItem(item: Item)`, `getAllItems(): Flow<List<Item>>`, `updateItem(item: Item)`, `deleteItem(item: Item)`.
    - Room generates the implementation and uses `Item` to map query results to objects.[](https://developer.android.com/training/data-storage/room/accessing-data)
- **RoomDatabase subclass**
    - Annotated with `@Database(entities = [Item::class], …)` to register `Item` as a table.
    - Provides access to the DAO(s). The app gets the database instance and then uses the DAO to interact with `Item` rows.[](https://daily.dev/blog/android-room-persistence-library-complete-guide)
- **Repository + ViewModel + UI**
    - Repository calls DAO methods to read/write `Item` rows.
    - ViewModel exposes `List<Item>` (often as `Flow` / `StateFlow`) to the UI.
    - Composables observe that state and display or edit items (e.g., inventory list, detail screen).

So in context: this `Item` class is the **core data model** for your inventory table in the local database; all database operations related to inventory items (listing, inserting, updating, deleting) will use this type and its mapping to the `items` table.