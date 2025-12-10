---
aliases:
  - " "
  - array
---
## Create arrays﻿[](https://kotlinlang.org/docs/arrays.html#create-arrays)

To create arrays in [[Kotlin]], you can use:

- functions, such as [`arrayOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of.html), [`arrayOfNulls()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of-nulls.html#kotlin$arrayOfNulls\(kotlin.Int\)) or [`emptyArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/empty-array.html).
    
- the `Array` [[Kotlin Constructor|constructor]].



```
val variable_name = arrayOf<data_type>(element1, element2, ...)
```

```
val rockPlanets = arrayOf<String>("Mercury", "Venus", "Earth", "Mars")
```

The `arrayOf()` function takes the array elements as parameters, and returns an array of the type matching the parameters passed in. This might look a little different from other functions you've seen because `arrayOf()` has a varying number of parameters. If you pass in two arguments to `arrayOf()`, the resulting array contains two elements, indexed 0 and 1. If you pass in three arguments, the resulting array will have 3 elements, indexed 0 through 2.




This example uses the `arrayOf()` function and passes item values to it:

// Creates an array with values [1, 2, 3]
```
val simpleArray = arrayOf(1, 2, 3)

println(simpleArray.joinToString())
```
// 1, 2, 3

This example uses the `arrayOfNulls()`
// Creates an array with values [null, [[null]], null]

```
val nullArray: Array<Int?> = arrayOfNulls(3)

println(nullArray.joinToString())
```
// null, null, null


Target: JVMRunning on v.2.2.21

This example uses the `emptyArray()` function to create an empty array :

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

## note

Nested arrays don't have to be the same type or the same size.