The `groupBy()` function can be used to turn a list into a map, based on a function. Each unique return value of the function becomes a key in the resulting map. The values for each key are all the items in the collection that produced that unique return value.

The data type of the keys is the same as the return type of the function passed into `groupBy()`. The data type of the values is a list of items from the original list.

**Note:** The value doesn't have to be the same type of the list. There's another version of [`groupBy()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/group-by.html) that can transform the values into a different type. However, that version is not covered here.

Pass in a lambda expression that returns `it.softBaked`. The return type will be `Map<Boolean, List<Cookie>>`.

```
val groupedMenu = cookies.groupBy { it.softBaked }
```


Create a `softBakedMenu` variable containing the value of `groupedMenu[true]`, and a `crunchyMenu` variable containing the value of `groupedMenu[false]`. Because the result of subscripting a `Map` is nullable, you can use the Elvis operator (`?:`) to return an empty list.

```
val softBakedMenu = groupedMenu[true] ?: listOf()
val crunchyMenu = groupedMenu[false] ?: listOf()
```

**Note:** Alternatively, `emptyList()` creates an empty list and may be more readable.

```
println("Soft cookies:")
softBakedMenu.forEach {
    println("${it.name} - $${it.price}")
}
println("Crunchy cookies:")
crunchyMenu.forEach {
    println("${it.name} - $${it.price}")
}

```


```
Soft cookies:
Banana Walnut - $1.49
Snickerdoodle - $1.39
Blueberry Tart - $1.79
Crunchy cookies:
Chocolate Chip - $1.69
Vanilla Creme - $1.59
Chocolate Peanut Butter - $1.49
Sugar and Sprinkles - $1.39
```