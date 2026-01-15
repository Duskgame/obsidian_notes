```
interface AmphibianInfoRepository {  
    suspend fun getAmphibianInfos(): List<AmphibianInfo>  
}  
  
class NetworkAmphibianRepository(  
    private val amphibianApiService: AmphibianApiService  
) : AmphibianInfoRepository {  
    override suspend fun getAmphibianInfos(): List<AmphibianInfo> = amphibianApiService.getInfos()  
}
```

NetworkAmphibianRepository is the **[[Data Layer]]** implementation that acts as an **[[OOP|abstraction]] between your ViewModels and the raw network [[API]]**. It implements the repository pattern from Google's recommended [[Android]] architecture.

```
UI Layer (Composables)
       ↓
ViewModels ← uses → Repository Interface (AmphibianInfoRepository)
                     ↓ implements
              NetworkAmphibianRepository ← delegates → Retrofit Service
                                                ↓
                                           HTTP Network (Mars server) [attached_file:1]

```

**[[Interface]] contract** (what ViewModels see):
```
interface AmphibianInfoRepository {
    suspend fun getAmphibianInfos(): List<AmphibianInfo>
}
```

**Concrete implementation** (network-specific):
```
class NetworkAmphibianRepository(
    private val amphibianApiService: AmphibianApiService  // Injected dependency
) : AmphibianInfoRepository {
    override suspend fun getAmphibianInfos(): List<AmphibianInfo> = 
        amphibianApiService.getInfos()  // Just forwards the network call
}
```

## Why this separation exists

1. **Abstraction**: ViewModels only know about `AmphibianInfoRepository`, not Retrofit details
2. **Single responsibility**: [[Repository]] = data access, ViewModel = business logic
3. **Testability**: Easy to inject a `FakeAmphibianRepository` with mock data for tests
4. **Future-proofing**: Later you can add:
    - Caching: `if (cache.isFresh) return cache else return network`
    - Database: `return database + network`
    - Multiple sources without changing ViewModels

## How the pieces connect
```
AppContainer → amphibianInfoRepository → NetworkAmphibianRepository(retrofitService)
ViewModel → container.amphibianInfoRepository.getAmphibianInfos() → network call
```

**Right now**: Simple pass-through to network  
**Later**: Can add offline caching, error handling, data transformation without touching [[User Interface|UI]] code

This is exactly the pattern from Google's Mars Photos codelab - repository hides network complexity from the UI layer.

