## Create arrays﻿[](https://kotlinlang.org/docs/arrays.html#create-arrays)

To create arrays in [[Kotlin]], you can use:

- functions, such as [`arrayOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of.html), [`arrayOfNulls()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of-nulls.html#kotlin$arrayOfNulls\(kotlin.Int\)) or [`emptyArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/empty-array.html).
    
- the `Array` [[Kotlin Constructor|constructor]].
    

This example uses the [`arrayOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of.html) function and passes item values to it:

// Creates an array with values [1, 2, 3]

val simpleArray = arrayOf(1, 2, 3)

println(simpleArray.joinToString())

// 1, 2, 3

[Open in Playground →](https://play.kotlinlang.org/editor/v1/N4Igxg9gJgpiBcIBmBXAdgAgLYEMCWaAFAJQbAA6alGNGA9HRgMIBOMOALjAM4Y6Y4WLHAE8MAdzwcAFhgBuOADYoeGANoBGADQYATDoDMAXWq0FijNzxYADopgBBIaIwBePs5EB5JIW17DYlMaGxYCDkUiK1t7J2ERADoAKwgCABUIAGUOMLQAcxIgzFoGDH99DANKSgBfEC0QDkE8mA4ABUVOJAgWLAQQJJwFevAIWzx7FgA1GBYrCDR%2B3QTl3Q0QGqA%3D%3D)

Target: JVMRunning on v.2.2.21

This example uses the [`arrayOfNulls()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of-nulls.html#kotlin$arrayOfNulls\(kotlin.Int\)) function to create an array of a given size filled with `[[null]]` elements:

// Creates an array with values [null, [[null]], null]

val nullArray: Array<Int?> = arrayOfNulls(3)

println(nullArray.joinToString())

// null, null, null

[Open in Playground →](https://play.kotlinlang.org/editor/v1/N4Igxg9gJgpiBcIBmBXAdgAgLYEMCWaAFAJQbAA6alGNGA9HRgMIBOMOALjAM4Y6Y4WLHAE8MAdzwcAFhgBuOADYoeGANpoUixQBoMm7XoOKAutVoLF%2BrYoCCQ0fAz3hIgDwBJNBwD8APgwAXj4HEQB5JAA5G25CAGZicxoABxYCDkUiYxdRADoAKwgCABUIAGUONLQAcxJEzFoGa0Nm3VbKSgBfEB0QDkFqmA4ABUVOJAgWLAQQfJwFHvAILGS8RRgWADUN7jwINBmAJlzjw4BGEE6gA%3D%3D%3D)

Target: JVMRunning on v.2.2.21

This example uses the [`emptyArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/empty-array.html) function to create an empty array :

```
var exampleArray = emptyArray<String>()
```

### note

You can specify the type of the empty array on the left-hand or right-hand side of the assignment due to Kotlin's type inference.

For example:

```
var exampleArray = emptyArray<String>()

var exampleArray: Array<String> = emptyArray()
```

The `Array` constructor takes the array size and a function that returns values for array elements given its index:

// Creates an Array<Int> that initializes with zeros [0, 0, 0]

val initArray = Array<Int>(3) { 0 }

println(initArray.joinToString())

// 0, 0, 0

​

// Creates an Array<String> with values ["0", "1", "4", "9", "16"]

val asc = Array(5) { i -> (i * i).toString() }

asc.forEach { print(it) }

// 014916


> ### note

> Like in most programming languages, indices start from 0 in Kotlin.

### Nested arrays﻿[](https://kotlinlang.org/docs/arrays.html#nested-arrays)

Arrays can be nested within each other to create multidimensional arrays:

// Creates a two-dimensional array

val twoDArray = Array(2) { Array<Int>(2) { 0 } }

println(twoDArray.contentDeepToString())

// [[0, 0], [0, 0]]

​

// Creates a three-dimensional array

val threeDArray = Array(3) { Array(3) { Array<Int>(3) { 0 } } }

println(threeDArray.contentDeepToString())

// [[[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0, 0], [0, 0, 0], [0, 0, 0]]]

> ### note

Nested arrays don't have to be the same type or the same size.