Sets are unordered collections and cannot contain duplicates.

A set is a collection that does not have a specific order and does not allow duplicate values.

![[Pasted image 20251210115935.png]]

How is a collection like this possible? The secret is a _hash code_. A hash code is an `Int` produced by the `hashCode()` [[Kotlin Class Method|Method]] of any [[Kotlin]] class. It can be thought of as a semi-unique identifier for a [[Kotlin Object]]. A small change to the object, such as adding one character to a `String`, results in a vastly different hash value. While it's possible for two objects to have the same hash code (called a hash collision), the `hashCode()` [[Function]] ensures some degree of uniqueness, where most of the time, two different values each have a unique hash code.


**Note:** A set uses hash codes as [[Arrays|array]] indices. Of course, there can be about 4 billion different hash codes, so a `Set` isn't just one giant array. Instead, you can think of a `Set` as an array of [[Lists]].

![[Pasted image 20251210120144.png]]

The outer array—the numbers outlined in blue on the left—each correspond to a range (also known as a bucket) of possible hash codes. Each inner list—shaded in green on the right—represents the individual items in the set. Since hash collisions are relatively uncommon, even when the potential indices are limited, the inner lists at each array index will only have one or two items each, unless tens or hundreds of thousands of elements are added.

Sets have two important [[Kotlin Class Properties|properties]]:

1. Searching for a specific element in a set is fast—compared with lists—especially for large [[Collections]]. While the `indexOf()` of a `List` requires checking each element from the beginning until a match is found, on average, it takes the same amount of time to check if an element is in a set, whether it's the first element or the hundred thousandth.
2. Sets tend to use more memory than lists for the same amount of data, since more array indices are often needed than the data in the set.

**Note:** Contrary to popular belief, the time it takes to check if a set contains an element is not fixed, and does in fact, depend on the amount of data in the set. However, as there will usually be few hash collisions, the number of elements that need to be checked is still orders of magnitude smaller than searching for an item in a list.

The benefit of sets is ensuring uniqueness. If you were writing a program to keep track of newly discovered planets, a set provides a simple way to check if a planet has already been discovered. With large amounts of data, this is often preferable to checking if an element exists in a list, which requires iterating over all the elements.

Like `List` and `MutableList`, there's both a `Set` and a `MutableSet`. `MutableSet` implements `Set`, so any class implementing `MutableSet` needs to implement both.


## Use a `MutableSet` in Kotlin

Create a `Set` of planets called `solarSystem` using `mutableSetOf()`. This returns a `MutableSet`, the default implementation of which is `LinkedHashSet()`.

```
val solarSystem = mutableSetOf("Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune")
```

```
println(solarSystem.size)
```

Like `MutableList`, `MutableSet` has an `add()` method. Elements in sets don't necessarily have an order, so there's no index!

```
solarSystem.add("Pluto")
```

The `contains()` function takes a single [[Parameter]] and checks if the specified element is contained in the set. If so, it returns true. Otherwise, it returns false.

```
println(solarSystem.contains("Pluto"))
```

**Note**: Alternatively, you can use the `in` [[Keywords and operators|operator]] to check if an element is in a collection, for example: `"Pluto" in solarSystem` is equivalent to `solarSystem.contains("Pluto")`.

The `remove()` function takes a single parameter and removes the specified element from the set.

```
solarSystem.remove("Pluto")
```