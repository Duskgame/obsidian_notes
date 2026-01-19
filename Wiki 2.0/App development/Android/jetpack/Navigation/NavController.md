[[Jetpack Compose]]

## Pop to the start screen

Unlike the system back button, the **Cancel** button doesn't go back to the previous screen. Instead, it should pop—remove—all screens from the back stack and return to the starting screen.

You can do this by calling the `popBackStack()` method.

![[image-7.png]]

The `popBackStack()` method has two required parameters.

- **`route`****:** The string representing the route of the destination you want to navigate back to.
- **`inclusive`****:** A Boolean value that, if true, also pops (removes) the specified route. If false, `popBackStack()` will remove all destinations on top of—but not including—the start destination, leaving it as the topmost screen visible to the user.

When users press the **Cancel** button on any of the screens, the app resets the state in the view model and calls `popBackStack()`.