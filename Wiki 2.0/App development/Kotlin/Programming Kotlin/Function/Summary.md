Use the val keyword when the value doesn't change. 
Use the var keyword when the value can change.
When you define a [[Function]], you define the parameters that can be passed to it. 
When you call a [[Function]], you pass [[Arguments]] for the parameters.

- [[Function]]s are defined with the `fun` keyword and contain reusable pieces of code.
- Functions help make larger programs easier to maintain and prevent the unnecessary repetition of code.
- Functions can give a [[returnvalue]] that you can store in a variable for later use.
- Functions can take [[parameter]]s, which are [[Variables]] available inside a [[Function]] body.
- [[Arguments]] are the values that you pass in when you call a [[Function]].
- You can name [[Arguments]] when you call a [[Function]]. When you use named [[Arguments]], you can reorder the [[Arguments]] without affecting the output.
- You can specify a default argument that lets you omit the argument when you call a [[Function]].

- A variable is a container for a single piece of data.
- You must declare a variable first before you use it.
- Use the `val` keyword to define a variable that is read-only where the value cannot change once it's been assigned.
- Use the `var` keyword to define a variable that is mutable or changeable.
- In [[Kotlin]], it's preferred to use `val` over `var` when possible.
- To declare a variable, start with the `val` or `var` keyword. Then specify the variable name, [[Data Type]], and initial value. For example: `val count: Int = 2`.
- With type inference, omit the [[Data Type]] in the variable declaration if an initial value is provided.
- Some common basic [[Kotlin]] data types include: `Int`, `String`, `Boolean`, `Float`, and `Double`.
- Use the assignment operator (`=`) to assign a value to a variable either during declaration of the variable or updating the variable.
- You can only update a variable that has been declared as a mutable variable (with `var`).
- Use the increment operator (`++`) or decrement operator (`--`) to increase or decrease the value of an integer variable by 1, respectively.
- Use the `+` symbol to concatenate strings together. You can also concatenate [[Variables]] of other data types like `Int` and `Boolean` to `Strings`.