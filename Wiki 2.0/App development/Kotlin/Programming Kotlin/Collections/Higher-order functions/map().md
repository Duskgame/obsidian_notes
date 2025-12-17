The [[Kotlin]] `map()` function lets you transform a collection into a new collection with the same number of elements. For example, `map()` could transform a `List<Cookie>` into a `List<String>` only containing the cookie's `name`, provided you tell the `map()` function how to create a `String` from each `Cookie` item.


```
val fullMenu = cookies.map {
    "${it.name} - $${it.price}"
}
```

**Note:** There's a second `$` used before the expression. The first is treated as the dollar sign character ($) since it's not followed by a variable name or lambda expression.

Print the contents of `fullMenu`. You can do this using `forEach()`. The `fullMenu` collection returned from `map()` has type `List<String>` rather than `List<Cookie>`. Each `Cookie` in `cookies` corresponds to a `String` in `fullMenu`.

```
println("Full menu:")
fullMenu.forEach {
    println(it)
}

```

```
Full menu:
Chocolate Chip - $1.69
Banana Walnut - $1.49
Vanilla Creme - $1.59
Chocolate Peanut Butter - $1.49
Snickerdoodle - $1.39
Blueberry Tart - $1.79
Sugar and Sprinkles - $1.39
```

[map()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html)