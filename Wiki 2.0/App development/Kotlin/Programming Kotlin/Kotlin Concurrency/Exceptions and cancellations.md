## Introduction to exceptions

An [exception](https://kotlinlang.org/docs/exceptions.html) is an unexpected event that happens during execution of your code. You should implement appropriate ways of handling these exceptions, to prevent your app from crashing and impacting the user experience negatively.

## Exceptions with coroutines

you'll need to know that there is a parent-child relationship among coroutines. You can launch a coroutine (known as the child) from another coroutine (parent). As you launch more coroutines from those coroutines, you can build up a whole hierarchy of coroutines.

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

