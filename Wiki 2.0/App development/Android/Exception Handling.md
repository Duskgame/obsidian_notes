
[Exceptions: try, catch, finally, throw, Nothing](https://kotlinlang.org/docs/reference/exceptions.html)

[_Exceptions_](https://developer.android.com/reference/java/lang/Exception) are errors that can occur during runtime, not compile time, and they terminate the app abruptly without notifying the user. This can result in a poor user experience. _Exception handling_ is a mechanism by which you prevent the app from terminating abruptly and handle the situation in a user-friendly way.

The reason for exceptions could be as simple as division by zero or an error with the network connection. These exceptions are similar to the `IllegalArgumentException` 

Examples of potential issues while connecting to a server include the following:

- The URL or URI used in the [[API]] is incorrect.
- The server is unavailable, and the app could not connect to it.
- A network latency issue.
- Poor or no internet connection on the device.

These exceptions can't be handled during compile time, but you can use a `try-catch` block to handle the exception in runtime. For further learning, refer to [Exceptions](https://kotlinlang.org/docs/reference/exceptions.html).

**Example syntax for try-catch block**

```
try {
    // some code that can cause an exception.
}
catch (e: SomeException) {
    // handle the exception to avoid abrupt termination.
}

```

