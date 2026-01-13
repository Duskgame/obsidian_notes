**Concurrency** involves performing multiple tasks in your app at the same time. For example, your app can get data from a web server or save user data on the device, while responding to user input events and updating the UI accordingly.

To do work concurrently in your app, you will be using Kotlin **coroutines**. Coroutines allow the execution of a block of code to be suspended and then resumed later, so that other work can be done in the meantime. Coroutines make it easier to write **asynchronous** code, which means one task doesn't need to finish completely before starting the next task, enabling multiple tasks to run concurrently.

## Simple Program

In **synchronous** code, only one conceptual task is in progress at a time. You can think of it as a sequential linear path. One task must finish completely before the next one is started. Below is an example of synchronous code.

```
fun main() {
    println("Weather forecast")
    println("Sunny")
}
```

`println()` is a synchronous call because the task of printing the text to the output is completed before execution can move to the next line of code. Because each function call in `main()` is synchronous, the entire `main()` function is synchronous. Whether a function is synchronous or asynchronous is determined by the parts that it's composed of.

A synchronous function returns only when its task is fully complete. So after the last print statement in `main()` is executed, all work is done. The `main()` function returns and the program ends.


## Add a delay

Now let's pretend that getting the weather forecast of sunny weather requires a network request to a remote web server. Simulate the network request by adding a delay in the code before printing that the weather forecast is sunny.

```
import kotlinx.coroutines.*

fun main() {
    println("Weather forecast")
    delay(1000)
    println("Sunny")
}

```

`delay()` is actually a special **suspending function** provided by the Kotlin coroutines library. Execution of the `main()` function will suspend (or pause) at this point, and then resume once the specified duration of the delay is over (one second in this case).

If you try to run your program at this point, there will be a compile error: `Suspend function 'delay' should be called only from a coroutine or another suspend function`.

For the purposes of learning coroutines within the Kotlin Playground, you can wrap your existing code with a call to the `runBlocking()` function from the coroutines library. `runBlocking()` runs an event loop, which can handle multiple tasks at once by continuing each task where it left off when it's ready to be resumed.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        delay(1000)
        println("Sunny")
    }
}

```

`runBlocking()` is synchronous; it will not return until all work within its lambda block is completed. That means it will wait for the work in the `delay()` call to complete (until one second elapses), and then continue with executing the `Sunny` print statement. Once all the work in the `runBlocking()` function is complete, the function returns, which ends the program.

The output is the same as before. The code is still synchronous - it runs in a straight line and only does one thing at a time. However, the difference now is that it runs over a longer period of time due to the delay.

The "co-" in coroutine means cooperative. The code cooperates to share the underlying event loop when it suspends to wait for something, which allows other work to be run in the meantime. (The "-routine" part in "coroutine" means a set of instructions like a function.) In the case of this example, the coroutine suspends when it reaches the `delay()` call. Other work can be done in that one second when the coroutine is suspended (even though in this program, there is no other work to do). Once the duration of the delay elapses, then the coroutine resumes execution and can proceed with printing `Sunny` to the output.

**Note:** In general, only use `runBlocking()` within a `main()` function like this for learning purposes. In your Android app code, you do not need `runBlocking()` because Android provides an event loop for your app to process resumed work when it becomes ready. `runBlocking()` can be useful in your tests, however, and can let your test await specific conditions in your app before invoking the test assertions.


## Suspending functions

If the actual logic to perform the network request to get the weather data becomes more complex, you may want to extract that logic out into its own function. Let's refactor the code to see its effect.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        printForecast()
    }
}

fun printForecast() {
    delay(1000)
    println("Sunny")
}

```

If you run the program now, you will see the same compile error you saw earlier. A suspend function can only be called from a coroutine or another suspend function, so define `printForecast()` as a `suspend` function.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        printForecast()
    }
}

suspend fun printForecast() {
    delay(1000)
    println("Sunny")
}

```

Remember that `delay()` is a suspending function, and now you've made `printForecast()` a suspending function too.

A **suspending** function is like a regular function, but it can be suspended and resumed again later. To do this, suspend functions can only be called from other suspend functions that make this capability available.

A suspending function may contain zero or more suspension points. A **suspension point** is the place within the function where execution of the function can suspend. Once execution resumes, it picks up where it last left off in the code and proceeds with the rest of the function.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        printForecast()
        printTemperature()
    }
}

suspend fun printForecast() {
    delay(1000)
    println("Sunny")
}

suspend fun printTemperature() {
    delay(1000)
    println("30\u00b0C")
} 

```

In this code, the coroutine is first suspended with the delay in the `printForecast()` suspend function, and then resumes after that one-second delay. The `Sunny` text is printed to the output. The `printForecast()` function returns back to the caller.

Next the `printTemperature()` function gets called. That coroutine suspends when it reaches the `delay()` call, and then resumes one second later and finishes printing the temperature value to the output. `printTemperature()` function has completed all work and returns.

In the `runBlocking()` body, there are no further tasks to execute, so the `runBlocking()` function returns, and the program ends.

As mentioned earlier, `runBlocking()` is synchronous and each call in the body will be called sequentially. Note that a well-designed suspending function returns only once all work has been completed. As a result, these suspending functions run one after the other.

