The drive [[User Interface|UI]] from a model principle states that you should drive your UI from a model, preferably a persistent model. Models are components responsible for handling the data for an app. They're independent from the UI elements and app components in your app, so they're unaffected by the app's lifecycle and associated concerns.

## Recommended app architecture

Considering the common architectural principles mentioned in the previous section, each app should have at least two layers:

- **UI layer:** a layer that displays the app data on the screen but is independent of the data.
- **Data layer:** a layer that stores, retrieves, and exposes the app data.

You can add another layer, called the domain layer, to simplify and reuse the interactions between the UI and data layers. 

![[image.png|387x225]]

**Note**: The arrows in the diagrams in this guide represent dependencies between classes. For example, the domain layer depends on data layer classes.

### UI layer

The role of the UI layer, or presentation layer, is to display the application data on the screen. Whenever the data changes due to a user interaction, such as pressing a button, the UI should update to reflect the changes.

The UI layer is made up of the following components:

- **UI elements:** components that render the data on the screen. You build these elements using [Jetpack Compose](https://developer.android.com/jetpack/compose).
- **[[State in Compose|State]] holders:** components that hold the data, expose it to the UI, and handle the app logic. An example state holder is [[View model]].

![[image-1.png|457x319]]


