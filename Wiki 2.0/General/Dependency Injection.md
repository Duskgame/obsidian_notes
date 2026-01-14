---
aliases:
  - " "
  - DI
---
- [Dependency injection in Android](https://developer.android.com/training/dependency-injection)

Many times, classes require objects of other classes to [[Function]]. When a [[Kotlin Class|Class]] requires another class, the required class is called a **dependency**.

In the following examples, the `Car` [[Kotlin Object|Object]] depends on an `Engine` [[Kotlin Object]].

There are two ways for a class to get these required objects. One way is for the class to instantiate the required object itself.

```
interface Engine {
    fun start()
}

class GasEngine : Engine {
    override fun start() {
        println("GasEngine started!")
    }
}

class Car {

    private val engine = GasEngine()

    fun start() {
        engine.start()
    }
}

fun main() {
    val car = Car()
    car.start()
}

```


The other way is by passing the required object in as an argument.

```
interface Engine {
    fun start()
}

class GasEngine : Engine {
    override fun start() {
        println("GasEngine started!")
    }
}

class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main() {
    val engine = GasEngine()
    val car = Car(engine)
    car.start()
}

```

Having a class instantiate the required objects is easy, but this approach makes the code inflexible and more difficult to test as the class and the required object are tightly coupled.

The calling class needs to call the object's [[Kotlin Constructor|constructor]], which is an implementation detail. If the [[Kotlin Constructor]] changes, the calling code needs to change, too.

To make the code more flexible and adaptable, ==a class must not instantiate the objects it depends on==. The objects it depends on must be instantiated outside the class and then passed in. This approach creates more flexible code, as the [[Kotlin Class]] is no longer hardcoded to one particular object. The implementation of the required object can change without needing to modify the calling code.

Continuing with the preceding example, if an `ElectricEngine` is needed, it can be created and passed into the `Car` class. The `Car` class does not need to be modified in any way.

```
interface Engine {
    fun start()
}

class ElectricEngine : Engine {
    override fun start() {
        println("ElectricEngine started!")
    }
}

class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main() {
    val engine = ElectricEngine()
    val car = Car(engine)
    car.start()
}

```

Passing in the required objects is called _dependency injection_ (DI). It is also known as [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control).

DI is when a dependency is provided at runtime instead of being hardcoded into the calling class.

Implementing dependency injection:

- **Helps with the reusability of code.** Code is not dependent on a specific object, which allows for greater flexibility.
- **Makes refactoring easier.** Code is loosely coupled, so refactoring one section of code does not impact another section of code.
- **Helps with testing.** Test objects can be passed in during testing.

One example of how DI can help with testing is when testing the network calling code. For this test, you are really trying to test that the network call is made and that data is returned. If you had to pay each time you made a network request during a test, you might decide to skip testing this code, as it can get expensive. Now, imagine if we can fake the network request for testing. How much happier (and wealthier) does that make you? For testing, you can pass a test object to the repository that returns fake data when called without actually performing a real network call.

![[image-23.png|655x278]]

We want to make the `ViewModel` testable, but it currently depends on a repository that makes actual network calls. When testing with the real production repository, it makes many network calls. To fix this issue, instead of the `ViewModel` creating the repository, we need a way to decide and pass a repository [[Kotlin Object|Instance]] to use for production and test dynamically.

This process is done by implementing an application container that provides the repository to `MarsViewModel`.

A [container](https://developer.android.com/training/dependency-injection/manual#dependencies-container) is an object that contains the dependencies that the app requires. These dependencies are used across the whole application, so they need to be in a common place that all activities can use. You can create a subclass of the Application class and store a reference to the container.

By using dependency injection, it is easier to test the `ViewModel`. Your app is now more flexible, robust, and ready to scale.