![[image-15.png|575x233]]
![[image-16.png|575x236]]

Most web servers today run web services using a common stateless web architecture known as [REST](https://en.wikipedia.org/wiki/Representational_state_transfer), which stands for **RE**presentational **S**tate **T**ransfer. Web services that offer this architecture are known as RESTful services.

Requests are made to RESTful web services in a standardized way, via [Uniform Resource Identifiers](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) (URIs). A URI identifies a resource in the server by name, without implying its location or how to access it. For example, in the app for this lesson, you retrieve the image URLs using the following server URI. (This server hosts both Mars real estate and Mars photos):

[android-kotlin-fun-mars-server.appspot.com](https://android-kotlin-fun-mars-server.appspot.com)

A URL (Uniform Resource Locator) is a subset of a URI that specifies where a resource exists and the mechanism for retrieving it.

**For example:**

The following URL gets a list of available real estate properties on Mars:

[https://android-kotlin-fun-mars-server.appspot.com/realestate](https://android-kotlin-fun-mars-server.appspot.com/realestate)

The following URL gets a list of Mars photos:

[https://android-kotlin-fun-mars-server.appspot.com/photos](https://android-kotlin-fun-mars-server.appspot.com/photos)

These URLs refer to an identified resource, such as [/realestate](https://android-kotlin-fun-mars-server.appspot.com/realestate) or [/photos](https://android-kotlin-fun-mars-server.appspot.com/photos), that is obtainable via the Hypertext Transfer Protocol (_http:_) from the network. You are using the [/photos](https://android-kotlin-fun-mars-server.appspot.com/photos) endpoint in this codelab. An endpoint is a URL that allows you to access a web service running on a server.

**Note**: The familiar web URL is actually a type of URI. This course uses both URL and URI interchangeably.

## Web service request

Each web service request contains a URI and is transferred to the server using the same HTTP protocol that's used by web browsers, like Chrome. HTTP requests contain an operation to tell the server what to do.

Common HTTP operations include:

- GET for retrieving server data.
- POST for creating new data on the server.
- PUT for updating existing data on the server.
- DELETE for deleting data from the server.

Your app makes an HTTP GET request to the server for the Mars photos information, and then the server returns a response to your app, including the image URLs.

![[image-17.png|598x239]]
![[image-18.png|600x245]]

The response from a web service is formatted in one of the common data formats, like XML (eXtensible Markup Language) or JSON (JavaScript Object Notation). The JSON format represents structured data in key-value pairs. An app communicates with the REST API using JSON, which you learn more about in a later task.

