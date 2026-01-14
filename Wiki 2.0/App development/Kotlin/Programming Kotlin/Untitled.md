[_Serialization_](https://kotlinlang.org/docs/serialization.html) is the process of converting data used by an application to a format that can be transferred over a network. As opposed to _serialization_, _deserialization_ is the process of reading data from an external source (like a server) and converting it into a runtime object. They are both essential components of most applications that exchange data over the network.

The `kotlinx.serialization` provides sets of libraries that convert a JSON string into Kotlin objects. There is a community developed third party library that works with Retrofit, [Kotlin Serialization Converter](https://github.com/JakeWharton/retrofit2-kotlinx-serialization-converter#kotlin-serialization-converter).


