---
aliases:
  - " "
  - keyword
  - operator
  - keywords
  - operators
---
## Hard keywords﻿[](https://kotlinlang.org/docs/keyword-reference.html#hard-keywords)

The following tokens are always interpreted as keywords and cannot be used as identifiers:

- `as`
    
    - is used for [type casts](https://kotlinlang.org/docs/typecasts.html#unsafe-cast-operator).
        
    - specifies an [alias for an import](https://kotlinlang.org/docs/packages.html#imports)
        
- `as?` is used for [safe type casts](https://kotlinlang.org/docs/typecasts.html#safe-nullable-cast-operator).
    
- `break` [terminates the execution of a loop](https://kotlinlang.org/docs/returns.html).
    
- `class` declares a [class](https://kotlinlang.org/docs/classes.html).
    
- `continue` [proceeds to the next step of the nearest enclosing loop](https://kotlinlang.org/docs/returns.html).
    
- `do` begins a [do/while loop](https://kotlinlang.org/docs/control-flow.html#while-loops) (a loop with a postcondition).
    
- `else` defines the branch of an [if expression](https://kotlinlang.org/docs/control-flow.html#if-expression) that is executed when the condition is false.
    
- `false` specifies the 'false' value of the [Boolean type](https://kotlinlang.org/docs/booleans.html).
    
- `for` begins a [for loop](https://kotlinlang.org/docs/control-flow.html#for-loops).
    
- `fun` declares a [function](https://kotlinlang.org/docs/functions.html).
    
- `if` begins an [if expression](https://kotlinlang.org/docs/control-flow.html#if-expression).
    
- `in`
    
    - specifies the object being iterated in a [for loop](https://kotlinlang.org/docs/control-flow.html#for-loops).
        
    - is used as an infix operator to check that a value belongs to [a range](https://kotlinlang.org/docs/ranges.html), a collection, or another entity that [defines a 'contains' method](https://kotlinlang.org/docs/operator-overloading.html#in-operator).
        
    - is used in [when expressions](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) for the same purpose.
        
    - marks a type parameter as [contravariant](https://kotlinlang.org/docs/generics.html#declaration-site-variance).
        
- `!in`
    
    - is used as an operator to check that a value does NOT belong to [a range](https://kotlinlang.org/docs/ranges.html), a collection, or another entity that [defines a 'contains' method](https://kotlinlang.org/docs/operator-overloading.html#in-operator).
        
    - is used in [when expressions](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) for the same purpose.
        
- `interface` declares an [interface](https://kotlinlang.org/docs/interfaces.html).
    
- `is`
    
    - checks that [a value has a certain type](https://kotlinlang.org/docs/typecasts.html#is-and-is-operators).
        
    - is used in [when expressions](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) for the same purpose.
        
- `!is`
    
    - checks that [a value does NOT have a certain type](https://kotlinlang.org/docs/typecasts.html#is-and-is-operators).
        
    - is used in [when expressions](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) for the same purpose.
        
- `null` is a constant representing an object reference that doesn't point to any object. [[null]]
    
- `object` declares [a class and its instance at the same time](https://kotlinlang.org/docs/object-declarations.html).
    
- `package` specifies the [package for the current file](https://kotlinlang.org/docs/packages.html).
    
- `return` [returns from the nearest enclosing function or anonymous function](https://kotlinlang.org/docs/returns.html).
    
- `super`
    
    - [refers to the superclass implementation of a method or property](https://kotlinlang.org/docs/inheritance.html#calling-the-superclass-implementation).
        
    - [calls the superclass [[Kotlin Constructor|constructor]] from a secondary constructor](https://kotlinlang.org/docs/classes.html#inheritance).
        
- `this`
    
    - refers to [the current receiver](https://kotlinlang.org/docs/this-expressions.html).
        
    - [calls another constructor of the same class from a secondary constructor](https://kotlinlang.org/docs/classes.html#constructors-and-initializer-blocks).
        
- `throw` [throws an exception](https://kotlinlang.org/docs/exceptions.html).
    
- `true` specifies the 'true' value of the [Boolean type](https://kotlinlang.org/docs/booleans.html).
    
- `try` [begins an exception-handling block](https://kotlinlang.org/docs/exceptions.html).
    
- `typealias` declares a [type alias](https://kotlinlang.org/docs/type-aliases.html).
    
- `typeof` is reserved for future use.
    
- `val` declares a read-only [property](https://kotlinlang.org/docs/properties.html) or [local variable](https://kotlinlang.org/docs/basic-syntax.html#variables).
    
- `var` declares a mutable [property](https://kotlinlang.org/docs/properties.html) or [local variable](https://kotlinlang.org/docs/basic-syntax.html#variables).
    
- `when` begins a [when expression](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) (executes one of the given branches).
	- **Note:** A `when` statement doesn't need the `else` branch to be defined. However, in most cases, a `when` expression requires the `else` branch because the `when` expression needs to return a value. As such, the Kotlin compiler checks whether all the branches are exhaustive. An `else` branch ensures that there won't be a scenario in which the variable doesn't get assigned a value.
    
- `while` begins a [while loop](https://kotlinlang.org/docs/control-flow.html#while-loops) (a loop with a precondition).
    

## Soft keywords﻿[](https://kotlinlang.org/docs/keyword-reference.html#soft-keywords)

The following tokens act as keywords in the context in which they are applicable, and they can be used as identifiers in other contexts:

- `by`
    
    - [delegates the implementation of an interface to another object](https://kotlinlang.org/docs/delegation.html).
        
    - [delegates the implementation of the accessors for a property to another object](https://kotlinlang.org/docs/delegated-properties.html).
        
- `catch` begins a block that [handles a specific exception type](https://kotlinlang.org/docs/exceptions.html).
    
- `constructor` declares a [primary or secondary constructor](https://kotlinlang.org/docs/classes.html#constructors-and-initializer-blocks).
    
- `delegate` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `dynamic` references a [dynamic type](https://kotlinlang.org/docs/dynamic-type.html) in Kotlin/JS code.
    
- `field` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `file` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `finally` begins a block that [is always executed when a try block exits](https://kotlinlang.org/docs/exceptions.html).
    
- `get`
    
    - declares the [getter of a property](https://kotlinlang.org/docs/properties.html).
        
    - is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
        
- `import` [imports a declaration from another package into the current file](https://kotlinlang.org/docs/packages.html).
    
- `init` begins an [initializer block](https://kotlinlang.org/docs/classes.html#constructors-and-initializer-blocks).
    
- `param` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `property` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `receiver` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `set`
    
    - declares the [setter of a property](https://kotlinlang.org/docs/properties.html).
        
    - is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
        
- `setparam` is used as an [annotation use-site target](https://kotlinlang.org/docs/annotations.html#annotation-use-site-targets).
    
- `value` with the `class` keyword declares an [inline class](https://kotlinlang.org/docs/inline-classes.html).
    
- `where` specifies the [constraints for a generic type parameter](https://kotlinlang.org/docs/generics.html#upper-bounds).
    

## Modifier keywords﻿[](https://kotlinlang.org/docs/keyword-reference.html#modifier-keywords)

The following tokens act as keywords in modifier lists of declarations, and they can be used as identifiers in other contexts:

- `abstract` marks a class or member as [abstract](https://kotlinlang.org/docs/classes.html#abstract-classes).
    
- `actual` denotes a platform-specific implementation in [multiplatform projects](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html).
    
- `annotation` declares an [annotation class](https://kotlinlang.org/docs/annotations.html).
    
- `companion` declares a [companion object](https://kotlinlang.org/docs/object-declarations.html#companion-objects).
    
- `const` marks a property as a [compile-time constant](https://kotlinlang.org/docs/properties.html#compile-time-constants).
    
- `crossinline` forbids [non-local returns in a lambda passed to an inline function](https://kotlinlang.org/docs/inline-functions.html#returns).
    
- `data` instructs the compiler to [generate canonical members for a class](https://kotlinlang.org/docs/data-classes.html).
    
- `enum` declares an [enumeration](https://kotlinlang.org/docs/enum-classes.html).
    
- `expect` marks a declaration as [platform-specific](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html), expecting an implementation in platform modules.
    
- `external` marks a declaration as implemented outside of Kotlin (accessible through [JNI](https://kotlinlang.org/docs/java-interop.html#using-jni-with-kotlin) or in [JavaScript](https://kotlinlang.org/docs/js-interop.html#external-modifier)).
    
- `final` forbids [overriding a member](https://kotlinlang.org/docs/inheritance.html#overriding-methods).
    
- `infix` allows calling a function using [infix notation](https://kotlinlang.org/docs/functions.html#infix-notation).
    
- `inline` tells the compiler to [inline a function and the lambdas passed to it at the call site](https://kotlinlang.org/docs/inline-functions.html).
    
- `inner` allows referring to an outer class instance from a [nested class](https://kotlinlang.org/docs/nested-classes.html).
    
- `internal` marks a declaration as [visible in the current module](https://kotlinlang.org/docs/visibility-modifiers.html).
    
- `lateinit` allows initializing a [non-nullable property outside of a constructor](https://kotlinlang.org/docs/properties.html#late-initialized-properties-and-variables).
    
- `noinline` turns off [inlining of a lambda passed to an inline function](https://kotlinlang.org/docs/inline-functions.html#noinline).
    
- `open` allows [subclassing a class or overriding a member](https://kotlinlang.org/docs/classes.html#inheritance).
    
- `operator` marks a function as [overloading an operator or implementing a convention](https://kotlinlang.org/docs/operator-overloading.html).
    
- `out` marks a type parameter as [covariant](https://kotlinlang.org/docs/generics.html#declaration-site-variance).
    
- `override` marks a member as an [override of a superclass member](https://kotlinlang.org/docs/inheritance.html#overriding-methods).
    
- `private` marks a declaration as [visible in the current class or file](https://kotlinlang.org/docs/visibility-modifiers.html).
    
- `protected` marks a declaration as [visible in the current class and its subclasses](https://kotlinlang.org/docs/visibility-modifiers.html).
    
- `public` marks a declaration as [visible anywhere](https://kotlinlang.org/docs/visibility-modifiers.html).
    
- `reified` marks a type parameter of an inline function as [accessible at runtime](https://kotlinlang.org/docs/inline-functions.html#reified-type-parameters).
    
- `sealed` declares a [sealed class](https://kotlinlang.org/docs/sealed-classes.html) (a class with restricted subclassing).
    
- `suspend` marks a function or lambda as suspending (usable as a [coroutine](https://kotlinlang.org/docs/coroutines-overview.html)).
    
- `tailrec` marks a function as [tail-recursive](https://kotlinlang.org/docs/functions.html#tail-recursive-functions) (allowing the compiler to replace recursion with iteration).
    
- `vararg` allows [passing a variable number of arguments for a parameter](https://kotlinlang.org/docs/functions.html#variable-number-of-arguments-varargs).
    

## Special identifiers﻿[](https://kotlinlang.org/docs/keyword-reference.html#special-identifiers)

The following identifiers are defined by the compiler in specific contexts, and they can be used as regular identifiers in other contexts:

- `field` is used inside a property accessor to refer to the [backing field of the property](https://kotlinlang.org/docs/properties.html#backing-fields).
    
- `it` is used inside a lambda to [refer to its parameter implicitly](https://kotlinlang.org/docs/lambdas.html#it-implicit-name-of-a-single-parameter).
    

## Operators and special symbols﻿[](https://kotlinlang.org/docs/keyword-reference.html#operators-and-special-symbols)

Kotlin supports the following operators and special symbols:

- `+`, `-`, `*`, `/`, `%` - mathematical operators
    
    - `*` is also used to [pass an array to a vararg parameter](https://kotlinlang.org/docs/functions.html#variable-number-of-arguments-varargs).
        
- `=`
    
    - assignment operator.
        
    - is used to specify [default values for parameters](https://kotlinlang.org/docs/functions.html#parameters-with-default-values).
        
- `+=`, `-=`, `*=`, `/=`, `%=` - [augmented assignment operators](https://kotlinlang.org/docs/operator-overloading.html#augmented-assignments).
    
- `++`, `--` - [increment and decrement operators](https://kotlinlang.org/docs/operator-overloading.html#increments-and-decrements).
    
- `&&`, `||`, `!` - logical 'and', 'or', 'not' operators (for bitwise operations, use the corresponding [infix functions](https://kotlinlang.org/docs/numbers.html#operations-on-numbers) instead).
    
- `==`, `!=` - [equality operators](https://kotlinlang.org/docs/operator-overloading.html#equality-and-inequality-operators) (translated to calls of `equals()` for non-primitive types).
    
- `===`, `!==` - [referential equality operators](https://kotlinlang.org/docs/equality.html#referential-equality).
    
- `<`, `>`, `<=`, `>=` - [comparison operators](https://kotlinlang.org/docs/operator-overloading.html#comparison-operators) (translated to calls of `compareTo()` for non-primitive types).
    
- `[`, `]` - [indexed access operator](https://kotlinlang.org/docs/operator-overloading.html#indexed-access-operator) (translated to calls of `get` and `set`).
    
- `!!` [asserts that an expression is non-nullable](https://kotlinlang.org/docs/null-safety.html#not-null-assertion-operator).
    
- `?.` performs a [safe call](https://kotlinlang.org/docs/null-safety.html#safe-call-operator) (calls a method or accesses a property if the receiver is non-nullable).
    
- `?:` takes the right-hand value if the left-hand value is null (the [elvis operator](https://kotlinlang.org/docs/null-safety.html#elvis-operator)).
    
- `::` creates a [member reference](https://kotlinlang.org/docs/reflection.html#function-references) or a [class reference](https://kotlinlang.org/docs/reflection.html#class-references).
    
- `..`, `..<` create [ranges](https://kotlinlang.org/docs/ranges.html).
    
- `:` separates a name from a type in a declaration.
    
- `?` marks a type as [nullable](https://kotlinlang.org/docs/null-safety.html#nullable-types-and-non-nullable-types).
    
- `->`
    
    - separates the parameters and body of a [lambda expression](https://kotlinlang.org/docs/lambdas.html#lambda-expression-syntax).
        
    - separates the parameters and return type declaration in a [function type](https://kotlinlang.org/docs/lambdas.html#function-types).
        
    - separates the condition and body of a [when expression](https://kotlinlang.org/docs/control-flow.html#when-expressions-and-statements) branch.
        
- `@`
    
    - introduces an [annotation](https://kotlinlang.org/docs/annotations.html#usage).
        
    - introduces or references a [loop label](https://kotlinlang.org/docs/returns.html#break-and-continue-labels).
        
    - introduces or references a [lambda label](https://kotlinlang.org/docs/returns.html#return-to-labels).
        
    - references a ['this' expression from an outer scope](https://kotlinlang.org/docs/this-expressions.html#qualified-this).
        
    - references an [outer superclass](https://kotlinlang.org/docs/inheritance.html#calling-the-superclass-implementation).
        
- `;` separates multiple statements on the same line.
    
- `$` references a variable or expression in a [string template](https://kotlinlang.org/docs/strings.html#string-templates).
    
- `_`
    
    - substitutes an unused parameter in a [lambda expression](https://kotlinlang.org/docs/lambdas.html#underscore-for-unused-variables).
        
    - substitutes an unused parameter in a [destructuring declaration](https://kotlinlang.org/docs/destructuring-declarations.html#underscore-for-unused-variables).
        

For operator precedence, see [this reference](https://kotlinlang.org/docs/reference/grammar.html#expressions) in Kotlin grammar.