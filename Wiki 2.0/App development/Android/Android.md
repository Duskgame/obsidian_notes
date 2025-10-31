The `onCreate()` function is the entry point to this Android app and calls other functions to build the user interface. In Kotlin programs, the `main()` function is the entry point/starting point of execution. In Android apps, the `onCreate()` function fills that role.

```
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
	Text(
		text = "Hello $name!",
		modifier = modifier
	)
}
```

Next, look at the `Greeting()` function. The `Greeting()` function is a Composable function, notice the `@Composable` annotation above it. This Composable function takes some input and generates what's shown on the screen.
- 1. You add the `@Composable` annotation before the function.
- 2. `@Composable` function names are capitalized.
- 3. `@Composable` functions can't return anything.

The `GreetingPreview()` function is a cool feature that lets you see what your composable looks like without having to build your entire app. To enable a preview of a composable, annotate with `@Composable` and `@Preview`. The `@Preview` annotation tells Android Studio that this composable should be shown in the design view of this file.

```
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
	GreetingCardTheme {
		Greeting("Meghan")
	}
}
```


In your code, the best practice is to keep your imports listed alphabetically and remove unused imports. To do this press **Help** on the top toolbar, type in **optimize imports**, and click on **Optimize Imports**.

A [`Modifier`](https://developer.android.com/reference/kotlin/androidx/compose/ui/Modifier) is used to augment or decorate a composable. One modifier you can use is the `padding` modifier, which adds space around the element (in this case, adding space around the text). This is accomplished by using the [`Modifier.padding()`](https://developer.android.com/reference/kotlin/androidx/compose/ui/Modifier#\(androidx.compose.ui.Modifier\).padding\(androidx.compose.ui.unit.Dp\)) function.

Every composable should have an optional parameter of the type `Modifier`. This should be the first optional parameter.

```
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
	Surface(color = Color.Cyan) {
		Text(
			text = "Hi, my name is $name!",
			modifier = modifier.padding(24.dp)
		)
	}
}
```



```
package com.example.greetingcardimport android.os.Bundleimport androidx.activity.ComponentActivityimport androidx.activity.compose.setContentimport androidx.compose.foundation.layout.fillMaxSizeimport androidx.compose.foundation.layout.paddingimport androidx.compose.material3.MaterialThemeimport androidx.compose.material3.Surfaceimport androidx.compose.material3.Textimport androidx.compose.runtime.Composableimport androidx.compose.ui.Modifierimport androidx.compose.ui.graphics.Colorimport androidx.compose.ui.tooling.preview.Previewimport androidx.compose.ui.unit.dpimport com.example.greetingcard.ui.theme.GreetingCardTheme


class MainActivity : ComponentActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContent {
			GreetingCardTheme {
				// A surface container using the 'background' color                 from the theme
				Surface(
					modifier = Modifier.fillMaxSize(),
					color = MaterialTheme.colorScheme.background
				) {
					Greeting("Android")
				}
			}
		}
	}
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
	Surface(color = Color.Cyan) {
		Text(
			text = "Hi, my name is $name!",
			modifier = modifier.padding(24.dp)
		)
	}
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
	GreetingCardTheme {
		Greeting("Meghan")
	}
}
```

## Summary

- To create a new project: open Android Studio, click **New Project > Empty Activity > Next**, enter a name for your project and then configure its settings.
- To see how your app looks, use the **Preview** pane.
- Composable functions are like regular functions with a few differences: functions names are capitalized, you add the `@Composable` annotation before the function, `@Composable` functions can't return anything.
- A [`Modifier`](https://developer.android.com/reference/kotlin/androidx/compose/ui/Modifier) is used to augment or decorate your composable.

## Learn more

- [Meet Android Studio](https://developer.android.com/studio/intro)
- [Projects overview](https://developer.android.com/studio/projects)
- [Create a project](https://developer.android.com/studio/projects/create-project)
- [Add code from a template](https://developer.android.com/studio/projects/templates)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Padding – Material Design 3](https://m3.material.io/foundations/layout/understanding-layout/spacing#64eb2223-f5e8-4d2a-9edc-9e3a7002220a)