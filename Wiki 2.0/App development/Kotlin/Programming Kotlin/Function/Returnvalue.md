
Kotlin functions can also generate data called a _return value_ which is stored in a variable that you can use elsewhere in your code. 

When defining a [[Function]], you can specify the [[data type]] of the value you want it to return. The return type is specified by placing a colon (`:`) after the parentheses, a single blank space, and then the name of the type (`Int`, `String`, etc). A single space is then placed between the return type and the opening curly brace. Within the function body, after all the statements, you use a return statement to specify the value you want the function to return. A return statement consists of the `return` keyword followed by the value, such as a variable, you want the function to return as an output.

The syntax for declaring a function with a return type is as follows.

```
fun name(): returnType {
	body
	return statement
}
```

## The `Unit` type

By default, if you don't specify a return type, the default return type is `Unit`. 
`Unit` means the function doesn't return a value.
`Unit` is equivalent to void return types in other languages 
(`void` in Java and C; `Void`/empty tuple `()` in Swift; `None` in Python, etc.). 
Any function that doesn't return a value implicitly returns `Unit`. 
You can observe this by modifying your code to return `Unit`.

```
fun main() {    
	birthdayGreeting()
}

fun birthdayGreeting(): Unit {
	println("Happy Birthday, Rover!")
	println("You are now 5 years old!")
}
```

## Return a `String`

```
fun birthdayGreeting(): String {
	val nameGreeting = "Happy Birthday, Rover!"
	val ageGreeting = "You are now 5 years old!"
	return "$nameGreeting\n$ageGreeting"}
```

```
fun main() {
	println(birthdayGreeting())
}
```
