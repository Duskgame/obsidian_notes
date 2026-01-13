[[Coroutines]] use dispatchers to determine the thread to use for its execution. A **thread** can be started, does some work (executes some code), and then terminates when there's no more work to be done.

When a user starts your app, the [[Android]] system creates a new process and a single thread of execution for your app, which is known as the **main thread**. The main thread handles many important operations for your app including Android system events, drawing the [[User Interface|UI]] on the screen, handling user input events, and more. As a result, most of the code you write for your app will likely run on the main thread.

There are two terms to understand when it comes to the threading behavior of your code: **blocking** and **non-blocking**. A regular [[Function]] blocks the calling thread until its work is completed. That means it does not yield the calling thread until the work is done, so no other work can be done in the meantime. Conversely, non-blocking code yields the calling thread until a certain condition is met, so you can do other work in the meantime. You can use an asynchronous function to perform non-blocking work because it returns before its work is completed.

In the case of Android apps, ==you should only call blocking code on the main thread if it will execute fairly quickly==. The goal is to keep the main thread unblocked, so that it can execute work immediately if a new event is triggered. This main thread is the **UI thread** for your activities and is responsible for UI drawing and UI related events. When there's a change on the screen, the UI needs to be redrawn. For something like an animation on the screen, the [[User Interface]] needs to be redrawn frequently so that it appears like a smooth transition. ==If the main thread needs to execute a long-running block of work, then the screen won't update as frequently== and the user will see an abrupt transition (known as "jank") or the app may hang or be slow to respond.

Hence we need to move any long-running work items off the main thread and handle it in a different thread. Your app starts off with a single main thread, but you can choose to create multiple threads to perform additional work. These additional threads can be referred to as worker threads. It's perfectly fine for a long-running task to block a worker thread for a long time, because in the meantime, the main thread is unblocked and can actively respond to the user.


There are some built-in dispatchers that [[Kotlin]] provides:

- **Dispatchers.Main:** Use this dispatcher to run a coroutine on the main Android thread. This dispatcher is used primarily for handling UI updates and interactions, and performing quick work.
- **Dispatchers.IO:** This dispatcher is optimized to perform disk or network I/O outside of the main thread. For example, read from or write to files, and execute any network operations.
- **Dispatchers.Default:** This is a default dispatcher used when calling `launch()` and `async()`, when no dispatcher is specified in their context. You can use this dispatcher to perform computationally-intensive work outside of the main thread. For example, processing a bitmap image file.

**Note:** There's also `Executor.asCoroutineDispatcher()` and `Handler.asCoroutineDispatcher()` extensions, if you need to make a `CoroutineDispatcher` from a `Handler` or `Executor` that you already have available.

```
...

fun main() {
    runBlocking {
        launch {
            withContext(Dispatchers.Default) {
                delay(1000)
                println("10 results found.")
            }
        }
        println("Loading...")
    }
}

```

wrap the contents of the launched coroutine with a call to `withContext()` to change the `CoroutineContext` that the coroutine is executed within, and specifically override the dispatcher. Switch to using the `Dispatchers.Default` (instead of `Dispatchers.Main` which is currently being used for the rest of the coroutine code in the program).

Switching dispatchers is possible because [`withContext()`](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/with-context.html) is itself a suspending function. It executes the provided block of code using a new `CoroutineContext`. The new context comes from the context of the parent job (the outer `launch()` block), except it overrides the dispatcher used in the parent context with the one specified here: `Dispatchers.Default`. This is how we are able to go from executing work with `Dispatchers.Main` to using `Dispatchers.Default`.

```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("${Thread.currentThread().name} - runBlocking function")
                launch {
            println("${Thread.currentThread().name} - launch function")
            withContext(Dispatchers.Default) {
                println("${Thread.currentThread().name} - withContext function")
                delay(1000)
                println("10 results found.")
            }
            println("${Thread.currentThread().name} - end of launch function")
        }
        println("Loading...")
    }
}


--->
main @coroutine#1 - runBlocking function
Loading...
main @coroutine#2 - launch function
DefaultDispatcher-worker-1 @coroutine#2 - withContext function
10 results found.
main @coroutine#2 - end of launch function
```

From this output, you can observe that most of the code is executed in coroutines on the main thread. However, for the portion of your code in the `withContext(Dispatchers.Default)` block, that is executed in a coroutine on a Default Dispatcher worker thread (which is not the main thread). Notice that after `withContext()` returns, the coroutine returns to running on the main thread (as evidenced by output statement: `main @coroutine#2 - end of launch function`). This example demonstrates that you can switch the dispatcher by modifying the context that is used for the coroutine.

If you have coroutines that were started on the main thread, and you want to move certain operations off the main thread, then you can use `withContext` to switch the dispatcher being used for that work. Choose appropriately from the available dispatchers: `Main`, `Default`, and `IO` depending on the type of operation it is. Then that work can be assigned to a thread (or group of threads called a thread pool) designated for that purpose. Coroutines can suspend themselves, and the dispatcher also influences how they resume.

Note that when working with popular libraries like Room and Retrofit (in this [[Unit]] and the next one), you may not have to explicitly switch the dispatcher yourself if the library code already handles doing this work using an alternative coroutine dispatcher like `Dispatchers.IO.` In those cases, the `suspend` functions that those libraries reveal may already be **main-safe** and can be called from a coroutine running on the main thread. The library itself will handle switching the dispatcher to one that uses worker threads.