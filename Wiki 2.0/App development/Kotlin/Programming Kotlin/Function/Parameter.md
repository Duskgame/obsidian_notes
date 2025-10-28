
A parameter specifies the name of a [[Variables]] and a [[Data Type]] that you can pass into a [[Function]] as data to be accessed inside the function. Parameters are declared within the parentheses after the function name.

Within the parentheses of the `birthdayGreeting()` function, add a `name` parameter of type `String`, using the syntax `name: String`.

```
fun birthdayGreeting(name: String): String {
	val nameGreeting = "Happy Birthday, $name!"
	val ageGreeting = "You are now 5 years old!"
	return "$nameGreeting\n$ageGreeting"
}
```

The parameter defined in the previous step works like a variable declared with the `val` keyword. Its value can be used anywhere in the `birthdayGreeting()` function.

When you call a function that takes a parameter, you pass an argument to the function. The argument is the value that you pass

```
fun main() {
	println(birthdayGreeting("Rover"))
}
```


***Note:*** Although you often find them used interchangeably, a parameter and an argument aren't the same thing. When you define a function, you define the parameters that are to be passed to it when the function is called. 
When you call a function, you pass arguments for the parameters. Parameters are the variables accessible to the function, such as a `name` variable, while [[arguments]] are the actual values that you pass, such as the `"Rover"` string.

***Warning:*** Unlike in some languages, such as Java, where a function can change the value passed into a parameter, 
**parameters in Kotlin are immutable.** 
You cannot reassign the value of a parameter from within the function body.

