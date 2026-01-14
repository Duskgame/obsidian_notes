
- The response from a web service is often formatted in [JSON](https://www.json.org/), a common format to represent structured data.
- A JSON [[Kotlin Object]] is a collection of key-value pairs.
- A collection of JSON objects is a JSON [[Arrays]]. You get a JSON array as a response from a [[Web Service]].
- The keys in a key-value pair are surrounded by quotes. The values can be numbers or strings.
- In [[Kotlin]], data [[Serialization]] tools are available in a separate component, [_kotlinx.serialization_](https://github.com/Kotlin/kotlinx.serialization). The _kotlinx.serialization_ provides [[Sets]] of libraries that convert a JSON string into Kotlin objects.
- There is a community developed Kotlin Serialization Converter library for Retrofit: [retrofit2-kotlinx-serialization-converter](https://github.com/JakeWharton/retrofit2-kotlinx-serialization-converter#kotlin-serialization-converter).
- The _kotlinx.serialization_ matches the keys in a JSON response with [[Kotlin Class Properties]] in a data object that have the same name.
- To use a different property name for a key, annotate that property with the `@SerialName` annotation and the JSON key `value`.