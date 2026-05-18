Classes sometimes only contain data. They don't have any methods that perform an action. These can be defined as a _data class_. 
Defining a [[Kotlin Class|Class]] as a data class allows the [[Kotlin]] compiler to make certain assumptions, and to automatically implement some methods. For example, `toString()` is called behind the scenes by the `println()` [[Function]]. When you use a data class, `toString()` and other methods are implemented automatically based on the class's [[Kotlin Class Properties|properties]].

To define a data class, simply add the `data` [[Keywords and operators|keyword]] before the `class` [[Keywords and operators|keyword]].

```
data class class_name (...)
```


```
data class Question<T>(
    val questionText: String,
    val answer: T,
    val difficulty: Difficulty
)

```


When a class is defined as a data class, the following methods are implemented.

- `equals()`
- `hashCode()`: you'll see this [[Kotlin Class Method|Method]] when working with certain collection types.
- `toString()`
- [`componentN()`](https://kotlinlang.org/docs/destructuring-declarations.html#example-returning-two-values-from-a-function): `component1()`, `component2()`, etc.
- `copy()`

**Note:** A data class needs to have at least one [[Parameter]] in its [[Kotlin Constructor|constructor]], and all constructor parameters must be marked with `val` or `var`. A data class also cannot be `abstract`, `open`, `sealed`, or `inner`.



To learn more about Data classes, check out the [Data classes](https://kotlinlang.org/docs/data-classes.html) documentation.