Composable functions are the basic building block of a [[User Interface]] in [[Jetpack Compose|Compose]]. A composable [[Function]]:

- Describes some part of your UI.
- Doesn't return anything.
- Takes some input and generates what's shown on the screen.

## Annotations

Annotations are means of attaching extra information to code. This information helps tools like the [[Jetpack Compose]] compiler, and other developers understand the app's code.

An annotation is applied by prefixing its name (the annotation) with the `@` character at the beginning of the declaration you are annotating. Different code elements, including properties, functions, and classes, can be annotated. Later on in the course, you'll learn about classes.

The following diagram is an example of annotated function:

```
Prefix character: @
Annotation: Json
Function declaration: val imgSrcUrl: String

@Json
val imgSrcUrl: String

@Volatile
private var INSTANCE: AppDatabase? = null

```

Annotations can take parameters. Parameters provide extra information to the tools processing them.

The Composable function is annotated with the [`@Composable`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/Composable) annotation. All composable functions must have this annotation. This annotation informs the Compose compiler that this function is intended to convert data into UI. As a reminder, a compiler is a special program that takes the code you wrote, looks at it line by line, and translates it into something the computer can understand (machine language).

This code snippet is an example of a simple composable function that is passed data (the `name` function [[Parameter]]) and uses it to render a text element on the screen.

```
@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!")
}
```

A few notes about the composable function:

- Jetpack Compose is built around composable functions. These functions let you define your app's UI programmatically by describing how it should look, rather than focusing on the process of the UI's construction. To create a composable function, just add the `@Composable` annotation to the function name.
- Composable functions can accept [[Arguments]], which let the app logic describe or modify the UI. In this case, your UI element accepts a `String` so that it can greet the user by name.

Composable functions can call other composable functions.

## Composable function names

The compose function that returns nothing and bears the `@Composable` annotation MUST be named using Pascal case. Pascal case refers to a naming convention in which the first letter of each word in a compound word is capitalized. The difference between Pascal case and camel case is that all words in Pascal case are capitalized. In camel case, the first word can be in either case.

The Compose function:

- _MUST_ be a noun: `DoneButton()`
- _NOT_ a verb or verb phrase: `DrawTextField()`
- _NOT_ a nouned preposition: `TextFieldWithLink()`
- _NOT_ an adjective: `Bright()`
- _NOT_ an adverb: `Outside()`
- Nouns _MAY_ be prefixed by descriptive adjectives: `RoundIcon()`