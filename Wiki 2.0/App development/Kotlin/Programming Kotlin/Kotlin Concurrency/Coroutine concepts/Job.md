When you launch a coroutine with the `launch()` function, it returns an [[Kotlin Object|Instance]] of `Job`. The Job holds a handle, or reference, to the coroutine, so you can manage its lifecycle.

```
val job = launch { ... }
```

The job can be used to control the life cycle, or how long the coroutine lives for, such as cancelling the coroutine if you don't need the task anymore.

```
job.cancel()
```

With a job, you can check if it's active, cancelled, or completed. The job is completed if the coroutine and any [[Coroutines]] that it launched have completed all of their work. Note that the coroutine could have completed due to a different reason, such as being cancelled, or failing with an exception, but the job is still considered completed at that point.

Jobs also keep track of the parent-child relationship among coroutines.

## Job hierarchy

When a coroutine launches another coroutine, the job that returns from the new coroutine is called the child of the original parent job.

```
val job = launch {
    ...            

    val childJob = launch { ... }

    ...
}

```

These parent-child relationships form a job hierarchy, where each job can launch jobs, and so on.

![[image-14.png]]

This parent-child relationship is important because it will dictate certain behavior for the child and parent, and other children belonging to the same parent. You saw this behavior in the earlier examples with the weather program.

- If a parent job gets cancelled, then its child jobs also get cancelled.
- When a child job is canceled using `job.cancel()`, it terminates, but it does not cancel its parent.
- If a job fails with an exception, it cancels its parent with that exception. This is known as propagating the error upwards (to the parent, the parent's parent, and so on). .