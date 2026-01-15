```
interface AppContainer {  
    val amphibianInfoRepository: AmphibianInfoRepository  
}  
  
class DefaultAppContainer : AppContainer {  
    private val baseUrl =  
        "https://android-kotlin-fun-mars-server.appspot.com/"  
  
    private val retrofit = Retrofit.Builder()  
        .addConverterFactory(Json.asConverterFactory("application/json".toMediaType()))  
        .baseUrl(baseUrl)  
        .build()  
  
    private val retrofitService: AmphibianApiService by lazy {  
        retrofit.create(AmphibianApiService::class.java)  
    }  
  
    override val amphibianInfoRepository: AmphibianInfoRepository by lazy {  
        NetworkAmphibianRepository(retrofitService)  
    }  
  
}
```

AppContainer is a **[[Dependency Injection]] container** that follows Google's manual DI pattern. It centralizes the creation and configuration of your app's **[[Data Layer]]** (Retrofit + [[Repository]]) so all screens/ViewModels can share the **same single [[Kotlin Object|Instance]]** without duplicating networking setup.

## Role in app architecture
```
UI Layer (Composables/ViewModels)
         ↓
AppContainer ← provides → Data Layer (Repository)
                           ↓
                      Network (Retrofit)

```

**Interface (`AppContainer`)**: Defines the **public contract** - what the rest of the app can access
```
interface AppContainer {
    val amphibianInfoRepository: AmphibianInfoRepository  // Only this is exposed
}

```

**Concrete implementation (`DefaultAppContainer`)**: Does the heavy lifting
```
class DefaultAppContainer : AppContainer {
    // Private: Network config details hidden from the app
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://android-kotlin-fun-mars-server.appspot.com/")  // [attached_file:1]
        .addConverterFactory(Json.asConverterFactory("application/json".toMediaType()))
        .build()

    // Lazy: Created only when first accessed (saves memory)
    private val retrofitService: AmphibianApiService by lazy {
        retrofit.create(AmphibianApiService::class.java)
    }

    '// Public: The only thing screens/ViewModels see
    override val amphibianInfoRepository: AmphibianInfoRepository by lazy {
        NetworkAmphibianRepository(retrofitService)
    }
}

```

## Key benefits

- **Single source of truth**: All screens use the **same Retrofit/Repository instance**
- **Testability**: Inject a fake `AppContainer` with mock data for tests
- **Clean boundaries**: [[User Interface|UI]] only knows about `AmphibianInfoRepository`, not Retrofit details
- **Memory efficient**: `lazy` creates objects only when needed

This is the **manual dependency injection** pattern recommended by Google for small-to-medium apps before moving to Hilt/Dagger.