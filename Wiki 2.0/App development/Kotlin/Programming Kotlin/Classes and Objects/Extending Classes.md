## Add an extension property

To define an extension property, add the type name and a dot [[Keywords and operators|operator]] (`.`) before the variable name.

```
val type_name.property_name: data type
property getter
```

```
val Quiz.StudentProgress.progressText: String
    get() = "${answered} of ${total} answered"

fun main() {
    println(Quiz.progressText)
}



```

**Note:** Extension [[Kotlin Class Properties|properties]] can't store data, so they must be get-only.

## Add an extension function

To define an extension [[Function]], add the type name and a dot [[Keywords and operators|operator]] (`.`) before the function name.

```
fun type_name.function_name(parameters) : return type {
	function body
}
```

```
fun Quiz.StudentProgress.printProgressBar() {
    repeat(Quiz.answered) { print("▓") }
    repeat(Quiz.total - Quiz.answered) { print("▒") }
    println()
    println(Quiz.progressText)
}

fun main() {
    Quiz.printProgressBar()
}


```


Is it mandatory to do any of this? Certainly not. However, having the option of extension properties and methods gives you more options to expose your code to other developers. Using dot syntax on other types can make your code easier to read, both for yourself and for other developers.