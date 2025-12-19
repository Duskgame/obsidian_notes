#jetpack_compose
Icons are symbols that can help users understand a user interface by visually communicating the intended function. They often take inspiration from objects in the physical world that a user is expected to have experienced. Icon design often reduces the level of detail to the minimum required to be familiar to a user. For example, a pencil in the physical world is used for writing, so its icon counterpart usually indicates **create** or **edit**.

Material Design provides a [number of icons](https://material.io/resources/icons/?style=baseline), arranged in common categories, for most of your needs.

## Add Gradle dependency

Add the `material-icons-extended` library dependency to your project.

1. In the **Project** pane, open **Gradle Scripts > build.gradle.kts (Module :app)**.
2. Scroll to the end of the `build.gradle.kts (Module :app)` file. In the `dependencies{}` block, add the following line:

```
implementation("androidx.compose.material:material-icons-extended")
```

**Tip:** Whenever you modify the Gradle files, Android Studio may have to import or update libraries and run some background tasks. Android Studio displays a pop-up that asks you to sync your project. Click **Sync Now**.

## Add the icon composable

Add a function to display the **Expand More** icon from the Material icons library and use it as a button.

1. In `MainActivity.kt`, after the `DogItem()` function, create a new composable function called `DogItemButton()`.
2. Pass in a `Boolean` for the expanded state, a lambda expression for the button onClick handler, and an optional `Modifier` 
3. Inside the `DogItemButton()` function, add an [`IconButton()`](https://developer.android.com/reference/kotlin/androidx/compose/material/package-summary#IconButton\(kotlin.Function0,androidx.compose.ui.Modifier,kotlin.Boolean,androidx.compose.foundation.interaction.MutableInteractionSource,kotlin.Function0\)) composable that accepts an `onClick` named parameter, a lambda using trailing lambda syntax, that is invoked when this icon is pressed and an optional `modifier`. Set the `IconButton's onClick` and `modifier value parameters` equal to the ones passed in to `DogItemButton`.
4. Inside the `IconButton()` lambda block, add in an [`Icon`](https://developer.android.com/reference/kotlin/androidx/compose/material/icons/Icons) composable and set the `imageVector value-parameter` to `Icons.Filled.ExpandMore`. This is what will display at the end of the list item. Android Studio shows you a warning for the `Icon()` composable parameters that you will fix in the next step.
5. Add the value parameter `tint`, and set the color of the icon to `MaterialTheme.colorScheme.secondary`. Add the named parameter `contentDescription`, and set it to the string resource `R.string.expand_button_content_description`.

```
import androidx.compose.material.icons.filled.ExpandMore
import androidx.compose.material.icons.Icons
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton

@Composable
private fun DogItemButton(
   expanded: Boolean,
   onClick: () -> Unit,
   modifier: Modifier = Modifier
){
	IconButton(
	   onClick = onClick,
	   modifier = modifier
	){
	   Icon(
	       imageVector = Icons.Filled.ExpandMore,
	       contentDescription = stringResource(R.string.expand_button_content_description),
	       tint = MaterialTheme.colorScheme.secondary
	   )
	}
}
```

## Display the icon

Display the `DogItemButton()` composable by adding it to the layout.

1. At the beginning of `DogItem()`, add a `var` to save the expanded state of the list item. Set the initial value to `false`.

```
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue

var expanded by remember { mutableStateOf(false) }
```

Display the icon button within the list item. In the `DogItem()` composable, at the end of the `Row` block, after the call to `DogInformation()`, add `DogItemButton()`. Pass in the `expanded` state and an empty lambda for the callback. You will define the `onClick` action in a later step.

```
Row(
   modifier = Modifier
       .fillMaxWidth()
       .padding(dimensionResource(R.dimen.padding_small))
) {
   DogIcon(dog.imageResourceId)
   DogInformation(dog.name, dog.age)
   DogItemButton(
       expanded = expanded,
       onClick = { /*TODO*/ }
   )
}

```