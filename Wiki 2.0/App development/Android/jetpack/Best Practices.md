#jetpack_compose
- ## It is a best practice to pass a modifier to every composable and set it to a default value.




Programming is a creative field, and building [[Android]] apps isn't an exception. There are many ways to solve a problem; you might communicate data between multiple activities or fragments, retrieve remote data and persist it locally for offline mode, or handle any number of other common scenarios that nontrivial apps encounter.

Although the following recommendations aren't mandatory, in most cases following them makes your codebase more robust, testable, and maintainable.

## **Don't store data in app components.**

Avoid designating your app's entry points—such as activities, services, and broadcast receivers—as sources of data. The entry points should only coordinate with other components to retrieve the subset of data that is relevant to that entry point. Each app component is short‑lived, depending on the user's interaction with their device and capacity of the system.

## **Reduce dependencies on Android classes.**

Your app components should be the only classes that rely on Android framework SDK APIs such as [`Context`](https://developer.android.com/reference/android/content/Context) or [`Toast`](https://developer.android.com/guide/topics/ui/notifiers/toasts). Abstracting other classes in your app away from the app components helps with testability and reduces [coupling](https://en.wikipedia.org/wiki/Coupling_\(computer_programming\)) within your app.

## **Define clear boundaries of responsibility between modules in your app.**

Don't spread the code that loads data from the network across multiple classes or packages in your codebase. Similarly, don't define multiple unrelated responsibilities, such as data caching and data binding, in the same [[Kotlin Class|Class]]. Following the [recommended app architecture](https://developer.android.com/topic/architecture#recommended-app-arch) will help.

## **Expose as little as possible from each module.**

Don't create shortcuts that expose internal implementation details. You might gain a bit of time in the short term, but you are then likely to incur technical debt many times over as your codebase evolves.

## **Focus on the unique core of your app so it stands out from other apps.**

Don't reinvent the wheel by writing the same boilerplate code again and again. Instead, focus your time and energy on what makes your app unique. Let the [[jetpack]] libraries and other recommended libraries handle the repetitive boilerplate.

## **Use canonical layouts and app design patterns.**

The [[jetpack]] Compose libraries provide robust APIs for building adaptive user interfaces. Use the [canonical layouts](https://developer.android.com/develop/ui/compose/layouts/adaptive/canonical-layouts) in your app to optimize the user experience on multiple form factors and display sizes. Review the [gallery](https://developer.android.com/large-screens/gallery) of app design patterns to select the layouts that work best for your use cases.

## **Preserve UI state across configuration changes.**

When designing for adaptive layouts, preserve [[User Interface|UI]] [[State in Compose|State]] across configuration changes such as display resizing, folding, and orientation changes. Your architecture should verify that the user's current state is maintained, providing a seamless experience.

## **Design reusable and composable UI components.**

Build UI components that are reusable and composable to support adaptive design. This lets you combine and rearrange components to fit various screen sizes and postures without significant refactoring.

## **Consider how to make each part of your app testable in isolation.**

A well-defined [[API]] for fetching data from the network facilitates [[Testing Projects]] the module that persists that data in a local database. If instead, you mix the logic from these two functions in one place, or distribute your networking code across your entire codebase, testing becomes much more difficult, if not impossible.

## **Types are responsible for their concurrency policy.**

If a type is performing long-running blocking work, the type should be responsible for moving that computation to the right thread. The type knows the kind of computation that it is doing and in which thread the computation should be executed. Types should be main‑safe, meaning they're safe to call from the main thread without blocking it.

## **Persist as much relevant and fresh data as possible.**

That way, users can enjoy your app's functionality even when their device is in offline mode. Remember that not all of your users enjoy constant, high‑speed connectivity, and even if they do, they can get bad reception in crowded places.

