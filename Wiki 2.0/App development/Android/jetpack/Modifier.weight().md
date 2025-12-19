#jetpack_compose
**Note:** [`Modifier.weight()`](https://developer.android.com/reference/kotlin/androidx/compose/foundation/layout/RowScope#\(androidx.compose.ui.Modifier\).weight\(kotlin.Float,kotlin.Boolean\)) sets the UI element's width/height proportionally to the element's weight, relative to its weighted siblings (other child elements in the row or column).

Example: Consider three child elements in a row with weights `1f`, `1f`, and `2f`. All child elements have assigned weights in this case. The available space for the row is divided proportionally to the specified weight value, with more available space going to children with higher weight values. The child elements will distribute the weight as shown below:
```
|weight= 1f|weight= 1f|     weight= 2f     |
```

In the above row, the first child composable has ¼ of the row's width, the second also has ¼ of the row's width, and the third has ½ of the row's width.

If the children don't have assigned weights, (weight is an optional parameter), then the child composable's height/width would default to wrap content (wrapping the contents of what's inside the UI element).

**Note on Float values:** Float values in Kotlin are decimal numbers, represented with an `f` or `F` at the end of the number.