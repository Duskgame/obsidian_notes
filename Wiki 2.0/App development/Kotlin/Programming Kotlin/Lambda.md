- [[#Store a function in a variable|Store a function in a variable]]
- [[#Functions as data type|Functions as data type]]
- [[#Use a function as a return type|Use a function as a return type]]
- [[#Pass a function to another function as an argument|Pass a function to another function as an argument]]
- [[#Nullable function types|Nullable function types]]
- [[#Write Lambda expression with  shorthand syntax|Write Lambda expression with  shorthand syntax]]
	- [[#Write Lambda expression with  shorthand syntax#Omit parameter name|Omit parameter name]]
- [[#Pass a lambda expression directly into a function|Pass a lambda expression directly into a function]]
- [[#Use trailing lambda syntax|Use trailing lambda syntax]]


## Store a function in a variable

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


## Pass a function to another function as an argument

The function that `trickOrTreat()` uses as a parameter also needs to take a parameter of its own. When declaring function types, the parameters aren't labeled. You only need to specify the data types of each parameter, separated by a comma.

```
parameters(types only)
(string, int) -> int 
```

When you write a lambda expression for a function that takes a parameter, the parameters are given names in the order that they occur. Parameter names are listed after the opening curly braces and each name is separated by a comma. An arrow (`->`) separates the parameter names from the function body. 


```
val functionName = {parameter1, parameter2 ->
	function body
}
```


```
fun trickOrTreat(isTrick: Boolean, extraTreat: (Int) -> String): () -> Unit {
    if (isTrick) {
        return trick
    } else {
        println(extraTreat(5))
        return treat
    }
}
```

```
fun main() {
    val coins: (Int) -> String = { quantity ->
        "$quantity quarters"
    }

    val cupcake: (Int) -> String = {
        "Have a cupcake!"
    }

    val treatFunction = trickOrTreat(false, coins)
    val trickFunction = trickOrTreat(true, cupcake)
    treatFunction()
    trickFunction()
}
```

**Note:** In the `coins()` function, the `Int` parameter is named `quantity`. However, it could be named anything as long as the parameter name and the variable name in the string (`quantity , "$quantity"`) are the same.


## Nullable function types

Like other data types, function types can be declared as nullable. In these cases, a variable could contain a function or it could be `null`.

To declare a function as nullable, surround the function type in parentheses followed by a `?` symbol outside the ending parenthesis. For example, if you want to make the `() -> String` type nullable, declare it as a `(() -> String)?` type.

```
((parameters(optional)) -> return type)?
```

fun trickOrTreat(isTrick: Boolean, ==extraTreat: ((Int) -> String)?==): () -> Unit {
    if (isTrick) {
        return trick
    } else {
        if (extraTreat != null) {
            println(extraTreat(5))
        }
        return treat
    }
}

fun main() {
    val coins: (Int) -> String = { quantity ->
        "$quantity quarters"
    }
	val treatFunction = trickOrTreat(false, coins)
    val trickFunction = ==trickOrTreat(true, null)==
    treatFunction()
    trickFunction()
}


## Write Lambda expression with  shorthand syntax

Lambda expressions provide a variety of ways to make your code more concise. most of the lambda expressions that you encounter and write are written with shorthand syntax.

### Omit parameter name

When you wrote the `coins()` function, you explicitly declared the name `quantity` for the function's `Int` parameter. However, as you saw with the `cupcake()` function, you can omit the parameter name entirely. When a function has a single parameter and you don't provide a name, Kotlin implicitly assigns it the `it` name, so you can omit the parameter name and    `->` symbol, which makes your lambda expressions more concise.

```
val coins: (Int) -> String = {
    "$quantity quarters"
}
```

```
val coins: (Int) -> String = {
    "$it quarters"
}
```


## Pass a lambda expression directly into a function

Lambda expressions are simply function literals, just like `0` is an integer literal or `"Hello"` is a string literal. You can pass a lambda expression directly into a function call.

fun main() {
    ==val coins: (Int) -> String = {==
        =="$it quarters"==
    ==}==
    val treatFunction = trickOrTreat(false, =={ "$it quarters" }==)
    val trickFunction = trickOrTreat(true, null)
    treatFunction()
    trickFunction()
}


## Use trailing lambda syntax

You can use another shorthand option to write lambdas ==when a function type is the last parameter of a function.== If so, you can place the lambda expression after the closing parenthesis to call the function.

This makes your code more readable because it separates the lambda expression from the other parameters, but doesn't change what the code does.

val treatFunction = trickOrTreat(false) =={ "$it quarters" }==


