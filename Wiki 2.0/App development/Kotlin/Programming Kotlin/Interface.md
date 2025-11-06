https://kotlinlang.org/docs/interfaces.html

Interfaces in [[Kotlin]] can contain declarations of abstract methods, as well as method implementations. 

What makes them different from abstract classes is that interfaces cannot store state. 

They can have properties, but these need to be abstract or provide [[Accessor]] implementations.


An interface is defined using the keyword `interface`:

```
interface MyInterface {
    fun bar()
    fun foo() {
      // optional body
    }
}
```

## Implementing interfaces﻿

A class or object can implement one or more interfaces:

```
class Child : MyInterface {
    override fun bar() {
        // body
    }
}
```

## Properties in interfaces﻿

You can declare properties in interfaces. A property declared in an interface can either be abstract or provide implementations for accessors. Properties declared in interfaces can't have backing fields, and therefore accessors declared in interfaces can't reference them:

```
interface MyInterface {
    val prop: Int // abstract

    val propertyWithImplementation: String
        get() = "foo"

    fun foo() {
        print(prop)
    }
}

class Child : MyInterface {
    override val prop: Int = 29
}
```

## Interfaces Inheritance﻿

An interface can derive from other interfaces, meaning it can both provide implementations for their members and declare new functions and properties. Quite naturally, classes implementing such an interface are only required to define the missing implementations:

```
interface Named {
    val name: String
}

interface Person : Named {
    val firstName: String
    val lastName: String

    override val name: String get() = "$firstName $lastName"
}

data class Employee(
    // implementing 'name' is not required
    override val firstName: String,
    override val lastName: String,
    val position: Position
) : Person
```

## Resolving overriding conflicts﻿

When you declare many types in your supertype list, you may inherit more than one implementation of the same method:

```
interface A {
    fun foo() { print("A") }
    fun bar()
}

interface B {
    fun foo() { print("B") }
    fun bar() { print("bar") }
}

class C : A {
    override fun bar() { print("bar") }
}

class D : A, B {
    override fun foo() {
        super<A>.foo()
        super<B>.foo()
    }

    override fun bar() {
        super<B>.bar()
    }
}
```

Interfaces A and B both declare functions foo() and bar(). Both of them implement foo(), but only B implements bar() (bar() is not marked as abstract in A, because this is the default for interfaces if the [[Function]] has no body). Now, if you derive a concrete class C from A, you have to override bar() and provide an implementation.

However, if you derive D from A and B, you need to implement all the methods that you have inherited from multiple interfaces, and you need to specify how exactly D should implement them. This rule applies both to methods for which you've inherited a single implementation (bar()) and to those for which you've inherited multiple implementations (foo()).

## JVM default method generation for interface functions﻿

On the JVM, functions declared in interfaces are compiled to default methods. You can control this behavior using the `-jvm-default` compiler option with the following values:

- `enable` (default): generates default implementations in interfaces and includes bridge functions in subclasses and `DefaultImpls` classes. Use this mode to maintain binary compatibility with older [[Kotlin]] versions.
    
- `no-compatibility`: generates only default implementations in interfaces. This mode skips compatibility bridges and `DefaultImpls` classes, making it suitable for new [[Kotlin]] code.
    
- `disable`: skips default methods and generates only compatibility bridges and `DefaultImpls` classes.
    

To configure the `-jvm-default` compiler option, set the `jvmDefault` property in your Gradle [[Kotlin]] DSL:

```
kotlin {
    compilerOptions {
        jvmDefault = JvmDefaultMode.NO_COMPATIBILITY
    }
}
```


why does one declare a function name in an kotin interface when the body is written in the implementation

In Kotlin, declaring a function name (without a body) in an interface sets up a contract or blueprint for implementing classes to follow. This means any class that implements the interface must provide the actual body (implementation) for the declared function.

## Why declare a function name in an interface?

- **Defines a contract:** The interface tells all implementing classes, "You must have this function with this signature."

- **Promotes consistency:** All classes that implement the interface promise to provide their own version (implementation) of the declared function, ensuring a consistent API.

- **Supports polymorphism:** Code can interact with different objects through the interface, knowing those objects supply the required functions, regardless of their specific implementations.

**Example:**



```interface ClickListener {
		fun onClick() // Function declared but not implemented 
	} 
	
	class Button : ClickListener {
		override fun onClick() {
			println("Button was clicked!")
		}
	}
```

- The `onClick` function is declared in the interface but implemented in the `Button` class.
    
- This separates the "what" (the function signature) from the "how" (the function body), providing flexibility and reusability in your code design.