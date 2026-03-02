## [What is WorkManager?](https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-7-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-workmanager#3)

WorkManager is part of [Android Jetpack](http://d.android.com/jetpack) and an [Architecture Component](http://d.android.com/arch) for background work that needs a combination of opportunistic and guaranteed execution. 

Opportunistic execution means that WorkManager does your background work as soon as it can. 

Guaranteed execution means that WorkManager takes care of the logic to start your work under a variety of situations, even if you navigate away from your app.


WorkManager is an incredibly flexible library that has many additional benefits. Some of these benefits include:

- Support for both asynchronous one-off and periodic tasks.
- Support for constraints, such as network conditions, storage space, and charging status.
- Chaining of complex work requests, such as running work in parallel.
- Output from one work request used as input for the next.
- Handling API-level compatibility back to API level 14 (see note).
- Working with or without Google Play services.
- Following system health best practices.
- Support to easily display state of work requests in the app's UI.

**Note:** WorkManager sits on top of a few APIs, such as [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler.html) and [`AlarmManager`](https://developer.android.com/reference/android/app/AlarmManager). WorkManager picks the right APIs to use based on conditions like the user's device API level. To learn more, check out [Schedule tasks with WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager/) and the [WorkManager documentation](https://developer.android.com/reference/androidx/work/WorkManager).


## [When to use WorkManager](https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-7-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-workmanager#4)

The WorkManager library is a good choice for tasks that you need to complete. The running of these tasks is not dependent on the app continuing to run after the work is enqueued. The tasks run even if the app is closed or the user returns to the home screen.

Some examples of tasks that are a good use of WorkManager:

- Periodically querying for latest news stories.
- Applying filters to an image and then saving the image.
- Periodically syncing local data with the network.

WorkManager is one option for running a task off of the main thread but it is not a catch-all for running every type of task off of the main thread. [Coroutines](https://developer.android.com/kotlin/coroutines) are another option that previous codelabs discuss.

For more details about when to use WorkManager, check out the [Guide to background work](https://d.android.com/guide/background/).


## [Add WorkManager to your app](https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-7-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-workmanager#5)

`WorkManager` requires the following gradle dependency. **This is already included** in the build file:

**app/build.gradle.kts**
```
dependencies {
    // WorkManager dependency
    implementation("androidx.work:work-runtime-ktx:2.8.1")
}
```

You must use the most current [stable release](https://developer.android.com/jetpack/androidx/releases/work) version of `work-runtime-ktx` in your app.

If you change the version, make sure to click **Sync Now** to sync your project with the updated gradle files.


## [WorkManager Basics](https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-7-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-workmanager#6)

There are a few WorkManager classes you need to know about:

- [**`Worker`**](https://developer.android.com/reference/androidx/work/Worker) / [**`CoroutineWorker`**](https://developer.android.com/reference/androidx/work/CoroutineWorker): Worker is a class that performs work synchronously on a background thread. As we are interested in asynchronous work, we can use CoroutineWorker, which has interoperability with Kotlin Coroutines. In this app, you extend from the CoroutineWorker class and override the [`doWork()`](https://developer.android.com/reference/androidx/work/CoroutineWorker#doWork\(\)) method. This method is where you put the code for the actual work you want to perform in the background.
- [**`WorkRequest`**](https://developer.android.com/reference/androidx/work/WorkRequest.html): This class represents a request to do some work. A `WorkRequest` is where you define if the worker needs to be run once or periodically. [Constraints](https://developer.android.com/reference/androidx/work/Constraints.html) can also be placed on the `WorkRequest` that require certain conditions are met before the work runs. One example is that the device is charging before starting the requested work. You pass in your `CoroutineWorker` as part of creating your `WorkRequest`.
- [**`WorkManager`**](https://developer.android.com/reference/androidx/work/WorkManager.html): This class actually schedules your `WorkRequest` and makes it run. It schedules a `WorkRequest` in a way that spreads out the load on system resources, while honoring the constraints you specify.

In your case, you define a new `BlurWorker` class, which contains the code to blur an image. When you click the **Start** button, WorkManager creates and then enqueues a `WorkRequest` object.