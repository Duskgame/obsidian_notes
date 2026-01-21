[[Jetpack Compose]]

```
@Composable
fun HomeScreen(
    amphibianUiState: AmphibianUiState,
    retryAction: () -> Unit,
    modifier: Modifier = Modifier,
    contentPadding: PaddingValues = PaddingValues(0.dp),
) {
    when (amphibianUiState) {
        is AmphibianUiState.Loading -> LoadingScreen(modifier.fillMaxSize())
        is AmphibianUiState.Success -> InfoColumnScreen(
            amphibianUiState.infos,
            modifier,
            contentPadding = contentPadding
        )

        is AmphibianUiState.Error -> ErrorScreen(modifier = modifier.fillMaxSize(), retryAction)
    }
}

@Composable
fun LoadingScreen(modifier: Modifier = Modifier) {
    Image(
        modifier = modifier.size(200.dp),
        painter = painterResource(R.drawable.loading_img),
        contentDescription = stringResource(R.string.loading)
    )
}

@Composable
fun ErrorScreen(modifier: Modifier = Modifier, retryAction: () -> Unit) {
    Column(
        modifier = modifier,
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Image(
            painter = painterResource(id = R.drawable.ic_connection_error), contentDescription = ""
        )
        Text(text = stringResource(R.string.loading_failed), modifier = Modifier.padding(16.dp))
        Button(onClick = retryAction) {
            Text(stringResource(R.string.retry))
        }
    }
}

@Composable
fun AmphibianInfoCard(info: AmphibianInfo, modifier: Modifier = Modifier) {
    Card(
        modifier = modifier,
        elevation = CardDefaults.cardElevation(defaultElevation = 8.dp)
    ) {
        Text("${info.name}  (${info.type})")
        AsyncImage(
            model = ImageRequest.Builder(context = LocalContext.current)
                .data(info.imageSrc)
                .crossfade(true)
                .build(),
            error = painterResource(R.drawable.ic_broken_image),
            placeholder = painterResource(R.drawable.loading_img),
            contentDescription = stringResource(R.string.amphibian_picture),
            contentScale = ContentScale.Crop,
            modifier = Modifier.fillMaxWidth()
        )
        Text(info.description)
    }
}

@Composable
fun InfoColumnScreen(
    infos: List<AmphibianInfo>,
    modifier: Modifier = Modifier,
    contentPadding: PaddingValues = PaddingValues(0.dp),
) {
    LazyColumn(
        modifier = modifier.padding(4.dp),
        contentPadding = contentPadding
    ) {
        items(items = infos) { info ->
            AmphibianInfoCard(
                info = info,
                modifier = modifier
                    .padding(4.dp)
                    .fillMaxWidth()
            )
        }
    }
}
```

`HomeScreen` and its child composables represent the **pure [[User Interface|UI]] presentation layer** that **reactively renders** the `AmphibianUiState` from the ViewModel using an exhaustive `when` expression. This completes the [[Unidirectional data Flow]] from network → ViewModel → UI.

## Architecture role: State → UI mapping
```
ViewModel.amphibianUiState → HomeScreen → renders ONE of: Loading/Success/Error
```

## Core pattern: Exhaustive state handling
```
@Composable
fun HomeScreen(amphibianUiState: AmphibianUiState, retryAction: () -> Unit) {
    when (amphibianUiState) {
        is Loading -> LoadingScreen()           // Spinner while network call
        is Success -> InfoColumnScreen(...)     // List of amphibian cards  
        is Error -> ErrorScreen(retryAction)    // Error + retry button
    }
}
```

**Key principle**: UI is **always** in exactly **one [[State in Compose|State]]** - no overlapping flags or [[null]] checks.
## Component breakdown

**`LoadingScreen`**: Simple image placeholder during network fetch  
**`ErrorScreen`**: User-friendly error with retry callback (calls ViewModel's `getAmphibianInfos()`)  
**`InfoColumnScreen`**: `LazyColumn` renders amphibian cards efficiently  
**`AmphibianInfoCard`**: Single item with `AsyncImage` (Coil library) + text data
## Unidirectional flow in action
```
1. AppContainer → Repository → Network → List<AmphibianInfo>
2. ViewModel → AmphibianUiState.Success(list)
3. HomeScreen reads state → when(Success) → InfoColumnScreen → LazyColumn
4. User taps retry (Error state) → retryAction() → ViewModel reloads
5. ViewModel → Loading → HomeScreen recomposes → shows spinner
```

## Clean separation complete
```
 Data Layer:     AppContainer → Repository → Retrofit (handles network)
 ViewModel Layer: Orchestrates loading/error states  
 UI Layer:       HomeScreen renders state + forwards events back to ViewModel
```

**No business logic in UI** - just declarative rendering of ViewModel state. This is the **reference implementation** of Google's recommended [[Jetpack Compose|Compose]] + MVVM + [[Repository]] pattern.