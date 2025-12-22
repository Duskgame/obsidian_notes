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
    

This matches your earlier **state hoisting** questions perfectly - that's UDF in action!