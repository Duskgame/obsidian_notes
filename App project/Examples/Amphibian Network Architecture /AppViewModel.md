```
class AmphibianViewModel(private val amphibianInfoRepository: AmphibianInfoRepository) :
    ViewModel() {
    var amphibianUiState: AmphibianUiState by mutableStateOf(AmphibianUiState.Loading)
        private set

    init {
        getAmphibianInfos()
    }

    fun getAmphibianInfos() {
        viewModelScope.launch {
            amphibianUiState = AmphibianUiState.Loading
            amphibianUiState = try {
                AmphibianUiState.Success(amphibianInfoRepository.getAmphibianInfos())
            } catch (e: IOException) {
                AmphibianUiState.Error
            } catch (e: HttpException) {
                AmphibianUiState.Error
            }
        }
    }

    companion object {
        val Factory: ViewModelProvider.Factory = viewModelFactory {
            initializer {
                val application = (this[APPLICATION_KEY] as AmphibianInfosApplication)
                val amphibianInfoRepository = application.container.amphibianInfoRepository
                AmphibianViewModel(amphibianInfoRepository = amphibianInfoRepository)
            }
        }
    }
}
```

`AmphibianViewModel` is the **[[User Interface|UI]] Layer** bridge that orchestrates data loading from the repository and exposes **sealed [[Kotlin Class|Class]] [[UI State]]** to Composables. It follows the standard MVVM + [[Repository]] pattern from Google's [[Android]] architecture guidance.

## Role in complete architecture flow
```
UI Composable ← observes ← amphibianUiState (Loading/Success/Error)
                      ↓
AmphibianViewModel → Repository → Retrofit → Mars Server
```

## Key functionality breakdown

**1. State exposure (Composable reads this)**
```
var amphibianUiState: AmphibianUiState by mutableStateOf(AmphibianUiState.Loading)
    private set
```

- **Sealed class** pattern: `Loading` → `Success(data)` → `Error`
- `mutableStateOf` → UI automatically recomposes when state changes
- `private set` → UI can't mutate state directly

**2. Data loading with error handling**
```
fun getAmphibianInfos() {
    viewModelScope.launch {  // Structured concurrency - cancels on ViewModel clear
        amphibianUiState = AmphibianUiState.Loading
        amphibianUiState = try {
            AmphibianUiState.Success(amphibianInfoRepository.getAmphibianInfos())
        } catch (e: IOException) {
            AmphibianUiState.Error  // Network failure
        } catch (e: HttpException) {
            AmphibianUiState.Error  // 4xx/5xx errors
        }
    }
}
```

- `[[ViewModelScope]]` → automatically cancels when screen rotates/ViewModel destroyed
- Specific [[Exception Handling]] → network vs HTTP errors both map to `Error` state
- **Single source of truth** for UI loading states

**3. [[Dependency Injection]] via [[Factory]]**
```
companion object {
    val Factory: ViewModelProvider.Factory = viewModelFactory {
        initializer {
            val application = (this[APPLICATION_KEY] as AmphibianInfosApplication)
            val repository = application.container.amphibianInfoRepository
            AmphibianViewModel(repository)
        }
    }
}
```

- **Manual DI**: Gets repository from `Application.container`
- Modern `viewModelFactory` [[API]] ([[Jetpack Compose|Compose]]-friendly)
- Same repository [[Kotlin Object|Instance]] shared across all ViewModels


**ViewModel responsibilities complete**:

-  Fetches data from repository (doesn't know about network)
-  Converts raw data → UI-friendly states
-  Handles all network/error edge cases
-  Survives config changes
-  Automatically cancels work when screen destroyed