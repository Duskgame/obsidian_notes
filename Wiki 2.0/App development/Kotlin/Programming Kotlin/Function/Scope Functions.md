[[Kotlin]] includes a lot of features to make your code more concise.

One such feature is _scope functions_. Scope functions allow you to concisely access [[Kotlin Class Properties|properties]] and methods from a [[Kotlin Class|Class]] without having to repeatedly access the variable name.

## Eliminate repetitive object references with scope functions

Scope functions are higher-order functions that allow you to access properties and methods of an [[Kotlin Object|Object]] without referring to the object's name. These are called scope functions because the body of the [[Function]] passed in takes on the scope of the object that the scope function is called with. For example, some scope functions allow you to access the properties and methods in a class, as if the functions were defined as a [[Kotlin Class Method|Method]] of that class. This can make your code more readable by allowing you to omit the object name when including it is redundant.

## Replace long object names using `let()`

The `let()` function allows you to refer to an object in a [[Lambda]] expression using the identifier `it`, instead of the object's actual name. This can help you avoid using a long, more descriptive object name repeatedly when accessing more than one property. The `let()` function is an extension function that can be called on any Kotlin object using dot notation.

```
fun printQuiz() {
    println(question1.questionText)
    println(question1.answer)
    println(question1.difficulty)
    println()
    println(question2.questionText)
    println(question2.answer)
    println(question2.difficulty)
    println()
    println(question3.questionText)
    println(question3.answer)
    println(question3.difficulty)
    println()
}
```

```
fun printQuiz() {
    question1.let {
        println(it.questionText)
        println(it.answer)
        println(it.difficulty)
    }
    println()
    question2.let {
        println(it.questionText)
        println(it.answer)
        println(it.difficulty)
    }
    println()
    question3.let {
        println(it.questionText)
        println(it.answer)
        println(it.difficulty)
    }
    println()
}

```

## Call an object's methods without a variable using apply()

One of the cool features of scope functions is that you can call them on an object before that object has even been assigned to a variable. For example, the `apply()` function is an extension function that can be called on an object using dot notation. The `apply()` function also returns a reference to that object so that it can be stored in a variable.

```
Quiz().apply {
    printQuiz()
}
The `apply()` function returns the instance of the `Quiz` class, but since you're no longer using it anywhere, remove the `quiz` variable. With the `apply()` function, you don't even need a variable to call methods on the instance of `Quiz`.

```

While using scope functions isn't mandatory to achieve the desired output, the above examples illustrate how they can make your code more concise and avoid repeating the same variable name.