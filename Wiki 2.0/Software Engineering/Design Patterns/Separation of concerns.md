
Design your app architecture to follow a few specific principles.

The most important principle is [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns). It's a common mistake to write all your code in an [`Activity`](https://developer.android.com/guide/components/activities/intro-activities) or a [`Fragment`](https://developer.android.com/guide/fragments).

The primary role of an `Activity` or `Fragment` is to host your app's UI. The Android OS controls their lifecycle, frequently destroying and recreating them in response to user actions like screen rotation or system events like low memory.

This ephemeral nature makes them unsuitable for holding application data or state. If you store data in an `Activity` or `Fragment`, that data is lost when the component is recreated. To ensure data persistence and provide a stable user experience, don't entrust state to these UI components.




Separation of concerns is a design principle that says each part of a software system should deal with one distinct aspect (or “concern”) instead of mixing many responsibilities together.​
Core idea

A concern is any specific responsibility or aspect of the system: [[User Interface|UI]], business rules, persistence, [[Logging]], etc.​

Code for each concern is grouped into its own module, layer, or component, with minimal overlap between them.​

Typical examples

Layered architectures: separate presentation (UI), business logic, and data access into different layers.​

Keeping input validation, domain logic, and database code in different classes or packages rather than in a single big [[Kotlin Class|Class]] or [[Function]].​

Benefits

   Maintainability: changes to one concern (e.g., UI) are less likely to break others (e.g., database access).​

   Testability and debugging: smaller, focused modules are easier to understand, test in isolation, and debug.​

   Scalability and reuse: clearly separated components can be reused or evolved independently as the system grows.​