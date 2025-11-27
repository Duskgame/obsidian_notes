In [[Kotlin]], there's a distinction between nullable and non-nullable types:

- Nullable types are [[Variables]] that _can_ hold `null`.
- Non-null types are variables that _can't_ hold `null`.

A type is only nullable if you explicitly let it hold `null`. As the error message says, the `String` [[Data Type]] is a non-nullable type, so you can't reassign the variable to `null`.

```
fun main() {
    var favoriteActor: String? = "Sandra Oh"    
    favoriteActor = null
}
```

To declare nullable variables in Kotlin, you need to add a `?` operator to the end of the type. For example, a `String?` type can hold either a string or `null`, whereas a `String` type can only hold a string. To declare a nullable variable, you need to explicitly add the nullable type. Without the nullable type, the Kotlin compiler infers that it's a non-nullable type.

**Note:** While you should use nullable variables for variables that can carry `null`, you should use non-nullable [[Variables]] for variables that can never carry `null` because the access of nullable variables requires more complex handling.