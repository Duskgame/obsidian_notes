[[Jetpack Compose]] 

user interface component that covers the bottom part of the screen—that shows sharing options.

This piece of UI is provided by the Android operating system. 
System UI, such as the sharing screen, isn't called by your [[navController]]. Instead, you use something called an _Intent_.

An intent is a request for the system to perform some action, commonly presenting a new activity. 
There are many different intents, and you're encouraged to refer to the documentation for a comprehensive list. 
However, we are interested in the one called `ACTION_SEND`. You can supply this intent with some data, such as a string, and present appropriate sharing actions for that data.

The basic process for setting up an intent is as follows:

1. Create an intent object and specify the intent, such as `ACTION_SEND`.
2. Specify the type of additional data being sent with the intent. For a simple piece of text, you can use `"text/plain"`, though other types, such as `"image/*"` or `"video/*"`, are available.
3. Pass any additional data to the intent, such as the text or image to share, by calling the `putExtra()` method. This intent will take two extras: `EXTRA_SUBJECT` and `EXTRA_TEXT`.
4. Call the `startActivity()` method of context, passing in an activity created from the intent.


```
val intent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"
    putExtra(Intent.EXTRA_SUBJECT, subject)
    putExtra(Intent.EXTRA_TEXT, summary)
}

```