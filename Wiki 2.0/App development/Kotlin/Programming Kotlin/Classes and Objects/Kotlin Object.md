---
aliases:
  - " "
  - Object
  - Instance
---
The [[Kotlin]] runtime uses the [[Kotlin Class|Class]], or blueprint, to create an object of that particular type.
With the `SmartDevice` class, you have a blueprint of what a smart device is. To have an _actual_ smart device in your program, you need to create a `SmartDevice` object instance. The instantiation syntax starts with the class name followed by a set of parentheses as you can see in this diagram:

```
ClassName()
```

To use an object, you create the object and assign it to a variable, similar to how you define a variable. You use the `val` [[Keywords and operators|keyword]] to create an immutable variable and the `var` keyword for a mutable variable. The `val` or `var` keyword is followed by the name of the variable, then an `=` assignment operator, then the instantiation of the class object. You can see the syntax in this diagram:

```
val name = ClassName()
```

**Note:** When you define the variable with the `val` keyword to reference the object, the variable itself is read-only, but the class object remains mutable. This means that you can't reassign another object to the variable, but you can change the object's [[State in Compose|state]] when you update its [[Kotlin Class Properties|properties]]' values.