When you added methods to [[ItemDao]] to get items- `getItem()` and `getAllItems()`- you specified a `Flow` as the return type. Recall that a `Flow` represents a generic stream of data. By returning a `Flow`, you only need to explicitly call the methods from the [[Data Access Object|DAO]] once for a given lifecycle. [[Room]] handles updates to the underlying data in an asynchronous manner.

Getting data from a flow is called _collecting from a flow_. When collecting from a flow in your [[User Interface|UI]] layer, there are a few things to consider.

- Lifecycle events like configuration changes, for example rotating the device, causes the activity to be recreated. This causes recomposition and collecting from your `Flow` all over again.
- You want the values to be cached as [[State in Compose|State]] so that existing data isn't lost between lifecycle events.
- Flows should be canceled if there's no observers left, such as after a composable's lifecycle ends.

The recommended way to expose a `Flow` from a [[ViewModel]] is with a [[StateFlow]]. Using a `StateFlow` allows the data to be saved and observed, regardless of the UI lifecycle. To convert a `Flow` to a `StateFlow`, you use the `stateIn` [[Keywords and operators|operator]].

The `stateIn` operator has three parameters which are explained below:

- `scope` - The [[ViewModelScope]] defines the lifecycle of the `StateFlow`. When the `viewModelScope` is canceled, the `StateFlow` is also canceled.
- `started` - The pipeline should only be active when the UI is visible. The `SharingStarted.WhileSubscribed()` is used to accomplish this. To configure a delay (in milliseconds) between the disappearance of the last subscriber and the stopping of the sharing coroutine, pass in the `TIMEOUT_MILLIS` to the `SharingStarted.WhileSubscribed()` [[Kotlin Class Method|Method]].
- `initialValue` - Set the initial value of the state flow to `HomeUiState()`.

Once you've converted your `Flow` into a `StateFlow`, you can collect it using the `collectAsState()` method, converting its data into `State` of the same type.



In this step, you'll retrieve all items in the [[Room]] [[Database]] as a `StateFlow` observable [[API]] for [[UI State]]. When the [[Room]] Inventory data changes, the UI updates automatically.

1. Open the ui/home/[[HomeViewModel]].kt file, which contains a `TIMEOUT_MILLIS` constant and a `HomeUiState` [[Data Class]] with a list of items as a [[Kotlin Constructor|constructor]] [[Parameter]].

```kotlin
// No need to copy over, this code is part of starter code

class HomeViewModel : ViewModel() {

    companion object {
        private const val TIMEOUT_MILLIS = 5_000L
    }
}

data class HomeUiState(val itemList: List<Item> = listOf())

```

2. Inside the [[HomeViewModel]] class, declare a `val` called `homeUiState` of the type `StateFlow<HomeUiState>`. You will resolve the initialization error shortly.

```
val homeUiState: StateFlow<HomeUiState>
```

3. Call `getAllItemsStream()` on [[ItemsRepository]] and assign it to `homeUiState` you just declared.

```kotlin
val homeUiState: StateFlow<HomeUiState> =
    itemsRepository.getAllItemsStream()
```

You now get an error - Unresolved reference: [[ItemsRepository]]. To resolve the Unresolved reference error, you need to pass in the [[ItemsRepository]] [[Kotlin Object|Object]] to the [[HomeViewModel]].

4. Add a constructor parameter of the type `ItemsRepository` to the `HomeViewModel` class.

```Kotlin
import com.example.inventory.data.ItemsRepository

class HomeViewModel(itemsRepository: ItemsRepository): ViewModel() {
```

5. In the ui/[[AppViewModelProvider]].kt file, in the `HomeViewModel` initializer, pass the `ItemsRepository` object as shown.

```kotlin
initializer {
    HomeViewModel(inventoryApplication().container.itemsRepository)
}
```

6. Go back to the `HomeViewModel.kt` file. Notice the type mismatch error. To resolve this, add a transformation map as shown below.

```kotlin
val homeUiState: StateFlow<HomeUiState> =
    itemsRepository.getAllItemsStream().map { HomeUiState(it) }
```

[[Android]] Studio still shows you a type mismatch error. This error is because `homeUiState` is of the type `StateFlow` and `getAllItemsStream()` returns a `Flow`.

7. Use the `stateIn` operator to convert the `Flow` into a `StateFlow`. The `StateFlow` is the observable API for UI state, which enables the UI to update itself.

```kotlin
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.SharingStarted
import kotlinx.coroutines.flow.map
import kotlinx.coroutines.flow.stateIn

val homeUiState: StateFlow<HomeUiState> =
    itemsRepository.getAllItemsStream().map { HomeUiState(it) }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(TIMEOUT_MILLIS),
            initialValue = HomeUiState()
        )

```

8. Build the app to make sure there are no errors in the code. There will not be any visual changes.