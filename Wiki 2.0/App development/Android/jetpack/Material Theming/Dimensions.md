#jetpack_compose
## Dimens file

Just like you use the **strings.xml** to store the strings in your app, it is also a good practice to use a file called **dimens.xml** to store dimension values. This is helpful so you don't hard code values and so if you need to, you can change them in a single place.

Go to **app** > **res** > **values** > **dimens.xml** and take a look at the file. It stores dimension values for `padding_small`, `padding_medium`, and `image_size`. These dimensions will be used throughout the app.

```
<resources>
   <dimen name="padding_small">8dp</dimen>
   <dimen name="padding_medium">16dp</dimen>
   <dimen name="image_size">64dp</dimen>
</resources>

```

For example, to add padding_small, you would pass in dimensionResource(id = R.dimen.padding_small).

n WoofApp(), add a modifier with padding_small in the call to DogItem().

```
@Composable
fun WoofApp() {
    Scaffold { it ->
        LazyColumn(contentPadding = it) {
            items(dogs) {
                DogItem(
                    dog = it,
                    modifier = Modifier.padding(dimensionResource(R.dimen.padding_small))
                )
            }
        }
    }
}
```