[Android documentation](https://developer.android.com/develop/ui/compose/components/navigation-bar)

```
@Composable
fun NavigationBarExample(modifier: Modifier = Modifier) {
    val navController = rememberNavController()
    val startDestination = Destination.SONGS
    var selectedDestination by rememberSaveable { mutableIntStateOf(startDestination.ordinal) }

    Scaffold(
        modifier = modifier,
        bottomBar = {
            NavigationBar(windowInsets = NavigationBarDefaults.windowInsets) {
                Destination.entries.forEachIndexed { index, destination ->
                    NavigationBarItem(
                        selected = selectedDestination == index,
                        onClick = {
                            navController.navigate(route = destination.route)
                            selectedDestination = index
                        },
                        icon = {
                            Icon(
                                destination.icon,
                                contentDescription = destination.contentDescription
                            )
                        },
                        label = { Text(destination.label) }
                    )
                }
            }
        }
    ) { contentPadding ->
        AppNavHost(navController, startDestination, modifier = Modifier.padding(contentPadding))
    }
}

enum class Destination(
    val route: String,
    val icon: ImageVector,
    val contentDescription: String,
    val label: String
) {
    SONGS(
        route = "songs",
        icon = Icons.Default.LibraryMusic,
        contentDescription = "Songs",
        label = "Songs"
    ),
    ALBUMS(
        route = "albums",
        icon = Icons.Default.Album,
        contentDescription = "Alben",
        label = "Alben"
    ),
    ARTISTS(
        route = "artists",
        icon = Icons.Default.Person,
        contentDescription = "Künstler",
        label = "Künstler"
    )
}
```