---
aliases:
  - " "
  - DAO
---
The [Data Access Object](https://developer.android.com/reference/androidx/room/Dao) (DAO) is a pattern you can use to separate the persistence layer from the rest of the application by providing an abstract interface. This isolation follows the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle), which you have seen in previous codelabs.

The functionality of the DAO is to hide all the complexities involved in performing database operations in the underlying persistence layer, separate from the rest of the application. This lets you change the data layer independently of the code that uses the data.

![[image-32.png|587x172]]

