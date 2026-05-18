[[Jetpack Compose]]

## Understanding Surface

Surface is a Material Design primitive that represents a sheet of material with configurable appearance [[Kotlin Class Properties|properties]]. It automatically handles color theming, elevation shadows, and content color selection based on Material Design guidelines. Every major [[User Interface|UI]] component in Material Design is built upon Surface concepts.

```
@Composable  
@NonRestartableComposable  
fun Surface(  
    modifier: Modifier = Modifier,  
    [[shape]]: Shape = RectangleShape,  
    color: Color = MaterialTheme.colorScheme.surface,  
    contentColor: Color = contentColorFor(color),  
    tonalElevation: Dp = 0.dp,  
    shadowElevation: Dp = 0.dp,  
    border: BorderStroke? = null,  
    content: @Composable () -> Unit  
)
```
## Breaking Down the Parameters

**modifier**: Standard Modifier for styling, sizing, and behavior modifications.

**shape**: Defines the shape of the Surface. Can be RectangleShape, CircleShape, RoundedCornerShape, or custom shapes.

**color**: Background color of the Surface. Defaults to `MaterialTheme.colorScheme.surface` for automatic theming.

**contentColor**: Color that will be used for content inside the Surface. Automatically calculated using `contentColorFor(color)` to ensure proper contrast.

**tonalElevation**: Elevation that affects the surface color by blending with the primary color. Higher values create lighter tints.

**shadowElevation**: Elevation that creates actual shadows. Higher values create more pronounced shadows.

**border**: Optional BorderStroke for adding borders around the Surface.

**content**: The composable content to be placed inside the Surface.