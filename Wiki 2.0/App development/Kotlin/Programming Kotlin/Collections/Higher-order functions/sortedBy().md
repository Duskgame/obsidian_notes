When you first learned about collections, you learned that the `sort()` function could be used to sort the elements. However, this won't work on a collection of `Cookie` objects. The `Cookie` class has several properties and Kotlin won't know which properties (`name`, `price`, etc.) you want to sort by.

For these cases, Kotlin collections provide a `sortedBy()` function. `sortedBy()` lets you specify a lambda that returns the property you'd like to sort by. For example, if you'd like to sort by `price`, the lambda would return `it.price`. So long as the data type of the value has a natural sort order—strings are sorted alphabetically, numeric values are sorted in ascending order—it will be sorted just like a collection of that type.

```
val alphabeticalMenu = cookies.sortedBy {
    it.name
}
```

```
println("Alphabetical menu:")
alphabeticalMenu.forEach {
    println(it.name)
}
```

```
Alphabetical menu:
Banana Walnut
Blueberry Tart
Chocolate Chip
Chocolate Peanut Butter
Snickerdoodle
Sugar and Sprinkles
Vanilla Creme
```

**Note:** Kotlin collections also have a [`sort()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort.html) function if the data type has a natural sort order.

[sortedBy()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-by.html)