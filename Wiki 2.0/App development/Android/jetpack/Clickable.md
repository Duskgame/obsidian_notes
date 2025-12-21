#jetpack_compose 

composables can become clickable by adding a clickable modifier


```
Image(
    painter = painterResource(R.drawable.icon),
    contentDescription = null,
    modifier = Modifier
        .size(48.dp)
        .clickable(
            enabled = true,           // disable if false
            onClickLabel = "Open",    // accessibility label
            role = Role.Button,       // semantics role
            onClick = {
                // Handle click
            }
        )
)
```

```
            Row(
                modifier = modifier
                    .clickable(
                        interactionSource = null,
                        indication = null,
                        onClick = {
                            expanded = !expanded
                        }
                    )
```