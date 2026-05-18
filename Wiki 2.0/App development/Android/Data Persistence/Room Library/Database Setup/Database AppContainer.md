In this task, you instantiate the Database and pass in the Data Access Object Kotlin Object to the OfflineItemsRepository Kotlin Class.

1. Open the [[AppContainer]].kt file under the `data` package.
2. Pass in the [[ItemDao]]() instance to the [[OfflineItemsRepository]] Kotlin Constructor.
3. Instantiate the [[Database]] instance by calling `getDatabase()` on the `InventoryDatabase` class passing in the context and call .ItemDao() to create the instance of [[Data Access Object]].

```kotlin
override val itemsRepository: ItemsRepository by lazy {
    OfflineItemsRepository(InventoryDatabase.getDatabase(context).itemDao())
}
```

