## Loop over a list with `forEach()`

The [[Kotlin]] `forEach()` function executes the function passed as a parameter once for each item in the collection.

This works similarly to the `repeat()` function, or a `for` loop. The lambda is executed for the first element, then the second element, and so on, until it's executed for each element in the collection. The method signature is as follows:

```
forEach(action: (T) -> Unit)
```

`forEach()` takes a single action parameter—a function of type `(T) -> Unit`.

`T` corresponds to whatever data type the collection contains. Because the lambda takes a single parameter, you can omit the name and refer to the parameter with `it`.

```
fun main() {
    cookies.forEach {
        println("Menu item: $it")
    }
}

```

To access properties and embed them in a string, you need an expression. You can make an expression part of a string template by surrounding it with curly braces.

The lambda expression is placed between the opening and closing curly braces. You can access properties, perform math operations, call functions, etc., and the return value of the lambda is inserted into the string.

```
"${expression}"
```


```
cookies.forEach {
    println("Menu item: ${it.name}")
}
```

[forEach()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/for-each.html)