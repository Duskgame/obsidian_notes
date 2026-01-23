In this task, you instantiate the [[Database]] and pass in the [[Data Access Object|DAO]] [[Kotlin Object|Instance]] to the [[OfflineItemsRepository]] [[Kotlin Class|Class]].

1. Open the [[AppContainer]].kt file under the `data` package.
2. Pass in the [[ItemDao]]() instance to the [[OfflineItemsRepository]] [[Kotlin Constructor|constructor]].
3. Instantiate the [[Database]] instance by calling `getDatabase()` on the `InventoryDatabase` class passing in the context and call .[[ItemDao]]() to create the instance of [[Data Access Object|DAO]].

```kotlin
override val itemsRepository: ItemsRepository by lazy {
    OfflineItemsRepository(InventoryDatabase.getDatabase(context).itemDao())
}
```

