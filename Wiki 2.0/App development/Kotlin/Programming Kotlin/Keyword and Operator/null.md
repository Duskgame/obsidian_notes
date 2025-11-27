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

Kotlin intentionally applies syntactic rules so that it can achieve `null` _safety_, which refers to a guarantee that _no accidental calls are made on potentially_ `null` _variables_. This doesn't mean that variables can't be `null`. It means that if a member of a variable is accessed, the variable can't be `null`.

This is critical because if there's an attempt to access a member of a variable that's `null` - known as `null` _reference -_ during the running of an app, the app crashes because the `null` variable doesn't contain any property or method. This type of crash is known as a _runtime error_ in which the error happens after the code has compiled and runs.

## Use the `?.` safe call [[Keywords and operators|operator]]

You can use the `?.` safe call operator to access methods or [[Kotlin Class Properties|properties]] of nullable variables.

```
fun main() {
    var favoriteActor: String? = null
    println(favoriteActor?.length)
}
```

**Note:** You can also use the `?.` safe call operators on non-nullable variables to access a method or property. While the Kotlin compiler won't give any error for this, it's unnecessary because the access of methods or properties for non-nullable variables is always safe.


## Use the `!!` not-null assertion operator

You can also use the `!!` not-null assertion operator to access methods or properties of nullable variables.

```
(nullable variable)!!.(method/property)
```

As the name suggests, if you use the `!!` not-null assertion, it means that you assert that the value of the variable isn't `null`, regardless of whether it is or isn't.

==Unlike `?.` safe-call operators, the use of a `!!` not-null assertion operator may result in a `NullPointerException` error being thrown if the nullable variable is indeed `null`.== Thus, it should be done only when the variable is always non-nullable or proper exception handling is set in place. When not handled, exceptions cause runtime errors.


## Use the `?:` [[Elvis operator]]

The `?:` Elvis operator is an operator that you can use together with the `?.` safe-call operator. With the `?:` Elvis operator, you can add a default value when the `?.` safe-call operator returns `null`. It's similar to an `if/else` expression, but in a more idiomatic way.

If the variable _isn't_ `null`, the expression before the `?:` Elvis operator executes. If the variable _is_ `null`, the expression after the `?:` Elvis operator executes.


## Summary

- A variable can be set to `null` to indicate that it holds no value.
- Non-nullable variables cannot be assigned `null`.
- Nullable variables can be assigned `null`.
- To access methods or properties of nullable variables, you need to use `?.` safe-call operators or `!!` not-null assertion operators.
- You can use `if/else` statements with `null` checks to access nullable variables in non-nullable contexts.
- You can convert a nullable variable to a non-nullable type with `if/else` expressions.
- You can provide a default value for when a nullable variable is `null` with the `if/else` expression or the `?:` Elvis operator.

## **Learn more**

- [Null safety](https://kotlinlang.org/docs/null-safety.html)
