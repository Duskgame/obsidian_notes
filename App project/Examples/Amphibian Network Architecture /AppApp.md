```
@Composable
fun AmphibianApp() {
    val scrollBehavior = TopAppBarDefaults.enterAlwaysScrollBehavior()
    Scaffold(
        modifier = Modifier.nestedScroll(scrollBehavior.nestedScrollConnection),
        topBar = { AmphibianTopAppBar(scrollBehavior = scrollBehavior) }
    ) {
        Surface(
            modifier = Modifier.fillMaxSize()
        ) {
            val amphibianViewModel: AmphibianViewModel =
                viewModel(factory = AmphibianViewModel.Factory)
            HomeScreen(
                amphibianUiState = amphibianViewModel.amphibianUiState,
                retryAction = amphibianViewModel::getAmphibianInfos,
                contentPadding = it,
            )
        }
    }
}

@Composable
fun AmphibianTopAppBar(scrollBehavior: TopAppBarScrollBehavior, modifier: Modifier = Modifier) {
    CenterAlignedTopAppBar(
        scrollBehavior = scrollBehavior,
        title = {
            Text(
                text = stringResource(R.string.app_name),
                style = MaterialTheme.typography.headlineSmall,
            )
        },
        modifier = modifier
    )
}
```

`AmphibianApp()` is the **root [[User Interface|UI]] composable** that wires together the entire [[App Architecture]]: it creates the ViewModel (with [[Dependency Injection|DI]]), observes [[UI State]], and provides Material Design structure with a collapsing top bar.

## Complete architecture wiring
```
AmphibianApp() ← Entry point from MainActivity
     ↓
ViewModel.Factory → Application.container → Repository → Network
     ↓
amphibianUiState → HomeScreen (shows Loading/Success/Error)

```

## Key functionality breakdown

**1. Material 3 Scaffold with scroll behavior**
```
val scrollBehavior = TopAppBarDefaults.enterAlwaysScrollBehavior()
Scaffold(
    modifier = Modifier.nestedScroll(scrollBehavior.nestedScrollConnection),  // Enables collapsing
    topBar = { AmphibianTopAppBar(scrollBehavior = scrollBehavior) }
)
```

- **`enterAlwaysScrollBehavior()`**: Top bar collapses/expands when scrolling content
- **`nestedScroll()`**: Connects Scaffold to child scrollable content (LazyColumn in HomeScreen)
- Creates the **Material Design collapsing toolbar effect**

**2. ViewModel creation with manual DI**
```
val amphibianViewModel: AmphibianViewModel = 
    viewModel(factory = AmphibianViewModel.Factory)
```

- **`AmphibianViewModel.[[Factory]]`** gets repository from `Application.container`
- **Single ViewModel [[Kotlin Object|Instance]]** survives recompositions/config changes
- ViewModel auto-loads data on init via `getAmphibianInfos()`

**3. [[State in Compose|State]] observation + event forwarding**
```
HomeScreen(
    amphibianUiState = amphibianViewModel.amphibianUiState,     // Read-only state
    retryAction = amphibianViewModel::getAmphibianInfos,       // Event callback
    contentPadding = it                                        // Scaffold padding
)
```

- **Unidirectional flow**: UI state flows **down** from ViewModel, events flow **up**
- `retryAction` called when user taps "Retry" on Error screen
- `contentPadding` prevents content from hiding behind top bar

## How everything connects
```
App starts → MainActivity → setContent { AmphibianApp() }
1. ViewModel created → loads data → emits Success/Loading/Error
2. HomeScreen reads uiState → renders appropriate UI
3. User scrolls → TopAppBar collapses (nestedScroll)
4. User taps retry → calls ViewModel function → reloads
```

## Clean MVVM separation

 **UI Layer**: Scaffold, TopAppBar, HomeScreen - pure presentation  
 **ViewModel Layer**: Business logic, state management, data loading  
 **[[Data Layer]]**: [[Repository]] → Network (hidden behind [[OOP|abstraction]])

This is the **complete reference implementation** of Google's recommended [[Jetpack Compose|Compose]] + MVVM + Repository architecture pattern.