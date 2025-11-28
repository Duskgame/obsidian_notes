As a first-class construct, functions are also data types, so you can store functions in variables, pass them to functions, and return them from functions.

to refer to a function as a value, you need to use the function reference operator (`::`).

```
::functionName
```

```
fun main() {
    val trickFunction = ::trick
}

fun trick() {
    println("No treats!")
}
```



Lambda expressions provide a concise syntax to define a function without the `fun` keyword. You can store a lambda expression directly in a variable without a function reference on another function.

Before the assignment operator (`=`), you add the `val` or `var` keyword followed by the name of the variable, which is what you use when you call the function. After the assignment operator (`=`) is the lambda expression, which consists of a pair of curly braces that form the function body.

```
val variable name = {
	function body
}
```

When you define a function with a lambda expression, you have a variable that refers to the function. You can also assign its value to other variables like any other type and call the function with the new variable's name.

==With lambda expressions, you can create variables that store functions, call these variables like functions, and store them in other variables that you can call like functions.==


## Functions as data type

Kotlin has type inference. When you declare a variable, you often don't need to explicitly specify the type.

However, if you want to specify the type of a function parameter or a return type, you need to know the syntax for expressing function types. Function types consist of a set of parentheses that contain an optional parameter list, the `->` symbol, and a return type.

```
( parameters ( optional ) ) -> return type
```

If you had a function that took two `Int` parameters and returned an `Int`, its data type would be `(Int, Int) -> Int`.

```
val treat: () -> Unit = {
    println("Have a treat!")
}
```

## Use a function as a return type

A function is a data type, so you can use it like any other data type. You can even return functions from other functions.


```
fun functionName () : functionType {
	//code
	return (Name of another function)
}
```


fun main() {
    
}

fun trickOrTreat(isTrick: Boolean): () -> Unit {
}

==val trick = {==
    ==println("No treats!")==
==}==

==val treat = {==
    ==println("Have a treat!")==
==}==


fun trickOrTreat(isTrick: Boolean): () -> Unit {
    if (isTrick) {
        ==return trick==
    } else {
        ==return treat==
    }
}

