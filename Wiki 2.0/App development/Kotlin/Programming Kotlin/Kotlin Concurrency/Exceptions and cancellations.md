## Introduction to exceptions

An [exception](https://kotlinlang.org/docs/exceptions.html) is an unexpected event that happens during execution of your code. You should implement appropriate ways of handling these exceptions, to prevent your app from crashing and impacting the user experience negatively.

## Exceptions with coroutines

you'll need to know that there is a parent-child relationship among [[Coroutines]]. You can launch a coroutine (known as the child) from another coroutine (parent). As you launch more coroutines from those coroutines, you can build up a whole hierarchy of coroutines.

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
    delay(500)
    throw AssertionError("Temperature is invalid")
    return "30\u00b0C"
}

```

The coroutine executing `getTemperature()` and the coroutine executing `getForecast()` are child coroutines of the same parent coroutine. The behavior you're seeing with exceptions in coroutines is due to structured concurrency. When one of the child coroutines fails with an exception, it gets propagated upwards. The parent coroutine is cancelled, which in turn cancels any other child coroutines (e.g. the coroutine running `getForecast()` in this case). Lastly, the error gets propagated upwards and the program crashes with the `AssertionError`.


## Try-catch exceptions

If you know that certain parts of your code can possibly throw an exception, then you can surround that code with a [try-catch](https://kotlinlang.org/docs/exceptions.html) block. You can catch the exception and handle it more gracefully in your app, such as by showing the user a helpful error message. Here's a code snippet of how it might look:

```
try {
    // Some code that may throw an exception
} catch (e: IllegalArgumentException) {
    // Handle exception
}

```

This approach also works for [[Asynchronous Code]] with coroutines. You can still use a try-catch expression to catch and handle exceptions in coroutines. The reason is because with structured concurrency, the sequential code is still [[Synchronous Code]] so the try-catch block will still work in the same expected way.

```
...

fun main() {
    runBlocking {
        ...
        try {
            ...
            throw IllegalArgumentException("No city selected")
            ...
        } catch (e: IllegalArgumentException) {
            println("Caught exception $e")
            // Handle error
        }
    }
}

...

```

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        try {
            println(getWeatherReport())
        } catch (e: AssertionError) {
            println("Caught exception in runBlocking(): $e")
            println("Report unavailable at this time")
        }
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
    delay(500)
    throw AssertionError("Temperature is invalid")
    return "30\u00b0C"
}



--->
Weather forecast
Caught exception in runBlocking(): java.lang.AssertionError: Temperature is invalid
Report unavailable at this time
Have a good day!
```

From the output, you can observe that `getTemperature()` throws an exception. In the body of the `runBlocking()` function, you surround the `println(getWeatherReport())` call in a try-catch block. You catch the type of exception that was expected (`AssertionError` in the case of this example). Then you print the exception to the output as `"Caught exception"` followed by the error message string. To handle the error, you let the user know that the weather report is not available with an additional `println()` statement: `Report unavailable at this time`.

Note that this behavior means that if there's a failure with getting the temperature, then there will be no weather report at all (even if a valid forecast was retrieved).

Depending on how you want your program to behave, there's an alternative way that you could have handled the exception in the weather program.

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
    val temperature = async {
        try {
            getTemperature()
        } catch (e: AssertionError) {
            println("Caught exception $e")
            "{ No temperature found }"
        }
    }

    "${forecast.await()} ${temperature.await()}"
}

suspend fun getForecast(): String {
    delay(1000)
    return "Sunny"
}

suspend fun getTemperature(): String {
    delay(500)
    throw AssertionError("Temperature is invalid")
    return "30\u00b0C"
}


--->
Weather forecast
Caught exception java.lang.AssertionError: Temperature is invalid
Sunny { No temperature found }
Have a good day!

```

From the output, you can see that calling `getTemperature()` failed with an exception, but the code within `async()` was able to catch that exception and handle it gracefully by having the coroutine still return a `String` that says the temperature was not found. The weather report is still able to be printed, with a successful forecast of `Sunny`. The temperature is missing in the weather report, but in its place, there is a message explaining that the temperature was not found. This is a better user experience than the program crashing with the error.

A helpful way to think about this error handling approach is that ==`async()` is the producer when a coroutine is started with it. `await()` is the consumer because it's waiting to consume the result from the coroutine. The producer does the work and produces a result. The consumer consumes the result. If there's an exception in the producer, then the consumer will get that exception if it's not handled, and the coroutine will fail. However, if the producer is able to catch and handle the exception, then the consumer won't see that exception and will see a valid result.==

```
suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async {
        try {
            getTemperature()
        } catch (e: AssertionError) {
            println("Caught exception $e")
            "{ No temperature found }"
        }
    }

    "${forecast.await()} ${temperature.await()}"
}

```

In this case, the producer (`async()`) was able to catch and handle the exception and still return a `String` result of `"{ No temperature found }"`. The consumer (`await()`) receives this `String` result and doesn't even need to know that an exception happened. This is another option to gracefully handle an exception that you expect could happen in your code.

## **Note:**
Exceptions are propagated differently for coroutines started with `launch()` versus `async()`. Within a coroutine started by `launch()`, an exception is thrown immediately so you can surround code with a try-catch block if it's expected to throw an exception. See [example](https://developer.android.com/kotlin/coroutines#handling-exceptions).


## **Warning:** 
Within a try-catch statement in your coroutine code, avoid catching a general `Exception` because that includes a very broad range of exceptions. You could be inadvertently catching and suppressing an error that is actually a bug that should be fixed in your code. Another important reason is that cancellation of coroutines, which is discussed later in this section, depends on [`CancellationException`](https://kotlinlang.org/docs/exception-handling.html#cancellation-and-exceptions). So if you catch any type of `Exception` including `CancellationExceptions` without rethrowing them, then the cancellation behavior within your coroutines may behave differently than expected. Instead, catch a specific type of exception that you expect will be thrown from your code.

Now you've learned that exceptions propagate upwards in the tree of coroutines, unless they are handled. It's also important to be careful when the exception propagates all the way to the root of the hierarchy, which could crash your whole app. Learn more details about exception handling in the [Exceptions in coroutines](https://medium.com/androiddevelopers/exceptions-in-coroutines-ce8da1ec060c) blogpost and [Coroutine exceptions handling](https://kotlinlang.org/docs/exception-handling.html) article.


## **Cancellation**

A similar topic to exceptions is cancellation of coroutines. This scenario is typically user-driven when an event has caused the app to cancel work that it had previously started.

For example, say that the user has selected a preference in the app that they no longer want to see temperature values in the app. They only want to know the weather forecast (e.g. `Sunny`), but not the exact temperature. Hence, cancel the coroutine that is currently getting the temperature data.

```
...

suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async { getTemperature() }
    
    delay(200)
    temperature.cancel()

    "${forecast.await()}"
}

...


--->
Weather forecast
Sunny
Have a good day!
```

What you've learned here is that a coroutine can be cancelled, but it won't affect other coroutines in the same scope and the parent coroutine will not be cancelled.

**Note**: You can learn more about [Cancellation of Coroutines](https://medium.com/androiddevelopers/cancellation-in-coroutines-aa6b90163629) in this [[Android]] Developers blogpost. Cancellation must be cooperative, so you should implement your coroutine so that it can be cancelled.