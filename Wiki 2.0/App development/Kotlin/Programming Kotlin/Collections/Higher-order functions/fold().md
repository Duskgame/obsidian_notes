The `fold()` function is used to generate a single value from a collection. This is most commonly used for things like calculating a total of prices, or summing all the elements in a list to find an average.

The `fold()` function takes two parameters:

- An initial value. The data type is inferred when calling the function (that is, an initial value of `0` is inferred to be an `Int`).
- A lambda expression that returns a value with the same type as the initial value.


The lambda expression additionally has two parameters:

- The first is known as the accumulator. It has the same data type as the initial value. Think of this as a running total. Each time the lambda expression is called, the accumulator is equal to the return value from the previous time the lambda was called.
- The second is the same type as each element in the collection.

Like other functions you've seen, the lambda expression is called for each element in a collection, so you can use `fold()` as a concise way to sum all the elements.


```
val totalPrice = cookies.fold(0.0) {total, cookie ->
    total + cookie.price
}
```

`0.0` for the initial value
`total` for the accumulator
`cookie` for the collection element

In the lambda's body, calculate the sum of `total` and `cookie.price`. This is inferred to be the return value and is passed in for `total` the next time the lambda is called.

```
println("Total price: $${totalPrice}")
```

```
Total price: $10.83
```



**Note:** `fold()` is sometimes called `reduce()`. The `fold()` function in Kotlin works the same as the `reduce()` function found in JavaScript, Swift, Python, etc. Note that Kotlin also has its own function called [`reduce()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/reduce.html), where the accumulator starts with the first element in the collection, rather than an initial value passed as an argument.

**Note:** Kotlin collections also have a [`sum()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sum.html) function for numeric types, as well as a higher-order [`sumOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sum-of.html) function.