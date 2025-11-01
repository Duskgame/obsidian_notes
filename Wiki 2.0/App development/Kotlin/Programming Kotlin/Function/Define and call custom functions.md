
While [[Kotlin]] allows you to put all your code in the `main()` [[Function]], you might not always want to. For example, if you also want your program to contain a New Year's greeting, the main [[Function]] would have to include those calls to `println()` as well. Or perhaps you want to greet Rover multiple times. You could simply copy and paste the code, or you could create a separate [[Function]] for the birthday greeting. You'll do the latter. Creating separate functions for specific tasks has a number of benefits.

- **Reusable code**: Rather than copying and pasting code that you need to use more than once, you can simply call a [[Function]] wherever needed.

- **Readability**: Ensuring functions do one and only one specific task helps other developers and teammates, as well as your future self to know exactly what a piece of code does.

```
fun main() {
	birthdayGreeting()
}

fun birthdayGreeting() {
	println("Happy Birthday, Rover!")
	println("You are now 5 years old!")
}
```


## Summary

- [[Function]]s are defined with the `fun` keyword and contain reusable pieces of code.
- Functions help make larger programs easier to maintain and prevent the unnecessary repetition of code.
- Functions can give a [[returnvalue]] that you can store in a variable for later use.
- Functions can take [[parameter]]s, which are [[Variables]] available inside a [[Function]] body.
- [[Arguments]] are the values that you pass in when you call a [[Function]].
- You can name [[Arguments]] when you call a [[Function]]. When you use named [[Arguments]], you can reorder the [[Arguments]] without affecting the output.
- You can specify a default argument that lets you omit the argument when you call a [[Function]].