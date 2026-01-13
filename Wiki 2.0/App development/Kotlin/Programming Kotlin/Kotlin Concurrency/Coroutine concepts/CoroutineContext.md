The `CoroutineContext` provides information about the context in which the coroutine will be running in. The `CoroutineContext` is essentially a map that stores elements where each element has a unique key. These are not required fields, but here are some examples of what may be contained in a context:

- name - name of the coroutine to uniquely identify it
- [[Job]] - controls the lifecycle of the coroutine
- [[Dispatcher]] - dispatches the work to the appropriate thread
- exception handler - handles exceptions thrown by the code executed in the coroutine


**Note:** These are default values for the `CoroutineContext`, which will be used if you don't provide values for them:

- "coroutine" for the coroutine name
- no parent job
- `Dispatchers.Default` for the coroutine dispatcher
- no exception handler


Each of the elements in a context can be appended together with the `+` [[Keywords and operators|operator]]. For example, one `CoroutineContext` could be defined as follows:

```
Job() + Dispatchers.Main + exceptionHandler
```

Because a name is not provided, the default coroutine name is used.

Within a coroutine, if you launch a new coroutine, the child coroutine will inherit the `CoroutineContext` from the parent coroutine, but replace the job specifically for the coroutine that just got created. You can also override any elements that were inherited from the parent context by passing in [[Arguments]] to the `launch()` or `async()` functions for the parts of the context that you want to be different.

```
scope.launch(Dispatchers.Default) {
    ...
}

```

You can learn more about `CoroutineContext` and how the context gets inherited from the parent in this [KotlinConf conference video talk](https://youtu.be/w0kfnydnFWI?t=256).

You've seen the mention of dispatcher several times. Its role is to dispatch or assign the work to a thread. Let's discuss threads and dispatchers in more detail.