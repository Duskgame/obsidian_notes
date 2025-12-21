
#jetpack_compose
Applying a shape can change so much about the look and feel of a composable. Shapes direct attention, identify components, communicate [[State in Compose|State]], and express brand.

Many shapes are defined using [`RoundedCornerShape`](https://developer.android.com/reference/kotlin/androidx/compose/foundation/shape/RoundedCornerShape), which describes a rectangle with rounded corners. The number passed in defines how round the corners are. If `RoundedCornerShape(0.dp)` is used, the rectangle has no rounded corners; if `RoundedCornerShape(50.dp)` is used, the corners will be fully circular.

You can also customize shapes further by adding different rounding percentages on each corner. It's pretty fun to play around with shapes!

|                                                                                       |                                                                                        |                                                                                      |
| ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Top left: 50.dp  <br>Bottom left: 25.dp  <br>Top right: 0.dp  <br>Bottom right: 15.dp | Top left: 15.dp  <br>Bottom left: 50.dp  <br>Top right: 50.dp  <br>Bottom right: 15.dp | Top left: 0.dp  <br>Bottom left: 50.dp  <br>Top right: 0.dp  <br>Bottom right: 50.dp |
|                                                                                       |                                                                                        |                                                                                      |
The **Shape.kt** file is used to define shapes of components in [[Jetpack Compose|Compose]]. There are three types of components: small, medium, and large. In this section, you will modify the `Card` component, which is defined as `medium` size. Components are grouped into [shape categories](https://m3.material.io/styles/shape/shape-scale-tokens#b09934f1-1b0f-4ce4-ade6-4a1f138add6c) based on their size.

```
val Shapes = Shapes(
   small = RoundedCornerShape(50.dp),
)
```


In `DogIcon()`, add a [`clip`](https://developer.android.com/reference/kotlin/androidx/compose/ui/Modifier#\(androidx.compose.ui.Modifier\).clip\(androidx.compose.ui.graphics.Shape\)) attribute to the `modifier` of the `Image`; this will clip the image into a shape. Pass in the `MaterialTheme.shapes.small`.
To make all the photos circular, add in a [`ContentScale`](https://developer.android.com/reference/kotlin/androidx/glance/layout/ContentScale) and a [`Crop`](https://developer.android.com/reference/kotlin/androidx/glance/layout/ContentScale#Crop\(\)) attribute; this crops the image to fit. Note that `contentScale` is an attribute of `Image`, and not part of the `modifier`.

```
@Composable
fun DogIcon(
    @DrawableRes dogIcon: Int,
    modifier: Modifier = Modifier
) {
    Image(
        modifier = modifier
            .size(dimensionResource(R.dimen.image_size))
            .padding(dimensionResource(R.dimen.padding_small))
            .clip(MaterialTheme.shapes.small),
        contentScale = ContentScale.Crop,
        painter = painterResource(dogIcon),

        // Content Description is not needed here - image is decorative, and setting a null content
        // description allows [[accessibility]] services to skip this element during navigation.

        contentDescription = null
    )
}

```

## Add a shape to the list item

In this section, you will add a shape to the list item. The list item is already being displayed through a `Card`. A `Card` is a [[Surface]] that can contain a single composable and contains options for decoration. The decoration can be added through the border, shape, and more. In this section, you will use the `Card` to add a shape to the list item.

1. Open the **Shape.kt** file. A `Card` is a medium component, so you add the medium [[Parameter]] of the `Shapes` [[Kotlin Object|Object]]. For this app, the top right and bottom left corners of the list item, but not make them fully circular. To achieve that, pass in `16.dp` to the `medium` attribute.

```
package com.example.courses.ui.theme

import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Shapes
import androidx.compose.ui.unit.dp

val Shapes = Shapes(
    small = RoundedCornerShape(50.dp),
    medium = RoundedCornerShape(bottomStart = 50.dp, topStart = 50.dp, bottomEnd = 15.dp, topEnd = 15.dp)
)

```

Since a `Card`, by default, already uses the medium shape, you do not have to explicitly set it to the medium shape. Check out the **Preview** and to see the newly shaped `Card`!

If you go back to the Theme.kt file in WoofTheme(), and look at the MaterialTheme(), you'll see the shapes attribute is set to the Shapes val that you just updated.

```
MaterialTheme(
   colors = colors,
   typography = Typography,
   shapes = Shapes,
   content = content
)
```
