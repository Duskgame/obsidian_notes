---
aliases:
  - " "
  - Class
---
==When you define a class, you specify the properties and methods that all objects of that class should have.==

==A class is a blueprint for an [[Kotlin Object|Object]].==

A class definition starts with the `class` [[Keywords and operators|keyword]], followed by a name and a set of curly braces. The part of the syntax before the opening curly brace is also referred to as the class header. In the curly braces, you can specify properties and functions for the class.

class _name_ {
	_body_
}

These are the recommended naming conventions for a class:

- You can choose any class name that you want, but don't use [[Kotlin]] [keywords](https://kotlinlang.org/docs/keyword-reference.html) as a class name, such as the `fun` keyword.
- The class name is written in PascalCase, so each word begins with a capital letter and there are no spaces between the words. For example, in SmartDevice, the first letter of each word is capitalized and there isn't a space between the words.

A class consists of three major parts:

- **Properties.** [[Variables]] that specify the attributes of the class's objects.
- **Methods.** Functions that contain the class's behaviors and actions.
- **Constructors.** A special member [[Function]] that creates instances of the class throughout the program in which it's defined.