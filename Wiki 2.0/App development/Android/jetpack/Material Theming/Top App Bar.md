[[Jetpack Compose]]

A `TopAppBar` can be used for many purposes, like use it for branding and to give your app personality. There are four different types of `TopAppBar`: center, small, medium and large. In this example you will implement a center top app bar. You will create a composable that looks like the screenshot below, and slot it into the `topBar` section of a `Scaffold`.

Add image and text to the top bar

In MainActivity.kt, create a composable called WoofTopAppBar() with an optional modifier.

```
@Composable
fun WoofTopAppBar(modifier: Modifier = Modifier) {
  
}
```
Scaffold supports the contentWindowInsets parameter which can help to specify insets for the scaffold content. WindowInsets are the parts of your screen where your app can intersect with the system UI, these ones are to be passed to the content slot via the PaddingValues parameters. Read more here.

The contentWindowInsets value is passed to the LazyColumn as the contentPadding.

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

Within the Scaffold, add a topBar attribute and set it to WoofTopAppBar().

```
Scaffold(
   topBar = {
       WoofTopAppBar()
   }
)
```

Below is how the WoofApp() composable will look:

```
@Composable
fun WoofApp() {
    Scaffold(
        topBar = {
            WoofTopAppBar()
        }
    ) { it ->
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


```
@Composable
fun WoofTopAppBar(modifier: Modifier = Modifier) {
   CenterAlignedTopAppBar(
       title = {
           Row(
               verticalAlignment = Alignment.CenterVertically
           ) {
               Image(
                   modifier = Modifier
                       .size(dimensionResource(id = R.dimen.image_size))
                       .padding(dimensionResource(id = R.dimen.padding_small)),
                   painter = painterResource(R.drawable.ic_woof_logo),

                   contentDescription = null
               )
               Text(
                   text = stringResource(R.string.app_name),
                   style = MaterialTheme.typography.displayLarge
               )
           }
       },
       modifier = modifier
   )
}

```