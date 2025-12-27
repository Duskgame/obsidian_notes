https://developer.android.com/codelabs/basic-android-kotlin-compose-build-a-dice-roller-app?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-2-pathway-2%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-build-a-dice-roller-app#8



```
package com.example.diceroller
	import android.os.Bundle
	import androidx.activity.ComponentActivity
	import androidx.activity.compose.setContent
	import androidx.activity.enableEdgeToEdge
	import androidx.compose.foundation.Image
	import androidx.compose.foundation.layout.Column
	import androidx.compose.foundation.layout.Spacer
	import androidx.compose.foundation.layout.fillMaxSize
	import androidx.compose.foundation.layout.height
	import androidx.compose.foundation.layout.padding
	import androidx.compose.foundation.layout.wrapContentSize
	import androidx.compose.material3.Button
	import androidx.compose.material3.Scaffold
	import androidx.compose.material3.Surface
	import androidx.compose.material3.Text
	import androidx.compose.runtime.Composable
	import androidx.compose.runtime.getValue
	import androidx.compose.runtime.mutableStateOf
	import androidx.compose.runtime.remember
	import androidx.compose.runtime.setValue
	import androidx.compose.ui.Alignment
	import androidx.compose.ui.Modifier
	import androidx.compose.ui.res.painterResource
	import androidx.compose.ui.res.stringResource
	import androidx.compose.ui.tooling.preview.Preview
	import androidx.compose.ui.unit.dp
	import com.example.diceroller.ui.theme.DiceRollerTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            DiceRollerTheme {
                DiceRollerApp()
            }
        }
    }
}

@Preview
@Composable
fun DiceRollerApp(){
    DiceWithButtonAndImage(modifier = Modifier
        .fillMaxSize()
        .wrapContentSize(Alignment.Center))
}

@Composable
fun DiceWithButtonAndImage(modifier: Modifier = Modifier) {

    var result: UInt by remember { mutableStateOf(1u) }

    val imageResource = when (result) {
        1u -> R.drawable.dice_1
        2u -> R.drawable.dice_2
        3u -> R.drawable.dice_3
        4u -> R.drawable.dice_4
        5u -> R.drawable.dice_5
        else -> R.drawable.dice_6
    }

    Column(
        modifier = modifier,
        //This ensures that the children within the column
        // are centered on the device screen with respect to the width.
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Image(
            painter = painterResource(imageResource),
            contentDescription = result.toString()
        )
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = { result = (1u..6u).random() }) {
            Text(stringResource(R.string.roll))
        }
    }
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    DiceRollerTheme {
        DiceRollerApp()
    }
}
```


## Learn more

- [Jetpack Compose Tutorial](https://developer.android.com/jetpack/compose/tutorial)
- [Add Jetpack Compose toolkit dependencies](https://developer.android.com/jetpack/compose/setup#compose-compiler)
- [Get started with Jetpack Compose](https://developer.android.com/jetpack/compose/documentation)
- [Basics of Composable functions](https://developer.android.com/jetpack/compose/layouts/basics#composable-functions)
- [`Button` composable](https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary#Button\(kotlin.Function0,androidx.compose.ui.Modifier,kotlin.Boolean,androidx.compose.ui.graphics.Shape,androidx.compose.material3.ButtonColors,androidx.compose.material3.ButtonElevation,androidx.compose.foundation.BorderStroke,androidx.compose.foundation.layout.PaddingValues,androidx.compose.foundation.interaction.MutableInteractionSource,kotlin.Function1\))
- [`Image` composable](https://developer.android.com/reference/kotlin/androidx/compose/foundation/package-summary#Image\(androidx.compose.ui.graphics.ImageBitmap,kotlin.String,androidx.compose.ui.Modifier,androidx.compose.ui.Alignment,androidx.compose.ui.layout.ContentScale,kotlin.Float,androidx.compose.ui.graphics.ColorFilter,androidx.compose.ui.graphics.FilterQuality\))
- [State and Jetpack Compose](https://developer.android.com/jetpack/compose/state)


