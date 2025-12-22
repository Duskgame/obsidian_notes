
The `ViewModel` component holds and exposes the state the UI consumes. The UI state is application data transformed by `ViewModel`. `ViewModel` lets your app follow the architecture principle of driving the UI from the model.

`ViewModel` stores the app-related data that isn't destroyed when the activity is destroyed and recreated by the Android framework. Unlike the activity instance, `ViewModel` objects are not destroyed. The app automatically retains `ViewModel` objects during configuration changes so that the data they hold is immediately available after the recomposition.

To implement `ViewModel` in your app, extend the `ViewModel` class, which comes from the architecture components library and stores app data within that class.