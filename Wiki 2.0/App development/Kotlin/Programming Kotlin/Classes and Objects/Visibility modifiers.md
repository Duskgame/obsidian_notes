Visibility modifiers play an important role to achieve [[OOP|encapsulation]]:

- In a _class_, they let you hide your [[Kotlin Class Properties|properties]] and methods from unauthorized access outside the [[Kotlin Class|Class]].
- In a _package_, they let you hide the classes and interfaces from unauthorized access outside the package.

[[Kotlin]] provides four visibility modifiers:

- `public`**.** Default visibility modifier. Makes the declaration accessible everywhere. The properties and methods that you want used outside the class are marked as public.
- `private`**.** Makes the declaration accessible in the same class or source file.

There are likely some properties and methods that are only used inside the class, and that you don't necessarily want other classes to use. These properties and methods can be marked with the `private` visibility modifier to ensure that another class can't accidentally access them.

- `protected`**.** Makes the declaration accessible in subclasses. The properties and methods that you want used in the class that defines them and the subclasses are marked with the `protected` visibility modifier.
- `internal`**.** Makes the declaration accessible in the same module. The internal modifier is similar to private, but you can access internal properties and methods from outside the class as long as it's being accessed in the same module.

**Note:** A [_module_](https://developer.android.com/studio/projects#ApplicationModules) is a collection of source files and build settings that let you divide your project into discrete units of functionality. Your project can have one or many modules. You can independently build, test, and debug each module.

A _package_ is like a directory or a folder that groups related classes, whereas a module provides a container for your app's source code, resource files, and app-level settings. A module can contain multiple packages.


==Ideally, you should strive for strict visibility of properties and methods, so declare them with the `private` modifier as often as possible. If you can't keep them private, use the `protected` modifier. If you can't keep them protected, use the `internal` modifier. If you can't keep them internal, use the `public` modifier.==

|              |                              |                            |                               |                               |
| ------------ | ---------------------------- | -------------------------- | ----------------------------- | ----------------------------- |
| **Modifier** | **Accessible in same class** | **Accessible in subclass** | **Accessible in same module** | **Accessible outside module** |
| `private`    | ✔                            | 𝗫                         | 𝗫                            | 𝗫                            |
| `protected`  | ✔                            | ✔                          | 𝗫                            | 𝗫                            |
| `internal`   | ✔                            | ✔                          | ✔                             | 𝗫                            |
| `public`     | ✔                            | ✔                          | ✔                             | ✔                             |

## Specify a visibility modifier for properties

The syntax to specify a visibility modifier for a property starts with the `private`, `protected`, or `internal` modifier followed by the syntax that defines a property.

_modifier_ var name : [[Data Type]] = initial value

You can also set the visibility modifiers to setter functions. The modifier is placed before the `set` [[Keywords and operators|keyword]].

var name : data type = initial value

_modifier_ set (value) {
	body
}

**Note:** If the visibility modifier for the getter [[Function]] doesn't match with the visibility modifier for the property, the compiler reports an error.


## Visibility modifiers for methods

The syntax to specify a visibility modifier for a [[Kotlin Class Method|Method]] starts with the `private`, `protected`, or `internal` modifiers followed by the syntax that defines a method.

_modifier_ fun name () {
	body
}


## Visibility modifiers for constructors

The syntax to specify a visibility modifier for a [[Kotlin Constructor|constructor]] is similar to defining the primary constructor with a couple of differences:

- The modifier is specified after the class name, but before the `constructor` [[Keywords and operators|keyword]].
- If you need to specify the modifier for the primary constructor, it's necessary to keep the `constructor` keyword and parentheses even when there aren't any parameters.

class name _modifier_ constructor (parameters) {
	body
}

```
open class SmartDevice protected constructor (val name: String, val category: String) {

    ...

}

```


## Visibility modifiers for classes

The syntax to specify a visibility modifier for a class starts with the `private`, `protected`, or `internal` modifiers followed by the syntax that defines a class.

_modifier_ class name {
	body
}

```
internal open class SmartDevice(val name: String, val category: String) {

    ...

}

```

