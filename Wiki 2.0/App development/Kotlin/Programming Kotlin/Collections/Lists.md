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