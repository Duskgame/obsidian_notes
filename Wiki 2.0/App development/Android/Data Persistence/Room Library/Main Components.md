Kotlin provides an easy way to work with data through data classes. While it is easy to work with in-memory data using data classes, when it comes to persisting data, you need to convert this data into a format compatible with Database storage. To do so, you need _tables_ to store the data and _queries_ to access and modify the data.

The following three components of [Room](https://developer.android.com/topic/libraries/architecture/room) make these workflows seamless.

- [Room entities](https://developer.android.com/training/data-storage/room/defining-data) represent tables in your app's database. You use them to update the data stored in rows in tables and to create new rows for insertion.
- Room [DAOs](https://developer.android.com/training/data-storage/room/accessing-data) provide methods that your app uses to retrieve, update, insert, and delete data in the database.
- Room [Database class](https://developer.android.com/reference/kotlin/androidx/room/Database) is the database Kotlin Class that provides your app with instances of the DAOs associated with that database.




## Add Room dependencies

In this task, you add the required [[Room]] component libraries to your Gradle files.

1. Open the module-level gradle file `build.gradle.kts (Module: InventoryApp.app)`.
2. In the `dependencies` block, add the dependencies for the [[Room]] library shown in the following code.

```
//Room
implementation("androidx.room:room-runtime:${rootProject.extra["room_version"]}")
ksp("androidx.room:room-compiler:${rootProject.extra["room_version"]}")
implementation("androidx.room:room-ktx:${rootProject.extra["room_version"]}")
```

KSP is a powerful and yet simple API for parsing Kotlin annotations.

**Note**: For the library dependencies in your Gradle file, always use the most current stable release version numbers from the [AndroidX releases](https://developer.android.com/jetpack/androidx/versions) page.
