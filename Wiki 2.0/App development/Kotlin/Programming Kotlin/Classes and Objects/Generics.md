## What is a generic data type?

Generic types, or _generics_ for short, allow a data type, such as a class, to specify an unknown placeholder data type that can be used with its properties and methods. 

A generic data type is provided when instantiating a class, so it needs to be defined as part of the class signature. After the class name comes a left-facing angle bracket (`<`), followed by a placeholder name for the data type, followed by a right-facing angle bracket (`>`).

The placeholder name can then be used wherever you use a real data type within the class, such as for a property.

```
class classname <generic data type>(
	val probperty name : generic data type
)
```

This is identical to any other property declaration, except the placeholder name is used instead of the data type.

How would your class ultimately know which data type to use? The data type that the generic type uses is passed as a parameter in angle brackets when you instantiate the class.

```
val instance name = class name <generic data type> (parameters)
```

After the class name comes a left-facing angle bracket (`<`), followed by the actual data type, `String`, `Boolean`, `Int`, etc., followed by a right-facing bracket (`>`). The data type of the value that you pass in for the generic property must match the data type in the angle brackets. You'll make the answer property generic so that you can use one class to represent any type of quiz question, whether the answer is a `String`, `Boolean`, `Int`, or any arbitrary data type.

**Note:** The generic types passed in when instantiating a class are also called "parameters", although they're part of a separate parameter list than the property values placed inside the parentheses.

**Note:** Like the example above, you'll often see a generic type named `T` (short for type), or other capital letters if the class has multiple generic types. However, there is definitely not a rule and you're welcome to use a more descriptive name for generic types.

```
class Question<T>(
	val questionText: String,
	val answer: T,
	val difficulty: String)
```

```
fun main() {
    val question1 = Question<String>("Quoth the raven ___", "nevermore", "medium")
    val question2 = Question<Boolean>("The sky is green. True or false", false, "easy")
    val question3 = Question<Int>("How many days are there between full moons?", 28, "hard")
}

```