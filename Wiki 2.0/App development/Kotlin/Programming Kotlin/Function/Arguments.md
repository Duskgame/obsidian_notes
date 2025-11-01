### **Named Argubents**

In the previous examples, you didn't need to specify the [[Parameter]] names, `name` or `age`, when you called a [[Function]]. However, you're able to do so if you choose. For example, you may call a [[function]] with many [[parameter]]s or you may want to pass your arguments in a different order, such as putting the `age` [[Parameter]] before the `name` [[Parameter]]. When you include the [[Parameter]] name when you call a [[Function]], it's called a [_named argument_](https://kotlinlang.org/docs/functions.html#named-arguments). Try using a named argument with the `birthdayGreeting()` [[Function]].

```
println(birthdayGreeting(age = 2, name = "Rex"))
```

Even though you changed the order of the arguments, the same values are passed in for the same parameters.

### **Default Arguments**

[[Function]] parameters can also specify default arguments. Maybe Rover is your favorite dog, or you expect a [[Function]] to be called with specific arguments in most cases. When you call a [[Function]], you can choose to omit arguments for which there is a default, in which case, the default is used.

To add a default argument, you add the assignment operator (`=`) after the [[Data Type]] for the [[Parameter]] and set it equal to a value. Modify your code to use a default argument.

```
fun birthdayGreeting(name: String = "Rover", age: Int): String {
	return "Happy Birthday, $name! You are now $age years old!"
}
```

In the first call to `birthdayGreeting()` for Rover in `main()`, set the `age` named argument to `5`. 

Because the `age` [[Parameter]] is defined after the `name`, you need to use the named argument `age`. Without named arguments, [[Kotlin]] assumes the order of arguments is the same order in which parameters are defined. 

The named argument is used to ensure [[Kotlin]] is expecting an `Int` for the `age` [[Parameter]].

```
println(birthdayGreeting(age = 5))
println(birthdayGreeting("Rex", 2))
```
