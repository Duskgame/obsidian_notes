[[Jetpack Compose]]

```
class AmphibianInfosApplication: Application() {  
    lateinit var container: AppContainer  
    override fun onCreate() {  
        super.onCreate()  
        container = DefaultAppContainer()  
    }  
}
```

`AmphibianInfosApplication` is the **app-wide [[Dependency Injection]] root** - a custom `Application` [[Kotlin Class|Class]] that creates and holds the **singleton `[[AppContainer]]`** so every screen/ViewModel in your app can access the same repository [[Kotlin Object|Object]].

## Role in complete architecture
```
AndroidManifest.xml → AmphibianInfosApplication (singleton)
                           ↓ onCreate()
                    container = DefaultAppContainer()
                           ↓
ViewModel.Factory → application.container → Repository → Network

```

## What it does
```
class AmphibianInfosApplication: Application() {
    lateinit var container: AppContainer  // Public access point
    
    override fun onCreate() {
        super.onCreate()
        container = DefaultAppContainer()  // Creates ONE shared instance
    }
}
```

**`Application()`**: [[Android]]'s **singleton lifecycle** - created once when app starts, lives for entire app lifetime

- Survives Activity recreation (screen rotation, process death + restore)
- Perfect place for app-wide singletons like network clients

**`lateinit var container`**: The **public [[API]]** - every ViewModel gets the same [[Kotlin Object|Instance]] via:

```
val repository = (application as AmphibianInfosApplication).container.amphibianInfoRepository
```

## Required AndroidManifest.xml entry
```
<application
    android:name=".AmphibianInfosApplication"
    ... >

```

## How ViewModel.Factory uses it

From your ViewModel's companion object:
```
initializer {
    val application = (this[APPLICATION_KEY] as AmphibianInfosApplication)
    val repository = application.container.amphibianInfoRepository  // Same instance everywhere!
    AmphibianViewModel(repository)
}
```

## Benefits of this pattern

 **Singleton networking**: One Retrofit/[[Repository]] shared across entire app  
 **Survives config changes**: Container lives in Application scope  
 **Manual DI**: No Hilt/Dagger boilerplate for simple apps  
 **Testable**: Override `container` with fake in tests

**Flow**: App starts → `Application.onCreate()` → creates container → ViewModels get repository → network calls work everywhere.

This is Google's **manual dependency injection pattern** from their official architecture codelabs.