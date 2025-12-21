- [[#The `listOf()` function|The `listOf()` function]]
- [[#Access elements from a list|Access elements from a list]]
- [[#Iterate over list elements using a `for` loop|Iterate over list elements using a `for` loop]]
- [[#Add elements to a list|Add elements to a list]]
- [[#Update elements at a specific index|Update elements at a specific index]]
- [[#Remove elements from a list|Remove elements from a list]]

Lists are a resizable, ordered collection.

A list is an ordered, resizable collection, typically implemented as a resizable [[Arrays|Array]] . When the array is filled to capacity and you try to insert a new element, the array is copied to a new bigger array.

The collection types you'll encounter in [[Kotlin]] implement one or more interfaces.
interfaces provide a standard set of [[Kotlin Class Properties|properties]] and methods for a [[Kotlin Class|Class]] to implement. A class that implements the `List` [[Interface]] provides implementations for all the properties and methods of the `List` interface. The same is true for `MutableList`.

- `List` is an interface that defines properties and methods related to a read-only ordered collection of items.
- `MutableList` extends the `List` interface by defining methods to modify a list, such as adding and removing elements.

## The `listOf()` function

Like `arrayOf()`, the `listOf()` [[Function]] takes the items as parameters, but returns a `List` rather than an array.

```
fun main() {
    val solarSystem = listOf("Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune")
}
```

## Access elements from a list

```
println(solarSystem[2])
```
==
```
println(solarSystem.get(3))
```

In addition to getting an element by its index, you can also search for the index of a specific element using the `indexOf()` [[Kotlin Class Method|Method]]. The `indexOf()` method searches the list for a given element (passed in as an argument), and returns the index of the first occurrence of that element. If the element doesn't occur in the list, it returns `-1`.

```
println(solarSystem.indexOf("Earth"))
```


## Iterate over list elements using a `for` loop

```
for (element_name in collection_name){
	body
}
```

```
for (planet in solarSystem) {
    println(planet)
}
```


## Add elements to a list

The ability to add, remove, and update elements in a collection is exclusive to classes that implement the `MutableList` interface.

There are two versions of the `add()` function:

- The first `add()` function has a single [[Parameter]] of the type of element in the list and adds it to the end of the list.
- The other version of `add()` has two parameters. The first parameter corresponds to an index at which the new element should be inserted. The second parameter is the element being added to the list.

```
solarSystem.add("Pluto")
```

```
solarSystem.add(3, "Theia")
```

## Update elements at a specific index

You can update existing elements with [[subscript syntax]]:

```
solarSystem[3] = "Future Moon"
```


## Remove elements from a list

Elements are removed using the `remove()` or `removeAt()` method. You can either remove an element by passing it into the `remove()` method or by its index using `removeAt()`.

```
solarSystem.removeAt(9)
```

```
solarSystem.remove("Future Moon")
```

`List` provides the `contains()` method that returns a `Boolean` if an element exists in a list.

```
println(solarSystem.contains("Pluto"))
```

An even more concise syntax is to use the `in` [[Keywords and operators|operator]]. You can check if an element is in a list using the element, the `in` [[Keywords and operators|operator]], and the collection.

```
println("Future Moon" in solarSystem)
```


- [List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/)
- [Conditions and loops](https://kotlinlang.org/docs/control-flow.html)
- [MutableList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/)

Creating lists

```
val empty = emptyList<String>()
val nums = listOf(1, 2, 3, 4, 5)
val mutableNums = mutableListOf(1, 2, 3)
```

Adding/Removing (mutable only)
```
mutableNums.add(6)           // adds to end
mutableNums.add(0, 0)        // adds at index 0
mutableNums.remove(2)        // removes first occurrence
mutableNums.clear()          // removes all
```

Querying elements
```
val first = nums.first()           // 1
val last = nums.last()             // 5
val firstEven = nums.first { it % 2 == 0 }  // 2
val countEven = nums.count { it % 2 == 0 }  // 2
val hasThree = nums.contains(3)    // true
val indexThree = nums.indexOf(3)   // 2
```

Filtering
```
val evens = nums.filter { it % 2 == 0 }     // [2, 4]
val odds = nums.filterNot { it % 2 == 0 }   // [1, 3, 5]
val large = nums.filter { it > 3 }          // [4, 5]
```

Transforming
```
val doubled = nums.map { it * 2 }           // [2, 4, 6, 8, 10]
val squared = nums.map { it * it }          // [1, 4, 9, 16, 25]
val strings = nums.map { "Num $it" }        // ["Num 1", "Num 2", ...]
```

Aggregating
```
val sum = nums.sum()                        // 15
val max = nums.maxOrNull()                  // 5
val avg = nums.average()                    // 3.0
val joined = nums.joinToString()            // "1, 2, 3, 4, 5"
```

Slicing/Partitions
```
val firstTwo = nums.take(2)                 // [1, 2]
val lastTwo = nums.takeLast(2)              // [4, 5]
val middle = nums.drop(1).dropLast(1)       // [2, 3, 4]
val chunked = nums.chunked(2)               // [[1,2], [3,4], [5]]
```

```
val nums = listOf(10, 20, 30, 40, 50, 60)

// Continuous range (includes end index)
val slice1 = nums.slice(1..3)      // [20, 30, 40]

// Range with step
val slice2 = nums.slice(0..4 step 2) // [10, 30, 50]

// Specific indices list
val slice3 = nums.slice(listOf(1, 3, 5)) // [20, 40, 60]

```

Sorting/Reordering
```
val sorted = nums.sorted()                  // [1,2,3,4,5]
val desc = nums.sortedDescending()          // [5,4,3,2,1]
val reversed = nums.reversed()              // [5,4,3,2,1]
```

Grouping
```
val grouped = nums.groupBy { it % 2 }       // {0=[2,4], 1=[1,3,5]}
```

All these return new lists except mutable operations like add/remove. Use mutableListOf when you need in-place changes.