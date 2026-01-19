[[Jetpack Compose]]
[[Coroutines]] help manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive. You also learned how to write [[Unit]] tests to test the coroutines.

The following features are some of the benefits of coroutines:

- **Readability:** The code you write with coroutines provides a clear understanding of the sequence that executes the lines of code.
- **[[jetpack]] integration:** Many Jetpack libraries, such as [[Jetpack Compose|Compose]] and ViewModel, include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for [[Structured concurrency]].
- **Structured concurrency:** Coroutines make concurrent code safe and easy to implement, eliminate unnecessary boilerplate code, and ensure that coroutines launched by the app are not lost or keep wasting resources.

## **Summary**

- Coroutines enable you to write long running code that runs concurrently without learning a new style of programming. The execution of a coroutine is sequential by design.
- The `suspend` [[Keywords and operators|keyword]] is used to mark a [[Function]], or function type, to indicate its availability to execute, pause, and resume a set of code instructions.
- A `suspend` function can be called only from another [[Suspend Function]].
- You can start a new coroutine using the `launch` or `async` builder function.
- Coroutine context, coroutine builders, [[Job]], coroutine scope and [[Dispatcher]] are the major components for implementing coroutines.
- Coroutines use dispatchers to determine the threads to use for its execution.
- Job plays an important role to ensure structured concurrency by managing the lifecycle of coroutines and maintaining the parent-child relationship.
- A `[[CoroutineContext]]` defines the behavior of a coroutine using Job and a coroutine dispatcher.
- A `[[CoroutineScope]]` controls the lifetime of coroutines through its Job and enforces cancellation and other rules to its children and their children recursively.
- Launch, completion, cancellation, and failure are four common operations in the coroutine's execution.
- Coroutines follow a principle of structured concurrency.

## Learn more

- [Kotlin coroutines on Android](https://developer.android.com/kotlin/coroutines)
- [Additional resources for Kotlin coroutines and flow](https://developer.android.com/kotlin/coroutines/additional-resources)
- [Exceptions in Coroutines](https://medium.com/androiddevelopers/exceptions-in-coroutines-ce8da1ec060c)
- [Coroutines on Android (part 1)](https://medium.com/androiddevelopers/coroutines-on-android-part-i-getting-the-background-3e0e54d20bb)
- [Kotlin coroutines 101](https://www.youtube.com/watch?v=ZTDXo0-SKuU&t=2s&sa=D&source=docs&ust=1664866751197807&usg=AOvVaw19xcRyp5y7Sdx1dzcf-YQP)