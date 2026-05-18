The [`Log`](https://developer.android.com/reference/kotlin/android/util/Log) class writes messages to the **Logcat**. The **Logcat** is the console for logging messages. Messages from Android about your app appear here, including the messages you explicitly send to the log with the `Log.d()` [[Kotlin Class Method|Method]] or other `Log` [[Kotlin Class|Class]] methods.

There are three important aspects of the `Log` instruction:

- The **_priority_** of the log message, that is, how important the message is. In this case, the [`Log.v()`](https://developer.android.com/reference/kotlin/android/util/Log#v_1) logs verbose messages. [`Log.d()`](https://developer.android.com/reference/kotlin/android/util/Log#d) method writes a debug message. Other methods in the `Log` class include [`Log.i()`](https://developer.android.com/reference/kotlin/android/util/Log#i_1) for informational messages, [`Log.w()`](https://developer.android.com/reference/kotlin/android/util/Log#w\(kotlin.String,%20kotlin.String\)) for warnings, and [`Log.e()`](https://developer.android.com/reference/kotlin/android/util/Log#e\(kotlin.String,%20kotlin.String\)) for error messages.
- The log `tag` (the first [[Parameter]]), in this case `"MainActivity"`. The tag is a string that lets you more easily find your log messages in the Logcat. The tag is typically the name of the class.
- The actual log message, called `msg` (the second parameter), is a short string, which in this case is `"onCreate Called"`.

The Logcat can contain many messages, most of which aren't useful to you. You can filter the Logcat entries in many ways, but searching is the easiest. Because you used `MainActivity` as the log tag in your code, you can use that tag to filter the log. Your log message includes the date and time, your log tag, the name of the package (`com.example.dessertclicker`), and the actual message. Because this message appears in the log, you know that `onCreate()`was executed.

- The Android logging API, and specifically the [`Log`](https://developer.android.com/reference/android/util/Log) class, enables you to write short messages that are displayed in the Logcat within Android Studio.
- Use `Log.d()` to write a debug message. This method takes two arguments: the log _tag_, typically the name of the class, and the log _message_, a short string.
- Use the **Logcat** window in Android Studio to view the system logs, including the messages you write.


- [`Log`](https://developer.android.com/reference/android/util/Log) class
- [View logs with Logcat](https://developer.android.com/studio/debug/am-logcat)