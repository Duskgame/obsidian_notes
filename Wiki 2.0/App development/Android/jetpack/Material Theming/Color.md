https://developer.android.com/codelabs/basic-android-kotlin-compose-material-theming?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-[[Unit]]-3-pathway-3%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-material-theming#3

A color scheme is the combination of colors that your app uses. Different color combinations evoke different moods, which influences how people feel when they use your app.

Color, in the [[Android]] system, is represented by a hexadecimal (hex) color value. A hex color code starts with a pound (#) character, and is followed by six letters and/or numbers that represent the red, green, and blue (RGB) components of that color. The first two letters/numbers refer to red, the next two refer to green, and the last two refer to blue.

```
	     blue
	  green|
	red |  |
	 |  |  |
#|FF|80|00|80
   |
  alpha 
```

A color can also include an alpha value—letters and/or numbers—which represents the transparency of the color (#00 is 0% opacity (fully transparent), `#FF` is 100% opacity (fully opaque)). When included, the alpha value is the first two characters of the hex color code after the pound (#) character. If an alpha value is not included, it is assumed to be `#FF`, which is 100% opacity (fully opaque).


Use [Material Theme Builder](https://m3.material.io/theme-builder#/custom). to create a color scheme

- The **primary** colors are used for key components across the [[User Interface|UI]].
- The **secondary** colors are used for less prominent components in the UI.
- The **tertiary** colors are used for contrasting accents that can be used to balance primary and secondary colors or bring heightened attention to an element, such as an input field.
- The **on** color elements appear **on top** of other colors in the palette, and are primarily applied to text, iconography, and strokes. In our color palette, we have an **onSurface** color, which appears on top of the **[[Surface]]** color, and an **onPrimary** color, which appears on top of the **primary** color.

Having these slots leads to a cohesive design system, where related components are colored similarly.

## Add color palette to theme

On the Material Theme Builder page, there is the option to click the **Export** button to download a **Color.kt** file and **Theme.kt** file with the custom theme you created in the Theme Builder.

This will work to add the custom theme we create to your app. However, because the generated **Theme.kt** file does not include the code for dynamic color

**Note**: If you do decide to use the files generated from the Material Theme Builder for a different project, you will need to update the package name to the package name of your project.


*Open the **Color.kt** file and replace the contents

*Open the **Theme.kt** file and replace the contents to add the new colors to the theme.*

```
package com.example.woof.ui.theme

import android.app.Activity
import android.os.Build
import android.view.View
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.darkColorScheme
import androidx.compose.material3.dynamicDarkColorScheme
import androidx.compose.material3.dynamicLightColorScheme
import androidx.compose.material3.lightColorScheme
import androidx.compose.runtime.Composable
import androidx.compose.runtime.SideEffect
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.toArgb
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.platform.LocalView
import androidx.core.view.WindowCompat

private val LightColors = lightColorScheme(
    primary = md_theme_light_primary,
    onPrimary = md_theme_light_onPrimary,
    primaryContainer = md_theme_light_primaryContainer,
    onPrimaryContainer = md_theme_light_onPrimaryContainer,
    secondary = md_theme_light_secondary,
    onSecondary = md_theme_light_onSecondary,
    secondaryContainer = md_theme_light_secondaryContainer,
    onSecondaryContainer = md_theme_light_onSecondaryContainer,
    tertiary = md_theme_light_tertiary,
    onTertiary = md_theme_light_onTertiary,
    tertiaryContainer = md_theme_light_tertiaryContainer,
    onTertiaryContainer = md_theme_light_onTertiaryContainer,
    error = md_theme_light_error,
    errorContainer = md_theme_light_errorContainer,
    onError = md_theme_light_onError,
    onErrorContainer = md_theme_light_onErrorContainer,
    background = md_theme_light_background,
    onBackground = md_theme_light_onBackground,
    surface = md_theme_light_surface,
    onSurface = md_theme_light_onSurface,
    surfaceVariant = md_theme_light_surfaceVariant,
    onSurfaceVariant = md_theme_light_onSurfaceVariant,
    outline = md_theme_light_outline,
    inverseOnSurface = md_theme_light_inverseOnSurface,
    inverseSurface = md_theme_light_inverseSurface,
    inversePrimary = md_theme_light_inversePrimary,
    surfaceTint = md_theme_light_surfaceTint,
    outlineVariant = md_theme_light_outlineVariant,
    scrim = md_theme_light_scrim,
)


private val DarkColors = darkColorScheme(
    primary = md_theme_dark_primary,
    onPrimary = md_theme_dark_onPrimary,
    primaryContainer = md_theme_dark_primaryContainer,
    onPrimaryContainer = md_theme_dark_onPrimaryContainer,
    secondary = md_theme_dark_secondary,
    onSecondary = md_theme_dark_onSecondary,
    secondaryContainer = md_theme_dark_secondaryContainer,
    onSecondaryContainer = md_theme_dark_onSecondaryContainer,
    tertiary = md_theme_dark_tertiary,
    onTertiary = md_theme_dark_onTertiary,
    tertiaryContainer = md_theme_dark_tertiaryContainer,
    onTertiaryContainer = md_theme_dark_onTertiaryContainer,
    error = md_theme_dark_error,
    errorContainer = md_theme_dark_errorContainer,
    onError = md_theme_dark_onError,
    onErrorContainer = md_theme_dark_onErrorContainer,
    background = md_theme_dark_background,
    onBackground = md_theme_dark_onBackground,
    surface = md_theme_dark_surface,
    onSurface = md_theme_dark_onSurface,
    surfaceVariant = md_theme_dark_surfaceVariant,
    onSurfaceVariant = md_theme_dark_onSurfaceVariant,
    outline = md_theme_dark_outline,
    inverseOnSurface = md_theme_dark_inverseOnSurface,
    inverseSurface = md_theme_dark_inverseSurface,
    inversePrimary = md_theme_dark_inversePrimary,
    surfaceTint = md_theme_dark_surfaceTint,
    outlineVariant = md_theme_dark_outlineVariant,
    scrim = md_theme_dark_scrim,
)

@Composable
fun WoofTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    // Dynamic color is available on Android 12+
    dynamicColor: Boolean = false,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }

        darkTheme -> DarkColors
        else -> LightColors
    }
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            setUpEdgeToEdge(view, darkTheme)
        }
    }

    MaterialTheme(
        colorScheme = colorScheme,
        shapes = Shapes,
        typography = Typography,
        content = content
    )
}

/**
 * Sets up edge-to-edge for the window of this [view]. The system icon colors are set to either
 * light or dark depending on whether the [darkTheme] is enabled or not.
 */
private fun setUpEdgeToEdge(view: View, darkTheme: Boolean) {
    val window = (view.context as Activity).window
    WindowCompat.setDecorFitsSystemWindows(window, false)
    window.statusBarColor = Color.Transparent.toArgb()
    val navigationBarColor = when {
        Build.VERSION.SDK_INT >= 29 -> Color.Transparent.toArgb()
        Build.VERSION.SDK_INT >= 26 -> Color(0xFF, 0xFF, 0xFF, 0x63).toArgb()
        // Min sdk version for this app is 24, this block is for SDK versions 24 and 25
        else -> Color(0x00, 0x00, 0x00, 0x50).toArgb()
    }
    window.navigationBarColor = navigationBarColor
    val controller = WindowCompat.getInsetsController(window, view)
    controller.isAppearanceLightStatusBars = !darkTheme
    controller.isAppearanceLightNavigationBars = !darkTheme
}

```

In `WoofTheme()` the `colorScheme val` uses a `when` statement

- If `dynamicColor` is true and the build version is S or higher, it checks if the device is in `darkTheme` or not.
- If it is in [[Dark Theme]], `colorScheme` will be set to `dynamicDarkColorScheme`.
- If it is not in dark theme, it will be set to `dynamicLightColorScheme`.
- If the app is not using `dynamicColorScheme`, it checks if your app is in `darkTheme`. If so then `colorScheme` will be set to `DarkColors`.
- If neither of those are true then `colorScheme` will be set to `LightColors`.

The copied in **Theme.kt** file has `dynamicColor` set to false and the devices we are working with are in light mode so the `colorScheme` will be set to `LightColors`.


## Color mapping

Material components are automatically mapped to color slots. Other key components across the UI like Floating Action Buttons also default to the Primary color. This means that you don't need to explicitly assign a color to a component; it is automatically mapped to a color slot when you set the color theme in your app. You can override this by explicitly setting a color in the code. Read more about color roles [here](https://m3.material.io/styles/color/the-color-system/color-roles).

