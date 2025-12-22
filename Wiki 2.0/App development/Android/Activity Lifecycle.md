During its lifetime, an activity transitions through, and sometimes back to, various states. This transitioning of states is known as the activity lifecycle.

In [[Android]], an activity is the entry point for interacting with the user.

The activity lifecycle extends from the creation of the activity to its destruction, when the system reclaims that activity's resources. As a user navigates in and out of an activity, each activity transitions between different states in the activity lifecycle.

**Note:** An Android app can have multiple activities. However, it is recommended to have a single activity

![[Pasted image 20251222105640.png|319x362]]

Typically, the entry point of a program is the `main()` [[Kotlin Class Method|Method]]. Android activities, however, begin with the `onCreate()` method;

Often, you want to change some behavior, or run some code, when the activity lifecycle [[State in Compose|State]] changes. Therefore, the `Activity` [[Kotlin Class|Class]] itself, and any subclasses of `Activity` such as `ComponentActivity`, implement a set of lifecycle callback methods.

**Note:** The asterisk on the `onRestart()` method indicates that this method is not called every time the [[State in Compose]] transitions between **Created** and **Started**. It is only called if `onStop()` was called and the activity is subsequently restarted.


## `onCreate()`

The `onCreate()` method is where you should do any one-time initializations for your activity. For example, in `onCreate()`, you call `setContent()`, which specifies the activity's [[User Interface|UI]] layout.

The `onCreate()` lifecycle method is called once, just after the activity _initializes_—when the OS creates the new `Activity` [[Kotlin Object|Object]] in memory. After `onCreate()` executes, the activity is considered _created_.

**Note:** When you override the `onCreate()` method, you must call the superclass implementation to complete the creation of the Activity, so within it, you must immediately call `super.onCreate()`. The same is true for other lifecycle callback methods.


## `onStart()`/`onStop()`

The `onStart()` lifecycle method is called just after `onCreate()`. After `onStart()` runs, your activity is visible on the screen. Unlike `onCreate()`, which is called only once to initialize your activity, `onStart()`can be called by the system many times in the lifecycle of your activity.

Note that `onStart()`is paired with a corresponding `onStop()`lifecycle method. If the user starts your app and then returns to the device's home screen, the activity is stopped and is no longer visible on screen.

## `onPause()`

When `onPause()` is called, the app no longer has focus. After `onStop()`, the app is no longer visible on screen. Although the activity is stopped, the `Activity` object is still in memory in the background. The Android OS has not destroyed the activity. The user might return to the app, so Android keeps your activity resources around.


## `onRestart()`

**Note**: `onRestart()` is only called by the system if the activity has already been created and eventually enters the **Created** state when `onStop()` is called, but returns back to the **Started** state instead of entering the **Destroyed** state. The `onRestart()` method is a place to put code that you only want to call if your activity is **not** being started for the first time.


## `onResume()`

When `onResume()` is called, the app gains the user focus–that is, the user can interact with the app. The part of the lifecycle in which the app is fully onscreen and has user focus is called the [foreground lifetime](https://developer.android.com/reference/android/app/Activity#activity-lifecycle).

When the app goes into the background, the focus is lost after `onPause()`, and the app is no longer visible after `onStop()`.





- The _activity lifecycle_ is a set of states through which an activity transitions. The activity lifecycle begins when the Android OS first creates the activity and ends when the OS destroys the activity.
- As the user navigates between activities, and inside and outside of your app, each activity moves between states in the activity lifecycle.
- Each state in the activity lifecycle has a corresponding callback method you can override in your `Activity` class. The core set of lifecycle methods are: [`onCreate()`](https://developer.android.com/reference/android/app/Activity.html#onCreate\(android.os.Bundle\)), [`onRestart()`](https://developer.android.com/reference/android/app/Activity.html#onRestart\(\)), [`onStart()`](https://developer.android.com/reference/android/app/Activity.html#onStart\(\)), [`onResume()`](https://developer.android.com/reference/android/app/Activity.html#onResume\(\)), [`onPause()`](https://developer.android.com/reference/android/app/Activity.html#onPause\(\)), [`onStop()`](https://developer.android.com/reference/android/app/Activity.html#onStop\(\)), [`onDestroy()`](https://developer.android.com/reference/android/app/Activity.html#onDestroy\(\)).
- To add behavior that occurs when your activity transitions into a lifecycle state, override the state's callback method.
- To add skeleton override methods to your classes in Android Studio, select **Code > Override Methods...** or press `Control+O`.