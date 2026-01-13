## **launch()**

Use the `launch()` function from the [[Coroutines]] library to launch a new coroutine. To execute tasks concurrently, add multiple `launch()` functions to your code so that multiple coroutines can be in progress at the same time.

Coroutines in [[Kotlin]] follow a key concept called [**structured concurrency**](https://kotlinlang.org/docs/coroutines-basics.html#structured-concurrency), where your code is sequential by default and cooperates with an underlying event loop, unless you explicitly ask for concurrent execution (e.g. using `launch()`). The assumption is that if you call a [[Function]], it should finish its work completely by the time it returns regardless of how many coroutines it may have used in its implementation details. Even if it fails with an exception, once the exception is thrown, there are no more pending tasks from the function. Hence, all work is finished once control flow returns from the function, whether it threw an exception or completed its work successfully. 

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        launch {
            printForecast()
        }
        launch {
            printTemperature()
        }
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

The output is the same but you may have noticed that it is faster to run the program. Previously, you had to wait for the `printForecast()` suspend function to finish completely before moving onto the `printTemperature()` function. Now `printForecast()` and `printTemperature()` can run concurrently because they are in separate coroutines.




The call to `launch { printForecast() }` can return before all the work in `printForecast()` is completed. That is the beauty of coroutines. You can move onto the next `launch()` call to start the next coroutine. Similarly, the `launch { printTemperature() }` also returns even before all work is completed.

You can see that the execution time has gone down from ~ 2.1 seconds to ~ 1.1 seconds, so it's faster to execute the program once you add concurrent operations!


```
...

fun main() {
    runBlocking {
        println("Weather forecast")
        launch {
            printForecast()
        }
        launch {
            printTemperature()
        }
        println("Have a good day!")
    }
}

...

--->
Weather forecast
Have a good day!
Sunny
30°C

```

From this output, you can observe that after the two new coroutines are launched for `printForecast()` and `printTemperature()`, you can proceed with the next instruction which prints `Have a good day!`. This demonstrates the "fire and forget" nature of `launch()`. You fire off a new coroutine with `launch()`, and don't have to worry about when its work is finished.

Later the coroutines will complete their work, and print the remaining output statements. Once all the work (including all coroutines) in the body of the `runBlocking()` call have been completed, then `runBlocking()` returns and the program ends.

Now you've changed your [[Synchronous Code]] into **asynchronous** code. When an asynchronous function returns, the task may not be finished yet. This is what you saw in the case of `launch()`. The function returned, but its work was not completed yet. By using `launch()`, multiple tasks can run concurrently in your code, which is a powerful capability to use in the [[Android]] apps you develop.


## async()

In the real world, you won't know how long the network requests for forecast and temperature will take. If you want to display a unified weather report when both tasks are done, then the current approach with `launch()` isn't sufficient. That's where `async()` comes in.

Use the `async()` function from the coroutines library if you care about when the coroutine finishes and need a return value from it.

The `async()` function returns an [[Kotlin Object]] of type `Deferred`, which is like a promise that the result will be in there when it's ready. You can access the result on the `Deferred` [[Kotlin Object|Object]] using `await()`.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        val forecast: Deferred<String> = async {
            getForecast()
        }
        val temperature: Deferred<String> = async {
            getTemperature()
        }
        println("${forecast.await()} ${temperature.await()}")
        println("Have a good day!")
    }
}

suspend fun getForecast(): String {
    delay(1000)
    return "Sunny"
}

suspend fun getTemperature(): String {
    delay(1000)
    return "30\u00b0C"
}

-->
Weather forecast
Sunny 30°C
Have a good day!

```

**Note:** As a real-world example of `async(),` you can check out this part of the [Now in Android app](https://github.com/android/nowinandroid). In the [SyncWorker](https://github.com/android/nowinandroid/blob/main/sync/work/src/main/java/com/google/samples/apps/nowinandroid/sync/workers/SyncWorker.kt#L65) class, the call to `sync()` returns a boolean if the sync to a particular backend was successful. If any of the sync operations failed, then the app needs to perform a retry.

## Parallel Decomposition

We can take this weather example a step further and see how coroutines can be useful in parallel decomposition of work. Parallel decomposition involves taking a problem and breaking it into smaller subtasks that can be solved in parallel. When the results of the subtasks are ready, you can combine them into a final result.

In your code, extract out the logic of the weather report from the body of `runBlocking()` into a single `getWeatherReport()` function that returns the combined string of `Sunny 30°C`.

```
...

suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async { getTemperature() }
    "${forecast.await()} ${temperature.await()}"
}

...

```

`coroutineScope{}` creates a local scope for this weather report task. The coroutines launched within this scope are grouped together within this scope, which has implications for cancellation and exceptions

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        println(getWeatherReport())
        println("Have a good day!")
    }
}

suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async { getTemperature() }
    "${forecast.await()} ${temperature.await()}"
}

suspend fun getForecast(): String {
    delay(1000)
    return "Sunny"
}

suspend fun getTemperature(): String {
    delay(1000)
    return "30\u00b0C"
}

```

The output is the same, but there are some noteworthy takeaways here. As mentioned earlier, `coroutineScope()` will only return once all its work, including any coroutines it launched, have completed. In this case, both coroutines `getForecast()` and `getTemperature()` need to finish and return their respective results. Then the `Sunny` text and `30°C` are combined and returned from the scope. This weather report of `Sunny 30°C` gets printed to the output, and the caller can proceed to the last print statement of `Have a good day!`.

With `coroutineScope()`, even though the function is internally doing work concurrently, it appears to the caller as a synchronous operation because `coroutineScope` won't return until all work is done.

The key insight here for structured concurrency is that you can take multiple concurrent operations and put it into a single synchronous operation, where concurrency is an implementation detail. The only requirement on the calling code is to be in a suspend function or coroutine. Other than that, the structure of the calling code doesn't need to take into account the concurrency details.