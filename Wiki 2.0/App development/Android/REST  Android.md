---
aliases:
---
### REST web services

- A _web service_ is software-based functionality, offered over the internet, that enables your app to make requests and get data back.
- Common web services use a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architecture. Web services that offer REST architecture are known as _RESTful_ services. RESTful web services are built using standard web components and protocols.
- You make a request to a REST [[Web Service]] in a standardized way via URIs.
- To use a web service, an app must establish a network connection and communicate with the service. Then the app must receive and parse response data into a format the app can use.
- The [Retrofit](https://square.github.io/retrofit/) library is a client library that enables your app to make requests to a REST web service.
- Use converters to tell Retrofit what to do with the data it sends to the web service and gets back from the web service. For example, the `ScalarsConverter` treats the web service data as a `String` or other primitive.
- To enable your app to make connections to the internet, add the `"android.permission.INTERNET"` permission in the [[Android]] manifest.
- Lazy initialization [[Delegates]] the creation of an object to the first time it is used. It creates the reference but not the [[Kotlin Object|Object]]. When an object is accessed for the first time, a reference is created and used every time thereafter.