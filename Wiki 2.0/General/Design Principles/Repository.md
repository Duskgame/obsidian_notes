### **What is a repository?**

In [[General]] a repository [[Kotlin Class|Class]]:

- Exposes data to the rest of the app.
- Centralizes changes to data.
- Resolves conflicts between multiple data sources.
- Abstracts sources of data from the rest of the app.
- Contains business logic.

The **Mars Photos** app has a single data source, which is the network [[API]] call. It does not have any business logic, as it is just retrieving data. The data is exposed to the app through the repository class, which abstracts away the source of the data.

![[image-21.png|327x327]]

The [repository naming convention](https://developer.android.com/topic/architecture/data-layer#naming-conventions) is **type of data + Repository**. In your app, this is `MarsPhotosRepository`.

Instead of the `ViewModel` directly making the network request for the data, the repository provides the data. The `ViewModel` no longer directly references the `MarsApi` code.

![[image-22.png|797x184]]

This approach helps make the code retrieving the data loosely coupled from `ViewModel`. Being loosely coupled allows changes to be made to the `ViewModel` or the repository without adversely affecting the other, as long as the repository has a function called `getMarsPhotos()`.