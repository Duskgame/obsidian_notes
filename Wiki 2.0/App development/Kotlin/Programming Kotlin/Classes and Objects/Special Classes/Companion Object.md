Companion objects allow you to define class-level functions and properties. This makes it easy to create factory methods, hold constants, and access shared utilities.

An object declaration inside a class can be marked with the `companion` keyword:

```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
```

Members of the `companion object` can be called simply by using the class name as the qualifier:

```kotlin
class User(val name: String) {

    // Defines a companion object that acts as a factory for creating User instances

    companion object Factory {

        fun create(name: String): User = User(name)

    }

}​

fun main(){

    // Calls the companion object's factory method using the class name as the qualifier. 

    // Creates a new User instance

    val userInstance = User.create("John Doe")

    println(userInstance.name)

    // John Doe

}
```


The name of the `companion object` can be omitted, in which case the name `Companion` is used:

```kotlin
class User(val name: String) {
    // Defines a companion object without a name
    companion object { }
}

// Accesses the companion object
val companionUser = User.Companion
```

Class members can access `private` members of their corresponding `companion object`:

```kotlin
class User(val name: String) {
    companion object {
        private val defaultGreeting = "Hello"
    }

    fun sayHi() {
        println(defaultGreeting)
    }
}
User("Nick").sayHi()
// Hello
```

When a class name is used by itself, it acts as a reference to the companion object of the class, regardless of whether the companion object is named or not:

```kotlin
class User1 {

    // Defines a named companion object

    companion object Named {

        fun show(): String = "User1's Named Companion Object"

    }

}​

// References the companion object of User1 using the class name

val reference1 = User1​

class User2 {

    // Defines an unnamed companion object

    companion object {

        fun show(): String = "User2's Companion Object"

    }

}

​

// References the companion object of User2 using the class name

val reference2 = User2
```

Although members of companion objects in Kotlin look like static members from other languages, they are actually instance members of the companion object, meaning they belong to the object itself. This allows companion objects to implement interfaces:


```kotlin
interface Factory<T> {

    fun create(name: String): T

}

​

class User(val name: String) {

    // Defines a companion object that implements the Factory interface

    companion object : Factory<User> {

        override fun create(name: String): User = User(name)

    }

}

​

fun main() {

    // Uses the companion object as a Factory

    val userFactory: Factory<User> = User

    val newUser = userFactory.create("Example User")

    println(newUser.name)

    // Example User

}
```

However, on the JVM, you can have members of companion objects generated as real static methods and fields if you use the `@JvmStatic` annotation. See the [Java interoperability](https://kotlinlang.org/docs/java-to-kotlin-interop.html#static-fields) section for more detail.