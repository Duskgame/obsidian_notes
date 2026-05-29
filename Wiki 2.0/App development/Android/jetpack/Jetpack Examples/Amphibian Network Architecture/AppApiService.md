[[Jetpack Compose]]
```
interface AmphibianApiService {  
    @GET("amphibians")  
    suspend fun getInfos(): List<AmphibianInfo>  
}
```

`AmphibianApiService` is the **raw network contract** - a Retrofit [[Interface]] that declares exactly what HTTP endpoint your app calls. It's the **lowest layer** in the [[Data Layer]], sitting directly between Retrofit and your [[Repository]].

## Position in the architecture stack
```
UI Layer
  ↓
ViewModel → Repository Interface
            ↓ implements
         NetworkAmphibianRepository → AmphibianApiService ← Retrofit
                                          ↓
                                    HTTP GET /amphibians
                                          ↓
                               server response → AmphibianInfo objects

```

## What each part does
```
interface AmphibianApiService {
    @GET("amphibians")                    // HTTP GET request to "/amphibians"
    suspend fun getInfos(): List<AmphibianInfo>  // Returns deserialized JSON
}
```

**`@GET("amphibians")`**: Tells Retrofit to:
- Make a `GET` request to `https://Android-Kotlin-fun-mars-server.appspot.com/amphibians`
- Expect [[JSON]] response containing `List<AmphibianInfo>`
**`suspend fun`**: Makes it coroutine-friendly - can be called from `[[ViewModelScope]].launch { }`


## The complete data flow
```
1. ViewModel: repository.getAmphibianInfos()
2. Repository: amphibianApiService.getInfos()
3. Retrofit: GET https://android-kotlin-fun-mars-server.appspot.com/amphibians
4. Server: returns JSON [ {id:1, name:"Frog"...}, ... ]
5. Retrofit: JSON → List<AmphibianInfo> (using kotlinx.serialization)
6. Repository: returns List to ViewModel
7. ViewModel: updates uiState → UI recomposes
```

## Why it's an interface (not a class)

Retrofit uses **dynamic proxies** at runtime:

- No manual implementation needed
- `retrofit.create()` generates the HTTP client automatically
- All your code sees is the clean interface declaration

This is the **thinnest possible network layer** - just a declaration of "I need this endpoint, make it happen." The repository and [[AppContainer]] handle everything else.
