The `filter()` function lets you create a subset of a collection. For example, if you had a list of numbers, you could use `filter()` to create a new list that only contains numbers divisible by 2.

Whereas the result of the `map()` function always yields a collection of the same size, `filter()` yields a collection of the same size or smaller than the original collection. Unlike `map()`, the resulting collection also has the same data type, so filtering a `List<Cookie>` will result in another `List<Cookie>`.

Like `map()` and `forEach()`, `filter()` takes a single lambda expression as a parameter. The lambda has a single parameter representing each item in the collection and returns a `Boolean` value.

For each item in the collection:

- If the result of the lambda expression is `true`, then the item is included in the new collection.
- If the result is `false`, the item is not included in the new collection.

```
val softBakedMenu = cookies.filter {
    it.softBaked
}
```

```
println("Soft cookies:")
softBakedMenu.forEach {
    println("${it.name} - $${it.price}")
}
```

```
Soft cookies:
Banana Walnut - $1.49
Snickerdoodle - $1.39
Blueberry Tart - $1.79
```

[filter()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html)