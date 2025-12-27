---
aliases:
  - " "
  - UDF
---
Unidirectional Data Flow (UDF), also called **one-way data flow**, is an architectural pattern where **state flows down** from parent to child components and **events flow up** via callbacks.

​

## The flow pattern

`State (read-only) ↓↓↓ UI renders → User Action → Event (callback) ↑↑↑ State Update`

- **Parent owns state** (`var count by remember { mutableStateOf(0) }`)
    
- **State passed down** as parameters (`Child(count = count)`)
    
- **Child is stateless**, just displays state and emits events
    
- **Child calls callback** (`onIncrement { count++ }`)
    
- **Parent updates state**, triggering recomposition
    

## Your TipTime example as UDF

**Before (bidirectional - bad):**

```
@Composable
fun TipTimeLayout() {
    var amount by remember { mutableStateOf("") }  // State IN UI
    // ... UI mutates state directly
}

```

**After (UDF - good):**

```
@Composable
fun TipScreen(tipState: TipState, onEvent: (TipEvent) -> Unit) {
    // State READ-ONLY, events only
}

@Composable
fun TipTimeLayout() {
    var tipState by remember { mutableStateOf(TipState()) }  // State in parent
    TipScreen(
        tipState = tipState,
        onEvent = { event -> tipState = tipState.reduce(event) }
    )
}

```

## Benefits

- **Predictable**: Always know where state lives and changes originate.[](https://developer.android.com/develop/ui/compose/architecture)
    
- **Debuggable**: Single source of truth, no hidden mutations.[](https://www.pluralsight.com/resources/blog/guides/understanding-unidirectional-data-flow)
    
- **Composable**: Easy to test UI independently of state logic.[](https://dev.to/sebastienlato/swiftui-data-flow-unidirectional-architecture-17ch)



## **Unidirectional data flow**

A _unidirectional data flow_ (UDF) is a design pattern in which state flows down and events flow up. By following unidirectional data flow, you can decouple composables that display state in the UI from the parts of your app that store and change state.

The UI update loop for an app using unidirectional data flow looks like the following:

- **Event:** Part of the UI generates an event and passes it upward—such as a button click passed to the ViewModel to handle—or an event that is passed from other layers of your app, such as an indication that the user session has expired.
- **Update state:** An event handler might change the state.
- **Display state:** The state holder passes down the state, and the UI displays it.

![[image-3.png|355x249]]

The use of the UDF pattern for app architecture has the following implications:

- The `ViewModel` holds and exposes the state the UI consumes.
- The UI state is application data transformed by the `ViewModel`.
- The UI notifies the `ViewModel` of user events.
- The `ViewModel` handles the user actions and updates the state.
- The updated state is fed back to the UI to render.
- This process repeats for any event that causes a mutation of state.
