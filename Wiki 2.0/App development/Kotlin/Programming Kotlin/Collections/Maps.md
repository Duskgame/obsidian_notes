- Maps work similarly to [[Sets]] and store pairs of keys and values of the specified type.

A `Map` is a collection consisting of keys and values. It's called a map because unique keys are _mapped_ to other values. A key and its accompanying value are often called a `key-value pair`.

A map's keys are unique. A map's values, however, are not. Two different keys could map to the same value. For example, `"Mercury"` has `0` moons, and `"Venus"` has `0` moons.

Accessing a value from a map by its key is generally faster than searching through a large list, such as with `indexOf()`.

Maps can be declared using the `mapOf()` or `mutableMapOf()` [[Function]]. Maps require two generic types separated by a comma—one for the keys and another for the values.

```
mutableMapOf<key_type, value_type>()
```

A map can also use type inference if it has initial values. To populate a map with initial values, each key value pair consists of the key, followed by the `to` [[Keywords and operators|operator]], followed by the value. Each pair is separated by a comma.

```
val solarSystem = mutableMapOf(
    "Mercury" to 0,
    "Venus" to 0,
    "Earth" to 1,
    "Mars" to 2,
    "Jupiter" to 79,
    "Saturn" to 82,
    "Uranus" to 27,
    "Neptune" to 14
)

```

Like [[Lists]] and sets, `Map` provides a `size` property, containing the number of key-value pairs.

```
println(solarSystem.size)
```


You can use [[subscript syntax]] to set additional key-value pairs.

```
solarSystem["Pluto"] = 5
```


You can use subscript syntax to get a value.

```
println(solarSystem["Pluto"])
```


You can also access values with the `get()` [[Kotlin Class Method|Method]]. Whether you use subscript syntax or call `get()`, it's possible that the key you pass in isn't in the map. If there isn't a key-value pair, it will return [[null]].

```
println(solarSystem.get("Theia"))
```


The `remove()` method removes the key-value pair with the specified key. It also returns the removed value, or `null`, if the specified key isn't in the map.

```
solarSystem.remove("Pluto")
```


Subscript syntax, or the `put()` method, can also modify a value for a key that already exists.

```
solarSystem["Jupiter"] = 78
==
solarSystem.put("Jupiter", 78)
```

