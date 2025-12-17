Applying a shape can change so much about the look and feel of a composable. Shapes direct attention, identify components, communicate state, and express brand.

Many shapes are defined using [`RoundedCornerShape`](https://developer.android.com/reference/kotlin/androidx/compose/foundation/shape/RoundedCornerShape), which describes a rectangle with rounded corners. The number passed in defines how round the corners are. If `RoundedCornerShape(0.dp)` is used, the rectangle has no rounded corners; if `RoundedCornerShape(50.dp)` is used, the corners will be fully circular.

You can also customize shapes further by adding different rounding percentages on each corner. It's pretty fun to play around with shapes!

|                                                                                       |                                                                                        |                                                                                      |
| ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Top left: 50.dp  <br>Bottom left: 25.dp  <br>Top right: 0.dp  <br>Bottom right: 15.dp | Top left: 15.dp  <br>Bottom left: 50.dp  <br>Top right: 50.dp  <br>Bottom right: 15.dp | Top left: 0.dp  <br>Bottom left: 50.dp  <br>Top right: 0.dp  <br>Bottom right: 50.dp |
|                                                                                       |                                                                                        |                                                                                      |
The **Shape.kt** file is used to define shapes of components in Compose. There are three types of components: small, medium, and large. In this section, you will modify the `Card` component, which is defined as `medium` size. Components are grouped into [shape categories](https://m3.material.io/styles/shape/shape-scale-tokens#b09934f1-1b0f-4ce4-ade6-4a1f138add6c) based on their size.

```
val Shapes = Shapes(
   small = RoundedCornerShape(50.dp),
)
```
