[[Jetpack Compose]]
Animations can add visual cues that notify users about what's going on in your app. They are especially useful when the [[User Interface|UI]] changes [[State in Compose|State]], such as when new content loads or new actions become available. Animations can also add a polished look to your app.

## Spring Animation

[Spring animation](https://developer.android.com/jetpack/compose/animation#spring) is a physics-based animation driven by a **spring force**. With a spring animation, the value and velocity of movement are calculated based on the spring force that is applied.

For example, if you drag an app icon around the screen and then release it by lifting your finger, the icon jumps back to its original location by an invisible force.

_Spring effect_

Spring force is guided by the following two [[Kotlin Class Properties|properties]]:

- **Damping ratio**: The bounciness of the spring.
- **Stiffness level**: The stiffness of the spring, that is, how fast the spring moves toward the end.

```
import androidx.compose.animation.core.Spring
import androidx.compose.animation.core.spring

Column(
   modifier = Modifier
       .animateContentSize(
           animationSpec = spring(
               dampingRatio = Spring.DampingRatioNoBouncy,
               stiffness = Spring.StiffnessMedium
           )
       )
)
```

## `animate*AsState`

The [`animate*AsState()`](https://developer.android.com/jetpack/compose/animation/value-based#animate-as-state) functions are one of the simplest animation APIs in Compose for animating a single value. You only provide the end value (or target value), and the [[API]] starts animation from the current value to the specified end value.

[[Jetpack Compose|Compose]] provides `animate*AsState()` functions for `Float`, `[[Color]]`, `Dp`, `Size`, `Offset`, and `Int`, to name a few. You can easily add support for other data types using `animateValueAsState()` that takes a generic type.

```
import androidx.compose.animation.animateColorAsState

@Composable
fun DogItem(
   dog: Dog,
   modifier: Modifier = Modifier
) {
   var expanded by remember { mutableStateOf(false) }
   val color by animateColorAsState(
       targetValue = if (expanded) MaterialTheme.colorScheme.tertiaryContainer
       else MaterialTheme.colorScheme.primaryContainer,
   )
   ...
}
```

```
@Composable
fun DogItem(
   dog: Dog, 
   modifier: Modifier = Modifier
) {
   ...
   Card(
       ...
   ) {
       Column(
           modifier = Modifier
               .animateContentSize(
                   ...
                   )
               )
               .background(color = color)
       ) {...}
}
```

## **Learn more**

- [Jetpack Compose Animation](https://developer.android.com/jetpack/compose/animation)
- Codelab: [Animating elements in Jetpack Compose](https://developer.android.com/codelabs/jetpack-compose-animation)
- Video: [Animation Reimagined](https://www.youtube.com/watch?v=Z_T1bVjhMLk)
- Video: [Jetpack Compose: Animation](https://youtu.be/7yY2OocGiQU)