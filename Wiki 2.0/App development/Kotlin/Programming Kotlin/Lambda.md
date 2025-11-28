- [[#Store a function in a variable|Store a function in a variable]]
- [[#Functions as data type|Functions as data type]]
- [[#Use a function as a return type|Use a function as a return type]]
- [[#Pass a function to another function as an argument|Pass a function to another function as an argument]]
- [[#Nullable function types|Nullable function types]]
- [[#Write Lambda expression with  shorthand syntax|Write Lambda expression with  shorthand syntax]]
	- [[#Write Lambda expression with  shorthand syntax#Omit parameter name|Omit parameter name]]
- [[#Pass a lambda expression directly into a function|Pass a lambda expression directly into a function]]
- [[#Use trailing lambda syntax|Use trailing lambda syntax]]
- [[#Use the repeat() function|Use the repeat() function]]
- [[#**Summary**|**Summary**]]
- [[#**Learn more**|**Learn more**]]


## Store a function in a variable

As a first-[[Kotlin Class|Class]] construct, functions are also data types, so you can store functions in [[Variables]], pass them to functions, and return them from functions.

to refer to a [[Function]] as a value, you need to use the function reference [[Keywords and operators|operator]] (`::`).

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



Lambda expressions provide a concise syntax to define a function without the `fun` [[Keywords and operators|keyword]]. You can store a lambda expression directly in a variable without a function reference on another function.

Before the assignment [[Keywords and operators|operator]] (`=`), you add the `val` or `var` [[Keywords and operators|keyword]] followed by the name of the variable, which is what you use when you call the function. After the assignment [[Keywords and operators|operator]] (`=`) is the lambda expression, which consists of a pair of curly braces that form the function body.

```
val variable name = {
	function body
}
```

When you define a function with a lambda expression, you have a variable that refers to the function. You can also assign its value to other variables like any other type and call the function with the new variable's name.

==With lambda expressions, you can create variables that store functions, call these variables like functions, and store them in other variables that you can call like functions.==


## Functions as data type

[[Kotlin]] has type inference. When you declare a variable, you often don't need to explicitly specify the type.

However, if you want to specify the type of a function [[Parameter]] or a return type, you need to know the syntax for expressing function types. Function types consist of a set of parentheses that contain an optional parameter list, the `->` symbol, and a return type.

```
( parameters ( optional ) ) -> return type
```

If you had a function that took two `Int` parameters and returned an `Int`, its [[Data Type]] would be `(Int, Int) -> Int`.

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

fun trickOrTreat(isTrick: Boolean): () -> [[Unit]] {
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

Like other data types, function types can be declared as nullable. In these cases, a variable could contain a function or it could be `[[null]]`.

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
``
val treatFunction = trickOrTreat(false) =={ "$it quarters" }==
``

**Note:** The composable functions that you used to declare your [[User Interface|UI]] take functions as parameters and are typically called using trailing lambda syntax.

## Use the repeat() function

When a function returns a function _or_ takes a function as an argument, it's called a higher-order function. The `trickOrTreat()` function is an example of a higher-order function because it takes a function of `((Int) -> String)?` type as a parameter and returns a function of `() -> Unit` type. Kotlin provides several useful higher-order functions, which you can take advantage of with your newfound knowledge of lambdas.

The [`repeat()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/repeat.html) function is one such higher-order function. The `repeat()` function is a concise way to express a `for` loop with functions. You use this and other higher-order functions frequently in later units. The `repeat()` function has this function signature:

```
repeat(times: Int, action: (Int) -> Unit)
```

The `times` parameter is the number of times that the action should happen. The `action` parameter is a function that takes a single `Int` parameter and returns a `Unit` type. The `action` function's `Int` parameter is the number of times that the action has executed so far, such as a `0` argument for the first iteration or a `1` argument for the second iteration. You can use the `repeat()` function to repeat code a specified number of times, similar to a `for` loop.

```
for (iteration in start...end){
	//code
}

repeat(times) {iteration -> 
	//code
}
```

```
fun main() {
    val treatFunction = trickOrTreat(false) { "$it quarters" }
    val trickFunction = trickOrTreat(true, null)
    repeat(4) {
        treatFunction()
    }
    trickFunction()
}

```

```
5 quarters
Have a treat!
Have a treat!
Have a treat!
Have a treat!
No treats!
```

## **Summary**

- Functions in Kotlin are first-class constructs and can be treated like data types.
- Lambda expressions provide a shorthand syntax to write functions.
- You can pass function types into other functions.
- You can return a function type from another function.
- A lambda expression returns the value of the last expression.
- If a parameter label is omitted in a lambda expression with a single parameter, it's referred to with the `it` identifier.
- Lambdas can be written inline without a variable name.
- If a function's last parameter is a function type, you can use trailing lambda syntax to move the lambda expression after the last parenthesis when you call a function.
- Higher-order functions are functions that take other functions as parameters or return a function.
- The `repeat()` function is a higher-order function that works similarly to a `for` loop.

## **Learn more**

- [High-order functions and lambdas](https://kotlinlang.org/docs/lambdas.html)