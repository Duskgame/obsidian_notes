The `groupBy()` function can be used to turn a list into a map, based on a function. Each unique return value of the function becomes a key in the resulting map. The values for each key are all the items in the collection that produced that unique return value.

The data type of the keys is the same as the return type of the function passed into `groupBy()`. The data type of the values is a list of items from the original list.

**Note:** The value doesn't have to be the same type of the list. There's another version of [`groupBy()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/group-by.html) that can transform the values into a different type. However, that version is not covered here.

