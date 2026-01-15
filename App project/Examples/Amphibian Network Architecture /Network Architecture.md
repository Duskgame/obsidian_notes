base directory
	data
		[[AppContainer]]
		[[AppRepository]]
	model
		[[AppDataClss]]
		[[AppUiState]]
	network
		[[AppApiService]]
	ui
		[[AppHomeScreen]]
		[[AppViewModel]]
	[[AppApp]]
[[AppApplication]]


```
APP START → AmphibianInfosApplication.onCreate()
            ↓
        container = DefaultAppContainer()  [1]
            ↓ (Retrofit configured)
        retrofitService = retrofit.create() [2]
            ↓
        amphibianInfoRepository = NetworkAmphibianRepository [3]

MainActivity.setContent {
    AmphibianApp()  [4]
        ↓ Scaffold + TopAppBar (scrollBehavior)
        ↓
    amphibianViewModel = viewModel(AmphibianViewModel.Factory) [5]
        ↓ ViewModel.Factory initializer:
        ↓ application.container.amphibianInfoRepository [6]
        ↓ AmphibianViewModel(repository) created [7]
            ↓ init() calls getAmphibianInfos() [8]
                ↓ viewModelScope.launch { } [9]
                    ↓ amphibianUiState = Loading [10]
                    ↓ repository.getAmphibianInfos() [11]
                        ↓ amphibianApiService.getInfos() [12]
                            ↓ Retrofit: GET /amphibians [13]
                            ↓ JSON → List<AmphibianInfo> [14]
                    ↓ amphibianUiState = Success(list) [15]
        ↓
    HomeScreen(amphibianUiState, retryAction) [16]
        ↓ when(Success) → InfoColumnScreen [17]
            ↓ LazyColumn { items(list) } [18]
                ↓ AmphibianInfoCard [19]
                    ↓ AsyncImage(info.imageSrc) [20]
```

```
Mars Server API
    ↓ HTTP GET /amphibians
[13]AmphibianApiService ──┐
    ↓ retrofit.create()   │
[2] DefaultAppContainer ──┼── [3]NetworkAmphibianRepository
    ↓ container           │       ↓ implements
[1]AmphibianInfosApp   ───┼── [11]AmphibianInfoRepository
                          │
[5]AmphibianApp   ────────┼── [6]ViewModel.Factory
    ↓ viewModel()         │       ↓
[7]AmphibianViewModel   ──┘    amphibianInfoRepository
    ↓ init → getAmphibianInfos()
    ↓ uiState: Loading → Success/Error [15]
         ↓
[16]HomeScreen ── when(uiState) ─→ [17]InfoColumnScreen → [19]AmphibianInfoCard [20]

```

```
1. Application → creates → AppContainer (singleton)
2. AppContainer → creates → Retrofit → AmphibianApiService  
3. AppContainer → creates → NetworkAmphibianRepository
4. AmphibianApp → creates → ViewModel (using Factory)
5. ViewModel.Factory → gets → repository from Application.container
6. ViewModel → calls → repository.getAmphibianInfos()
7. Repository → delegates → apiService.getInfos()
8. ApiService → Retrofit → Network → JSON → AmphibianInfo objects
9. ViewModel → emits → amphibianUiState = Success(data)
10. HomeScreen → observes → uiState → recomposes → renders list
11. User taps retry → retryAction() → ViewModel.getAmphibianInfos()
```

