An [Entity](https://developer.android.com/reference/androidx/room/Entity) class defines a table, and each [[Kotlin Object|Instance]] of this [[Kotlin Class|Class]] represents a row in the [[Database]] table. The entity class has mappings to tell [[Room]] how it intends to present and interact with the information in the database. In the example app, the entity holds information about inventory items, such as item name, item price, and quantity of items available.

![[image-31.png|436x269]]


The `@Entity` annotation marks a class as a database Entity class. For each Entity class, the app creates a database table to hold the items. Each field of the Entity is represented as a column in the database, unless denoted otherwise (see [Entity](https://developer.android.com/reference/androidx/room/Entity) docs for details). Every entity instance stored in the database must have a primary key. The [primary key](https://developer.android.com/reference/androidx/room/PrimaryKey) is used to uniquely identify every record/entry in your database tables. After the app assigns a primary key, it cannot be modified; it represents the entity [[Kotlin Object|Object]] as long as it exists in the database.

In this task, you create an Entity class and define fields to store the following inventory information for each item: an `Int` to store the primary key, a `String` to store the item name, a `double` to store the item price, and an `Int` to store the quantity in stock.

1. Open the starter code in the [[Android]] Studio.
2. Open the `data` package under the `com.example.inventory` base package.
3. Inside the `data` package, open the `Item` [[Kotlin]] class, which represents a database entity in your app.

```kotlin
// No need to copy over, this is part of the starter code
class Item(
    val id: Int,
    val name: String,
    val price: Double,
    val quantity: Int
)
```

**Note**: As a reminder, the primary [[Kotlin Constructor]] is part of the class header in a Kotlin class. It goes after the class name (and optional type parameters).

### Data classes

Data classes are primarily used to hold data in Kotlin. They are defined with the [[Keywords and operators|keyword]] `data`. Kotlin [[Data Class]] objects have some extra benefits. For example, the compiler automatically generates utilities to compare, print, and copy such as `toString()`, [`copy()`](https://kotlinlang.org/docs/data-classes.html#copying), and `equals()`.

To ensure consistency and meaningful behavior of the generated code, data classes must fulfill the following requirements:

- The primary [[Kotlin Constructor|constructor]] must have at least one [[Parameter]].
- All primary constructor parameters must be `val` or `var`.
- Data classes cannot be `abstract`, `open`, or `sealed`.

**Warning**: The compiler only uses the [[Kotlin Class Properties|properties]] defined inside the primary constructor for the automatically generated functions. The compiler excludes properties declared inside the class body from the generated implementations.


Prefix the definition of the `Item` class with the `data` keyword to convert it to a data class.
```kotlin
data class Item(
    val id: Int,
    val name: String,
    val price: Double,
    val quantity: Int
)
```

Above the `Item` class declaration, annotate the data class with `@Entity`. Use the `tableName` argument to set the `items` as the SQLite table name.

```kotlin
import androidx.room.Entity

@Entity(tableName = "items")
data class Item(
   ...
)
```

**Note**: The `@Entity` annotation has several possible [[Arguments]]. By default (no arguments to `@Entity`), the table name is the same as the class name. Use the `tableName` argument to customize the table name. For simplicity, you use an `item`. There are several other arguments for `@Entity` you can investigate in the [Entity documentation](https://developer.android.com/reference/androidx/room/Entity).


Annotate the `id` property with `@PrimaryKey` to make the `id` the primary key. A primary key is an ID to uniquely identify every record/entry in your `Item` table

```kotlin
import androidx.room.PrimaryKey

@Entity(tableName = "items")
data class Item(
    @PrimaryKey
    val id: Int,
    ...
)
```

Assign the `id` a default value of `0`, which is necessary for the `id` to auto generate `id` values.

Add the `autoGenerate` parameter to the `@PrimaryKey` annotation to specify whether the primary key column should be auto-generated. If `autoGenerate` is set to `true`, [[Room]] will automatically generate a unique value for the primary key column when a new entity instance is inserted into the database. This ensures that each entity instance has a unique identifier, without having to manually assign values to the primary key column

```kotlin
data class Item(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    // ...
)
```

