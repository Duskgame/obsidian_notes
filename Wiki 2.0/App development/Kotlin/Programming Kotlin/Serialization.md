https://kotlinlang.org/docs/serialization.html
Serialization is the process of converting data used by an application to a format that can be transferred over a network or stored in a database or a file. 
In turn, deserialization is the opposite process of reading data from an external source and converting it into a runtime object. Together, ==they are essential to most applications that exchange data with third parties.==

Some data serialization formats, such as [JSON](https://www.json.org/json-en.html) and [protocol buffers](https://developers.google.com/protocol-buffers) are particularly common. Being language-neutral and platform-neutral, they enable data exchange between systems written in any modern language.

In [[Kotlin]], data serialization tools are available in a separate component, [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization). It consists of several parts: the `org.jetbrains.kotlin.plugin.serialization` Gradle plugin, [runtime libraries](https://kotlinlang.org/docs/serialization.html#libraries), and compiler plugins.

Compiler plugins, `kotlinx-serialization-compiler-plugin` and `kotlinx-serialization-compiler-plugin-embeddable`, are published directly to Maven Central. The second plugin is designed for working with the `[[Kotlin]]-compiler-embeddable` artifact, which is the default option for scripting artifacts. Gradle adds compiler plugins to your projects as compiler [[Arguments]].



## Example: JSON serialization﻿

Let's take a look at how to serialize [[Kotlin]] objects into JSON.

### Add plugins and dependencies﻿

Before starting, you must configure your build script so that you can use Kotlin serialization tools in your project:

1. Apply the Kotlin serialization Gradle plugin `org.jetbrains.kotlin.plugin.serialization` (or `kotlin("plugin.serialization")` in the Kotlin Gradle DSL).
    

Kotlin

```
plugins {
    kotlin("jvm") version "2.2.21"
    kotlin("plugin.serialization") version "2.2.21"
}
```

- Add the JSON serialization library dependency: `org.jetbrains.kotlinx:kotlinx-serialization-json:1.9.0`
    

Kotlin

```
dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.9.0")
}
```

Now you're ready to use the serialization [[API]] in your code. The [[API]] is located in the `kotlinx.serialization` package and its format-specific subpackages, such as `kotlinx.serialization.json`.

### Serialize and deserialize JSON

- Make a class serializable by annotating it with `@Serializable`.
    
    ```
    import kotlinx.serialization.Serializable
    
    @Serializable
    data class Data(val a: Int, val b: String)
    ```
    

==Serialize an instance of this class by calling `Json.encodeToString()`.==

```
import kotlinx.serialization.Serializable
import kotlinx.serialization.json.Json
import kotlinx.serialization.encodeToString

@Serializable
data class Data(val a: Int, val b: String)

fun main() {
    val json = Json.encodeToString(Data(42, "str"))
}
```

As a result, you get a string containing the state of this object in the JSON format: `{"a": 42, "b": "str"}`

You can also serialize object collections, such as lists, in a single call:

```
val dataList = listOf(Data(42, "str"), Data(12, "test"))
val jsonList = Json.encodeToString(dataList)
```

- Use the `decodeFromString()` [[Function]] to deserialize an object from JSON:
    
    ```
    import kotlinx.serialization.Serializable
    import kotlinx.serialization.json.Json
    import kotlinx.serialization.decodeFromString
    
    @Serializable
    data class Data(val a: Int, val b: String)
    
    fun main() {
        val obj = Json.decodeFromString<Data>("""{"a":42, "b": "str"}""")
    }
    ```
    