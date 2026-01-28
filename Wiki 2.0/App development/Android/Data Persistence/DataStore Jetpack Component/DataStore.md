https://developer.android.com/codelabs/basic-android-kotlin-compose-datastore?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-6-pathway-3%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-datastore#0

SQL and Room are powerful tools. However, in cases where you don't need to store relational data, DataStore can provide a simple solution. The DataStore Jetpack Component is a great way to store small and simple data sets with low overhead. DataStore has two different implementations, `Preferences DataStore` and `Proto DataStore`.

- `Preferences DataStore` stores key-value pairs. The values can be Kotlin's basic data types, such as `String`, `Boolean`, and `Integer`. It does not store complex datasets. It does not require a predefined schema. The primary use case of the `Preferences Datastore` is to store user preferences on their device.
- `Proto DataStore` stores custom data types. It requires a predefined schema that maps proto definitions with object structures.

`Preferences DataStore` is a great way to store user-controlled settings.

Add the following to `dependencies` in the `app/build.gradle.kts` file:

```
implementation("androidx.datastore:datastore-preferences:1.0.0")
```

