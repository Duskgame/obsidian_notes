In [[Kotlin]], accessors use backing fields to store the property's value in memory. Backing fields are useful when you want to add extra logic to a getter or setter, or when you want to trigger an additional action whenever the property changes.

You can't declare backing fields directly. [[Kotlin]] generates them only when necessary. You can reference the backing field in accessors using the `field` keyword.

[[Kotlin]] only generates backing fields if you use the default getter or setter, or if you use `field` in at least one custom [[Accessor]].

For example, the `isEmpty` property has no backing field because it uses a custom getter without the `field` keyword:

```
val isEmpty: Boolean
    get() = this.size == 0
```

In this example, the `score` property has a backing field because the setter uses the `field` keyword:

class Scoreboard {

    var score: Int = 0

        set(value) {

            field = value

            // Adds logging when updating the value

            println("Score updated to $field")

        }

}

​

fun main() {

    val board = Scoreboard()

    board.score = 10  

    // Score updated to 10

    board.score = 20  

    // Score updated to 20

}