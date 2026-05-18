There are many scenarios where you want a class to only have one instance. For example:

1. Player stats in a mobile game for the current user.
2. Interacting with a single hardware device, like sending audio through a speaker.
3. An object to access a remote data source (such as a Firebase database).
4. Authentication, where only one user should be logged in at a time.

In the above scenarios, you'd probably need to use a class. However, you'll only ever need to instantiate one instance of that class. If there's only one hardware device, or only one user logged in at once, there would be no reason to create more than a single instance. Having two objects that access the same hardware device simultaneously could lead to some really strange and buggy behavior.

You can clearly communicate in your code that an object should have only one instance by defining it as a singleton. A _singleton_ is a class that can only have a single instance. Kotlin provides a special construct, called an _object_, that can be used to make a singleton class.

## Define a singleton object

```
object object_name {
	class body 1
}
```

The syntax for an object is similar to that of a class. Simply use the `object` keyword instead of the `class` keyword. A singleton object can't have a constructor as you can't create instances directly. Instead, all the properties are defined within the curly braces and are given an initial value.

```
object StudentProgress {
    var total: Int = 10
    var answered: Int = 3
}
```

## Access a singleton object

Remember how you can't create an instance of a singleton object directly? How then are you able to access its properties?

Because there's only one instance of `StudentProgress` in existence at one time, you access its properties by referring to the name of the object itself, followed by the dot operator (`.`), followed by the property name.

```
object_name.property_name
```


## Declare objects as companion objects

Classes and objects in Kotlin can be defined inside other types, and can be a great way to organize your code. You can define a singleton object inside another class using a _companion object_. A [[companion object]] allows you to access its properties and methods from inside the class, if the object's properties and methods belong to that class, allowing for more concise syntax.

To declare a companion object, simply add the `companion` keyword before the `object` keyword.

```
companion object object_name
```

```
class Quiz {
    val question1 = Question<String>("Quoth the raven ___", "nevermore", Difficulty.MEDIUM)
    val question2 = Question<Boolean>("The sky is green. True or false", false, Difficulty.EASY)
    val question3 = Question<Int>("How many days are there between full moons?", 28, Difficulty.HARD)

    companion object StudentProgress {
        var total: Int = 10
        var answered: Int = 3
    }
}

fun main() {
    println("${Quiz.answered} of ${Quiz.total} answered.")
}

```

Update the call to `println()` to reference the properties with `Quiz.answered` and `Quiz.total`. Even though these properties are declared in the `StudentProgress` object, they can be accessed with dot notation using only the name of the `Quiz` class.