#jetpack_compose 

The [[User Interface|UI]] is what the user sees, and the UI [[State in Compose|State]] is what the app says they should see. The UI is the visual representation of the UI state. Any changes to the UI state immediately are reflected in the UI.

![[image-2.png|630x157]]

In [[Jetpack Compose|Compose]], the only way to update the UI is by changing the state of the app. What you can control is your UI state. Every time the state of the UI changes, Compose recreates the parts of the UI tree that changed. Composables can accept state and expose events. For example, a `TextField`/`OutlinedTextField` accepts a value and exposes a callback `onValueChange` that requests the callback handler to change the value.

```
var name by remember { mutableStateOf("") }
OutlinedTextField(
    value = name,
    onValueChange = { name = it },
    label = { Text("Name") }
)
```

Because composables accept state and expose events, the unidirectional data flow pattern fits well with [[jetpack]] Compose. 