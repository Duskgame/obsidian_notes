An [Entity](https://developer.android.com/reference/androidx/room/Entity) class defines a table, and each instance of this class represents a row in the database table. The entity class has mappings to tell Room how it intends to present and interact with the information in the database. In the example app, the entity holds information about inventory items, such as item name, item price, and quantity of items available.

![[image-31.png|436x269]]


The `@Entity` annotation marks a class as a database Entity class. For each Entity class, the app creates a database table to hold the items. Each field of the Entity is represented as a column in the database, unless denoted otherwise (see [Entity](https://developer.android.com/reference/androidx/room/Entity) docs for details). Every entity instance stored in the database must have a primary key. The [primary key](https://developer.android.com/reference/androidx/room/PrimaryKey) is used to uniquely identify every record/entry in your database tables. After the app assigns a primary key, it cannot be modified; it represents the entity object as long as it exists in the database.

In this task, you create an Entity class and define fields to store the following inventory information for each item: an `Int` to store the primary key, a `String` to store the item name, a `double` to store the item price, and an `Int` to store the quantity in stock.

1. Open the starter code in the Android Studio.
2. Open the `data` package under the `com.example.inventory` base package.
3. Inside the `data` package, open the `Item` Kotlin class, which represents a database entity in your app.