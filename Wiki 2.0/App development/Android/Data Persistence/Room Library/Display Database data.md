In this task, you collect and update the UI state in the `HomeScreen`.

1. In the `HomeScreen.kt` file, in the `HomeScreen` composable function, add a new function parameter of the type `HomeViewModel` and initialize it.

```kotlin
import androidx.lifecycle.viewmodel.compose.viewModel
import com.example.inventory.ui.AppViewModelProvider


@Composable
fun HomeScreen(
    navigateToItemEntry: () -> Unit,
    navigateToItemUpdate: (Int) -> Unit,
    modifier: Modifier = Modifier,
    viewModel: HomeViewModel = viewModel(factory = AppViewModelProvider.Factory)
)
```

2. In the `HomeScreen` composable function, add a `val` called `homeUiState` to collect the UI state from the `HomeViewModel`. You use _`collectAsState`_`()`, which collects values from this [`StateFlow`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-state-flow/index.html) and represents its latest value via [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State).

```kotlin
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.getValue

val homeUiState by viewModel.homeUiState.collectAsState()
```

3. Update the `HomeBody()` function call and pass in `homeUiState.itemList` to the `itemList` parameter.

```kotlin
HomeBody(
    itemList = homeUiState.itemList,
    onItemClick = navigateToItemUpdate,
    modifier = modifier.padding(innerPadding)
)
```

4. Run the app. Notice that the inventory list displays if you saved items in your app database. If the list is empty, add some inventory items to the app database.
