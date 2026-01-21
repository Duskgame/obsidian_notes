In this task, you instantiate the database and pass in the DAO instance to the `OfflineItemsRepository` class.

1. Open the `AppContainer.kt` file under the `data` package.
2. Pass in the `ItemDao()` instance to the `OfflineItemsRepository` constructor.
3. Instantiate the database instance by calling `getDatabase()` on the `InventoryDatabase` class passing in the context and call `.itemDao()` to create the instance of `Dao`.

```kotlin
override val itemsRepository: ItemsRepository by lazy {
    OfflineItemsRepository(InventoryDatabase.getDatabase(context).itemDao())
}
```

