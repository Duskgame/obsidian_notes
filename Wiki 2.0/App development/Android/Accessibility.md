https://developer.android.com/codelabs/basic-android-kotlin-compose-test-accessibility?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-3-pathway-3%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-test-accessibility#0

There are a number of UI design choices to consider when trying to create a more accessible app. In addition to attributes and behaviors that allow for effective usage of TalkBack and Switch Access, below are some UI optimizations you can make to improve the accessibility of your app.

## **Content description**

Users of accessibility services, such as screen readers (like TalkBack), rely on content descriptions to understand the meaning of elements in an interface.

In some cases, such as when information is conveyed graphically within an element, content descriptions can provide a text description of the meaning or action associated with the element.

If elements in a user interface don't provide content labels, it can be difficult for some users to understand the information presented to them, or to perform actions in the interface. In Compose, you can describe visual elements using the `contentDescription` attribute. For strictly decorative visual elements, it's okay to set the `contentDescription` to `null`. Read more about how to apply content descriptions in the [documentation](https://developer.android.com/jetpack/compose/accessibility#describe-visual).

## Touch target size

Any on-screen element that someone can interact with must be large enough for reliable interaction. The minimum touch target size for something clickable is 48dp high x 48dp wide. There are a number of Material Design components for which Compose automatically assigns the correct minimum target size. Keep in mind that the minimum touch target size refers to clickable components smaller than 48dp. Components larger than 48dp will have a touch target that is at least the size of the component. Follow these resources for more information on touch target size:

1. Read about minimum target size in the [Accessibility in Compose documentation](https://developer.android.com/jetpack/compose/accessibility#minimum-target-sizes).
2. Watch the touch target sizes section of the [What's new in Google Accessibility video](https://youtu.be/6LsaP6oKxMY?t=166).

Take a look at the code for the **Woof** app. In **MainActivity.kt**, the `DogItemButton` composable uses an `IconButton` composable.

```
@Composable
private fun DogItemButton(
   expanded: Boolean,
   onClick: () -> Unit,
   modifier: Modifier = Modifier
) {
   IconButton(onClick = onClick) {
       Icon(
           imageVector = if (expanded) Icons.Filled.ExpandLess else Icons.Filled.ExpandMore,
           tint = MaterialTheme.colors.secondary,
           contentDescription = stringResource(R.string.expand_button_content_description),
       )
   }
}

```

The `IconButton` is a Material Design component. The [documentation for the `IconButton`](https://developer.android.com/reference/kotlin/androidx/compose/material/package-summary#IconButton\(kotlin.Function0,androidx.compose.ui.Modifier,kotlin.Boolean,androidx.compose.foundation.interaction.MutableInteractionSource,kotlin.Function0\)) composable indicates that the minimum touch target size is 48dp x 48dp.

The code below is the source code for the `IconButton`. Notice that the modifier sets the `minimumTouchTargetSize()`.

```
@Composable
fun IconButton(
   onClick: () -> Unit,
   modifier: Modifier = Modifier,
   enabled: Boolean = true,
   interactionSource: MutableInteractionSource = remember { MutableInteractionSource() },
   content: @Composable () -> Unit
) {
   Box(
       modifier = modifier
           .minimumTouchTargetSize()
           .clickable(
               onClick = onClick,
               enabled = enabled,
               role = Role.Button,
               interactionSource = interactionSource,
               indication = rememberRipple(bounded = false, radius = RippleRadius)
           ),
       contentAlignment = Alignment.Center
   ) {
       val contentAlpha = if (enabled) LocalContentAlpha.current else ContentAlpha.disabled
       CompositionLocalProvider(LocalContentAlpha provides contentAlpha, content = content)
   }
}

```

**Note**: The code that you see above for the `IconButton` cannot be found directly in the **Woof** app code. This is the source code for the `IconButton` that comes from the Compose Material library. As an optional step, if you want to find this code for yourself, you can right-click on the call to `IconButton` in the `MainActivity`, and select **Go To > Declaration or Usages**.

## Color contrast

The colors you choose for your app interface affect how easily users can read and understand it. Sufficient color contrast makes text and images easier to read and comprehend.

Along with benefiting users with various visual impairments, sufficient color contrast helps all users when viewing an interface on devices in extreme lighting conditions, such as when exposed to direct sunlight or on a display with low brightness.

You can read more about how to optimize for color contrast in the [Android Accessibility Help documentation](https://support.google.com/accessibility/android/answer/7158390). In that link, you will find information on contrast ratios to help guide your decision on which colors to use. Additionally, you can use [this tool](https://webaim.org/resources/contrastchecker/) to test your background and foreground colors for sufficient color contrast ratio. Small text has a recommended ratio of 4.5 : 1 and large text has a recommended ratio of 3.0 : 1.

For the **Woof** app, our designer picked the colors for us, and ensured they had enough color contrast. When you create your own app, remember to check the color contrast. The [Color Tool](https://material.io/resources/color/#!/?view.left=0&view.right=0) for Material Design has an accessibility tab where you can see appropriate text colors on top of the primary and secondary colors.

## Learn more

- [Accessibility on Android](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc8OENfLdh3zM5T6IRdlVYKj)
- [Accessible design](https://m3.material.io/foundations/accessible-design/overview)
- [Accessibility in Jetpack Compose](https://developer.android.com/codelabs/jetpack-compose-accessibility#0)
- [Make your Android app more accessible](https://developer.android.com/courses/pathways/make-your-android-app-accessible)