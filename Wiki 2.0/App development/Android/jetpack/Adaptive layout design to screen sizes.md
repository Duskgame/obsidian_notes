#jetpack_compose 

https://developer.android.com/codelabs/basic-android-kotlin-compose-adaptive-navigation-for-large-screens?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-4-pathway-3%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-adaptive-navigation-for-large-screens#0

- [[#What are breakpoints?|What are breakpoints?]]
- [[#Use Window Size Classes|Use Window Size Classes]]
- [[#Implement adaptive UI navigation|Implement adaptive UI navigation]]
- [[#**Learn more**|**Learn more**]]

## What are breakpoints?

You may wonder how you can show different layouts for the same app. The short answer is by using conditionals on different states, the way you did in the beginning of this codelab.

To create an adaptive app, you need the layout to change based on screen size. The measurement point where a layout changes is known as a breakpoint. Material Design created an [opinionated breakpoint range](https://m3.material.io/foundations/adaptive-design/large-screens/overview) that covers most Android screens.

![[image-8.png|884x304]]

**Note**: The breakpoints concept in adaptive layouts is different from the [breakpoints term in debugging](https://developer.android.com/studio/debug#breakPoints).

## Use Window Size Classes

The `WindowSizeClass` API introduced for Compose makes the implementation of Material Design breakpoints simpler.

Window Size Classes introduces three categories of sizes: Compact, Medium, and Expanded, for both width and height.

![[image-9.png|549x283]]
![[image-10.png|549x310]]

Complete the following steps to implement the `WindowSizeClass` API in the Reply app:

1. Add the `material3-window-size-class` dependency to the module `build.gradle.kts` file.

**build.gradle.kts**

```
...dependencies {...    implementation("androidx.compose.material3:material3-window-size-class")...
```

2. Click **Sync Now** to sync gradle after adding the dependency.

With the `build.gradle.kts` file up to date, you now can create a variable that stores the size of the app's window at any given time.

4. In the `onCreate()` function in the `MainActivity.kt` file, assign the `calculateWindowSizeClass()` method with `this` context passed in the parameter to a variable named `windowSize`.
5. Import the appropriate `calculateWindowSizeClass` package.


## Implement adaptive UI navigation

Currently, the [bottom navigation](https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary#NavigationBar\(androidx.compose.ui.Modifier,androidx.compose.ui.graphics.Color,androidx.compose.ui.graphics.Color,androidx.compose.ui.unit.Dp,kotlin.Function1\)) is used for all screen sizes.

![[image-11.png|357x89]]

As previously discussed, this navigation element is not ideal because users can find it difficult to reach these essential navigation elements on larger screens. Fortunately, there are recommended patterns for different navigation elements for various window size classes in [navigation for responsive UIs](https://developer.android.com/guide/topics/large-screens/navigation-for-responsive-uis#responsive_ui_navigation). For the Reply app, you can implement the following elements:

![[image-12.png|485x217]]

[Navigation rail](https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary#NavigationRail\(androidx.compose.ui.Modifier,androidx.compose.ui.graphics.Color,androidx.compose.ui.graphics.Color,kotlin.Function1,androidx.compose.foundation.layout.WindowInsets,kotlin.Function1\)) is another navigation component by [material design](https://m3.material.io/components/navigation-rail/overview) which allows compact navigation options for primary destinations to be accessible from the side of the app.

Similarly, a [persistent/permanent navigation drawer](https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary#PermanentNavigationDrawer\(kotlin.Function0,androidx.compose.ui.Modifier,kotlin.Function0\)) is created by [material design](https://m3.material.io/components/navigation-drawer/overview) as another option to provide ergonomic access for larger screens.


## **Learn more**

- [Build adaptive layouts](https://developer.android.com/jetpack/compose/layouts/adaptive)
- [Support different screen sizes](https://developer.android.com/guide/topics/large-screens/support-different-screen-sizes)
- [Design for large screens](https://m3.material.io/foundations/adaptive-design/large-screens/layout-anatomy)
- [Jetnews for every screen](https://medium.com/androiddevelopers/jetnews-for-every-screen-4d8e7927752)

## **Learn more**

- [Build adaptive layouts](https://developer.android.com/jetpack/compose/layouts/adaptive)
- [Support different screen sizes](https://developer.android.com/guide/topics/large-screens/support-different-screen-sizes)
- [Design for large screens](https://m3.material.io/foundations/adaptive-design/large-screens/layout-anatomy)
- [Jetnews for every screen](https://medium.com/androiddevelopers/jetnews-for-every-screen-4d8e7927752)
- [Multipreview annotations](https://developer.android.com/jetpack/compose/tooling#preview-multipreview)