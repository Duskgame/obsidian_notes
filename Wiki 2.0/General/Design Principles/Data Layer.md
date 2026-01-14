## **What is a data layer?**

A data layer is responsible for the business logic of your app and for sourcing and saving data for your app. The data layer exposes data to the [[User Interface]] layer using the [Unidirectional Data Flow](https://developer.android.com/topic/architecture#unidirectional-data-flow) pattern. Data can come from multiple sources, like a network request, a local database, or from a file on the device.

An app may even have more than one source of data. When the app opens, it retrieves data from a local database on the device, which is the first source. While the app is running, it makes a network request to the second source to retrieve newer data.

By having the data in a separate layer from the UI code, you can make changes in one part of the code without affecting another. This approach is part of a design principle called [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns). A section of code focuses on its own concern and encapsulates its inner workings from other code. [[OOP|encapsulation]] is a form of hiding how the code internally works from other sections of code. When one section of code needs to interact with another section of code, it does it through an [[Interface]].

The UI layer's concern is displaying the data it is provided. The UI no longer retrieves the data as that is the data layer's concern.

The data layer is made up of one or more repositories. Repositories themselves contain zero or more data sources.

![[image-20.png|376x336]]

[[Best Practices]] require the app to have a [[repository]] for each type of data source your app uses.

For this app, the repository that retrieves data from the internet completes the data source's responsibilities. It does so by making a network request to an [[API]]. If the data source coding is more complex or additional data sources are added, the data source responsibilities are encapsulated in separate data source classes, and the repository is responsible for managing all the data sources.