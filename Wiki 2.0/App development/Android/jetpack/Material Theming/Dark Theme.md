[[Jetpack Compose]]
In the Android system, there is the option to switch your device to a dark theme. A dark theme uses darker, more subdued colors, and:

- Can reduce power usage by a significant amount (depending on the device's screen technology).
- Improves visibility for users with low vision and those who are sensitive to bright light.
- Makes it easier for anyone to use a device in a low-light environment.

Your app can opt-in to [Force Dark](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme#force-dark), which means the system will implement a dark theme for you. However, it is a better experience for your users if you implement the dark theme, so that you maintain full control over the app theme.

When choosing your own dark theme, it is important to note that colors for a dark theme need to meet [accessibility contrast standards](https://webaim.org/resources/contrastchecker/). Dark themes use a dark surface color, with limited color accents.

```
@Preview
@Composable
fun WoofDarkThemePreview() {
   WoofTheme(darkTheme = true) {
       WoofApp()
   }
}

```