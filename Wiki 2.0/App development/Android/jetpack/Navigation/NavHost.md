[[Jetpack Compose]]

A NavHost is a Composable that displays other composable destinations, based on a given route. For example, if the route is `Flavor`, then the `NavHost` would show the screen to choose the cupcake flavor. If the route is `Summary`, then the app displays the summary screen.

The syntax for `NavHost` is just like any other Composable.

![[image-5.png|301x338]]


There are two notable parameters.

- **`navController`:** An instance of the `NavHostController` class. You can use this object to navigate between screens, for example, by calling the `navigate()` method to navigate to another destination. You can obtain the `NavHostController` by calling `rememberNavController()` from a composable function.
- **`startDestination`:** A string route defining the destination shown by default when the app first displays the `NavHost`. In the case of the Cupcake app, this should be the `Start` route.

Like other composables, `NavHost` also takes a `modifier` parameter.

**Note:** [`NavHostController`](https://developer.android.com/reference/androidx/navigation/NavHostController) is a subclass of the [`NavController`](https://developer.android.com/reference/androidx/navigation/NavController) class that provides additional functionality for use with a `NavHost` composable.


## Handle routes in your `NavHost`

Like other composables, `NavHost` takes a function type for its content.

![[image-6.png|429x190]]

Within the content function of a `NavHost`, you call the `composable()` function. The `composable()` function has two required parameters.

- **`route`:** A string corresponding to the name of a route. This can be any unique string. You'll use the name property of the `CupcakeScreen` enum's constants.
- **`content`:** Here you can call a composable that you want to display for the given route.

You'll call the `composable()` function once for each of the four routes.

**Note:** The `composable()` function is an extension function of [`NavGraphBuilder`](https://developer.android.com/reference/kotlin/androidx/navigation/NavGraphBuilder).


```
NavHost(
            navController = navController,
            startDestination = CupcakeScreen.Start.name,
            modifier = Modifier.padding(innerPadding)
        ) {
            composable(route = CupcakeScreen.Start.name) {
                StartOrderScreen(
                    quantityOptions = DataSource.quantityOptions,
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(dimensionResource(R.dimen.padding_medium))
                )
            }
            composable(route = CupcakeScreen.Flavor.name) {
                val context = LocalContext.current
                SelectOptionScreen(
                    subtotal = uiState.price,
                    options = DataSource.flavors.map { id -> context.resources.getString(id) },
                    onSelectionChanged = { viewModel.setFlavor(it) },
                    modifier = Modifier.fillMaxHeight()
                )
            }
            composable(route = CupcakeScreen.Pickup.name) {
                SelectOptionScreen(
                    subtotal = uiState.price,
                    options = uiState.pickupOptions,
                    onSelectionChanged = { viewModel.setDate(it) },
                    modifier = Modifier.fillMaxHeight()
                )
            }
            composable(route = CupcakeScreen.Summary.name) {
                OrderSummaryScreen(
                    orderUiState = uiState,
                    modifier = Modifier.fillMaxHeight()
                )
            }
        }

    }
```


