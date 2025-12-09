An enum [[Kotlin Class|Class]] is used to create types with a limited set of possible values. In the real world, for example, the four cardinal directions—north, south, east, and west—could be represented by an enum class. There's no need, and the code shouldn't allow, for the use of any additional directions. The [[Kotlin]] syntax for an enum class is shown below.

```
enum class enum_name {
	Case1, Case2, Case3
}
```

Each possible value of an enum is called an _enum constant_. Enum constants are placed inside the curly braces separated by commas. The convention is to capitalize every letter in the constant name.

You refer to enum constants using the dot [[Keywords and operators|operator]].

```
enum_name.case_name
```


```
enum class Difficulty {
    EASY, MEDIUM, HARD
}
```

```
class Question<T>(
    val questionText: String,
    val answer: T,
    val difficulty: Difficulty
)
```

```
val question1 = Question<String>("Quoth the raven ___", "nevermore", Difficulty.MEDIUM)
val question2 = Question<Boolean>("The sky is green. True or false", false, Difficulty.EASY)
val question3 = Question<Int>("How many days are there between full moons?", 28, Difficulty.HARD)

```