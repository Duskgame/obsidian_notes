
## Suspending functions﻿[](https://kotlinlang.org/docs/coroutines-basics.html#suspending-functions)

The most basic building block of [[Coroutines]] is the suspending [[Function]]. It allows a running operation to pause and resume later without affecting the structure of your code.

To declare a suspending [[Function]], use the `suspend` keyword:

```
suspend fun greet() {
    println("Hello world from a suspending function")
}
```

You can only call a suspending [[Function]] from another suspending [[Function]]. To call suspending functions at the entry point of a [[Kotlin]] application, mark the `main()` [[Function]] with the `suspend` keyword:

suspend fun main() {

    showUserInfo()

}

​

suspend fun showUserInfo() {

    println("Loading user...")

    greet()

    println("User: John Smith")

}

​

suspend fun greet() {

    println("Hello world from a suspending function")

}

[Open in Playground →](https://play.kotlinlang.org/editor/v1/N4Igxg9gJgpiBcIDOBXJAHGA7KACAZilrgLYCGAllgBQCUuwAOsbq0gBYQDuAqkjACcAklnwQ6zAL7NmqDNjyFiHbn0EixdBs1at0AqgBcANjUYgAMhDJQqAc1xpBAOlfnaO3XYEwYhiSx6BlgmZiBqAvC4AFIQ7MQAyiQUhuzuUjJYcpg4BES43r7%2B9EyBuPpGptTmABIwxsYQuFwQAsaKAhAkuGS42Qr2eVhghhQQWOlYkiAANCCGZAJ2fgAKxmSGYgIkCCAAVmQAbmSz4F3oFMaCAGqCSGNYuwBMzi9PAIwgkkA%3D%3D)

Target: JVMRunning on v.2.2.21

This example doesn't use concurrency yet, but by marking the functions with the `suspend` keyword, you allow them to call other suspending functions and run concurrent code inside.

While the `suspend` keyword is part of the core [[Kotlin]] language, most coroutine features are available through the [`kotlinx.coroutines`](https://github.com/Kotlin/kotlinx.coroutines) library.