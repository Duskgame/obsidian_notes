---
aliases:
  - Sealed Classes/Interfaces
---
#jetpack_compose 

- [[#1. Example setup: fixed start + per‑object destination|1. Example setup: fixed start + per‑object destination]]

In a [[jetpack]] Compose [[Navigation]] setup, sealed classes or interfaces are usually used to model all possible destinations as a closed, type‑safe set of route definitions. This lets you create destination routes based on instances instead of raw strings, and makes navigation safer and more maintainable.

​
Basic sealed [[Kotlin Class|Class]] for destinations

You define a sealed [[Kotlin Class]] (or sealed [[Interface]]) where each implementation represents one destination and contains its route (and often a helper to build that route):

```
sealed class Destination(val route: String) {
    object Home : Destination("home")
    
    data class Details(val id: Int) : Destination("details/{id}") {
        fun createRoute(id: Int) = "details/$id"
    }

    object Settings : Destination("settings")
}
```

Destination is sealed, so all destinations are known at compile time, which makes when expressions over destinations exhaustive.

​

Objects work well for destinations without parameters; data classes or objects with helpers work for destinations that need [[Arguments]].



Using instances to navigate

Instead of writing string routes all over the app, you create or pick a Destination [[Kotlin Object|Instance]] and derive the actual route from it:

```
fun navigateTo(navController: NavController, destination: Destination) {
    when (destination) {
        Destination.Home -> navController.navigate(Destination.Home.route)
        is Destination.Details -> navController.navigate(
            Destination.Details.createRoute(destination.id)
        )
        Destination.Settings -> navController.navigate(Destination.Settings.route)
    }
}
```

Then from your [[User Interface|UI]]:

```
navigateTo(navController, Destination.Details(id = 42))
```

The call site is type‑safe and self‑documenting; you pass an instance rather than a string.

If you add or change a destination, the compiler guides you to update all relevant when branches.


Integrating with [[NavHost]]

When you set up NavHost, you still use the route property from your sealed destinations:

```
NavHost(navController, startDestination = Destination.Home.route) {
    composable(Destination.Home.route) {
        HomeScreen(
            onOpenDetails = { id ->
                navigateTo(navController, Destination.Details(id))
            }
        )
    }

    composable("details/{id}") { backStackEntry ->
        val id = backStackEntry.arguments?.getString("id")?.toInt() ?: 0
        DetailsScreen(id = id)
    }

    composable(Destination.Settings.route) {
        SettingsScreen()
    }
}
```

If you have a sealed interface instead of a sealed class, the idea is the same; the benefit is that different hierarchies (e.g., top‑level tabs vs. nested destinations) can implement the same sealed interface to be handled uniformly.

​
When you say “depending on the instances I have”

If you have a domain model and want dynamic destinations based on it, you can:

Wrap your domain [[Kotlin Object|Object]]’s identity in a sealed destination:

```
data class ProfileDestination(val userId: String) : Destination("profile/{userId}") {
    fun createRoute() = "profile/$userId"
}
```


Use that in navigation:

```
navigateTo(navController, ProfileDestination(user.id))
```

You are still using a finite, sealed set of destination types, but you can create many instances (e.g., many different ProfileDestination for different users) and generate the route string from those instances in a type‑safe way.
​



A sealed class (or sealed [[Interface]]) works well as a **type‑safe route model**, and you can then create destinations dynamically for each instance of a [[Data Class]] (e.g., each item in a list) by encoding its identity into the route.

## 1. Example setup: fixed start + per‑object destination

Imagine a data class:
```
@kotlinx.serialization.Serializable
data class Product(
    val id: String,
    val name: String
)
```

Define a sealed class for destinations:
```
sealed class Dest(val route: String) {
    data object Home : Dest("home")

    data class ProductDetails(val productId: String) : Dest("product/{productId}") {
        fun route() = "product/$productId"
    }
}
```

- `Home` is the **fixed start location**. 
- `ProductDetails` is the **destination type**, and each instance (`ProductDetails("123")`, `ProductDetails("456")`, …) corresponds to one product screen.

In your `NavHost`:
```
NavHost(
    navController = navController,
    startDestination = Dest.Home.route
) {
    composable(Dest.Home.route) {
        HomeScreen(
            products = products,
            onProductClick = { product ->
                navController.navigate(Dest.ProductDetails(product.id).route())
            }
        )
    }

    composable("product/{productId}") { backStackEntry ->
        val productId = backStackEntry.arguments?.getString("productId") ?: return@composable
        val product = products.first { it.id == productId }
        ProductDetailsScreen(product)
    }
}
```

- From `HomeScreen` you map **each Product instance** to a `Dest.ProductDetails(product.id)` and call `route()` to get the navigation string.
- The destination type is sealed, so your navigation graph is still based on a **closed set** of screen types, even though you can have many instances.