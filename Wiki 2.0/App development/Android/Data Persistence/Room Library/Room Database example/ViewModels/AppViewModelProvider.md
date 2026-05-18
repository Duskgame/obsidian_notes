```kotlin
/**
 * Provides Factory to create instance of ViewModel for the entire Inventory app
 */
object AppViewModelProvider {
    val Factory = viewModelFactory {
        // Initializer for ItemEditViewModel
        initializer {
            ItemEditViewModel(
                this.createSavedStateHandle(),
                inventoryApplication().container.itemsRepository
            )
        }
        // Initializer for ItemEntryViewModel
        initializer {
            ItemEntryViewModel(inventoryApplication().container.itemsRepository)
        }

        // Initializer for ItemDetailsViewModel
        initializer {
            ItemDetailsViewModel(
                this.createSavedStateHandle(),
                inventoryApplication().container.itemsRepository
            )
        }

        // Initializer for HomeViewModel
        initializer {
            HomeViewModel(inventoryApplication().container.itemsRepository)
        }
    }
}

/**
 * Extension function to queries for [Application] object and returns an instance of
 * [InventoryApplication].
 */
fun CreationExtras.inventoryApplication(): InventoryApplication =
    (this[AndroidViewModelFactory.APPLICATION_KEY] as InventoryApplication)

```

This [[Kotlin]] code defines a centralized [[Factory]] for creating ViewModels in an [[Android]] inventory app, enabling [[Dependency Injection|DI]] for [[Database]]-related components without tight coupling between UI layers and [[Data Persistence]].

## Purpose of AppViewModelProvider

The [[Kotlin Object|Object]] AppViewModelProvider serves as a singleton [[Factory]] provider for the app's ViewModels, using `viewModelFactory` (from AndroidX Lifecycle library). This pattern follows recommended Android architecture—separating UI (ViewModels), business logic, and data layers ([[Repository]] pattern)—to manage [[Database]] operations cleanly. ViewModels like [[ItemEditViewModel]] depend on [[ItemsRepository]], which typically abstracts [[Room]] [[Database]] access, [[Coroutines]]/Flow for async queries, and possibly remote APIs.[](https://stackoverflow.com/questions/27915801/android-app-architecture-separate-code-and-database-layers)

## ViewModel Initializers Explained

Each `initializer` block creates a specific [[ViewModel]] instance with required dependencies:

- [[ItemEditViewModel]] and [[ItemDetailsViewModel]] receive a `SavedStateHandle` (for surviving configuration changes like rotations) and the [[Repository]] from [[InventoryApplication]]().container.[[ItemsRepository]].
- [[ItemEntryViewModel]] and [[HomeViewModel]] only need the [[Repository]].  
    The factory matches ViewModel classes automatically at runtime, injecting the shared [[ItemsRepository]]—ensuring all UI components use the same [[Database Instance]] without passing Context or Repository manually.[](https://stackoverflow.com/questions/76210060/android-correct-way-to-initialize-viewmodel-with-dependencies-viewmodelprovide)

## Dependency Injection Flow

[[InventoryApplication]]().container.itemsRepository retrieves the app-wide Repository from a custom [[InventoryApplication]] subclass. In Android architecture:
- `Application.container` holds singletons like Database ([[Room]]), [[Data Access Object|DAO]], and Repository.
- This avoids leaking Context into ViewModels and promotes testability (e.g., swap Repository for fakes).[](https://stackoverflow.com/questions/76210060/android-correct-way-to-initialize-viewmodel-with-dependencies-viewmodelprovide)​  
    Usage in Activity/Fragment: `viewModel(factory = AppViewModelProvider.Factory)` creates instances transparently.

## Extension Function Role

The `inventoryApplication()` extension on `CreationExtras` extracts `InventoryApplication` from Android's `APPLICATION_KEY` during ViewModel creation. This bridges the framework's internals to app-specific DI, keeping factories lifecycle-aware and scoped to the app process.[](https://www.linkedin.com/pulse/understanding-internal-workings-viewmodel-android-bibekananda-nayak-ljjvc)​

## Database Architecture Benefits

In broader Android [[Model-View-ViewModel|MVVM]] with [[Room]]:

- Repository handles [[CRUD]] on database (e.g., `Flow<List<Item>> getAllItems()`).
- ViewModels expose this as [[StateFlow]]/LiveData to UI, surviving config changes.
- Factory centralizes access, reducing boilerplate and enabling Hilt/Dagger integration for production apps.