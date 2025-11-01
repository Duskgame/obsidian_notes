## **Variable declaration**

An _expression_ is a small unit of code that evaluates to a value. An expression can be made up of variables, [[Function]] calls, and more. In the following case, the _expression_ is made up of one variable: the `count` variable. This expression evaluates to `2`.

_Evaluate_ means determining the value of an expression. In this case, the expression evaluates to `2`. The compiler evaluates expressions in the code and uses those values when executing the instructions in the program.

```
val count: Int = 2
```

This statement creates an integer variable called `count` that holds the number `2`.

### Keyword to define new variable

To define a new variable, start with the [[Kotlin]] keyword `val` (which stands for value). Then the [[Kotlin]] compiler knows that a variable declaration is in this statement.

### **Variable name**

Just like you name a [[Function]], you also name a variable. In the variable declaration, the variable name follows the `val` keyword.
It's best to choose a name that describes the data that the variable holds so that your code is easier to read.
Variable names should follow the camel case convention

### **Variable data type**

After the variable name, you add a colon, a space, and then the [[Data Type]] of the variable.

### **Assignment operator**

In the variable declaration, the equal sign symbol (`=`) follows the [[Data Type]]. The equal sign symbol is called the _assignment operator_. The assignment operator assigns a value to the variable. In other words, the value on the right-hand side of the equal sign gets stored in the variable on the left-hand side of the equal sign.

### **Variable initial value**

The variable value is the actual data that's stored in the variable.
For the `count` variable example, the number `2` is the initial value of the variable.

You may also hear the phrase: "the `count` variable is _initialized_ to `2`." This means that `2` is the first value stored in the variable when the variable is declared.


### **String Template**

This is a _string template_ because it contains a _template expression_, which is a dollar sign (`$`) followed by a variable name. A template expression is evaluated, and its value gets substituted into the string.

```
fun main() {    
	val count: Int = 2    
	println("You have $count unread messages.")
}
```
Add a dollar sign `$` before the `count` variable. In this case, the template expression `$count` evaluates to `2`, and the `2` gets substituted into the string where the expression was located.

### **Type inference**

Here's a tip that allows you to write less code when declaring variables.

Type inference is when the [[Kotlin]] compiler can infer (or determine) what [[Data Type]] a variable should be, without the type being explicitly written in the code. That means you can omit the [[Data Type]] in a variable declaration, if you provide an initial value for the variable. The [[Kotlin]] compiler looks at the [[Data Type]] of the initial value, and assumes that you want the variable to hold data of that type.

```
val count = 2
```

The updated syntax has fewer words to type, and it achieves the same outcome of creating an `Int` variable called `count` with a value of `2`.

**Note:** If you don't provide an initial value when you declare a variable, you must specify the type.

In this line of code, no initial value is provided, so you must specify the [[Data Type]]:

`val count: Int`

In this line of code, an assigned value is provided, so you can omit the [[Data Type]]:

`val count = 2`


## **Update Variables**

If you need to update the value of a variable, declare the variable with the [[Kotlin]] keyword `var`, instead of `val`.

- `val` keyword - Use when you expect the variable value will not change.
- `var` keyword - Use when you expect the variable value can change.

With `val`, the variable is _read-only_, which means you can only read, or access, the value of the variable. Once the value is set, you cannot edit or modify its value.

With `var`, the variable is _mutable_, which means the value can be changed or modified. The value can be mutated.

To remember the difference, think of `val` as a fixed _value_ and `var` as _variable_. In [[Kotlin]], it's recommended to use the `val` keyword over the `var` keyword when possible.

#### Constant names

Constant names use UPPER_SNAKE_CASE: all uppercase letters, with words separated by underscores. But what _is_ a constant, exactly?

Constants are `val` properties with no custom `get` [[Function]], whose contents are deeply immutable, and whose functions have no detectable side-effects. This includes immutable types and immutable collections of immutable types as well as scalars and string if marked as `const`. If any of an instance’s observable state can change, it is not a constant. Merely intending to never mutate the object is not enough.

```
const val NUMBER = 5
val NAMES = listOf("Alice", "Bob")
val AGES = mapOf("Alice" to 35, "Bob" to 32)
val COMMA_JOINER = Joiner.on(',') // Joiner is immutable
val EMPTY_ARRAY = arrayOf()
```

These names are typically nouns or noun phrases.

Constant values can only be defined inside of an `object` or as a top-level declaration. Values otherwise meeting the requirement of a constant but defined inside of a `class` must use a non-constant name.

## **Increment and decrement operators**

```
count = count + 1
```

```
count++
```

```
count--
```



## **Summary**

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
- Use the `+` symbol to concatenate strings together. You can also concatenate variables of other data types like `Int` and `Boolean` to `Strings`.

### **Learn more**

- [Variables](https://play.kotlinlang.org/byExample/01_introduction/03_Variables)
- [Basic types](https://kotlinlang.org/docs/basic-types.html)
- [String templates](https://kotlinlang.org/docs/basic-syntax.html#string-templates)
- [Keywords and operators](https://kotlinlang.org/docs/keyword-reference.html)
- [Basic syntax](https://kotlinlang.org/docs/basic-syntax.html)

