https://kotlinlang.org/docs/serialization.html
Serialization is the process of converting data used by an application to a format that can be transferred over a network or stored in a database or a file. 
In turn, deserialization is the opposite process of reading data from an external source and converting it into a runtime object. Together, ==they are essential to most applications that exchange data with third parties.==

Some data serialization formats, such as [JSON](https://www.json.org/json-en.html) and [protocol buffers](https://developers.google.com/protocol-buffers) are particularly common. Being language-neutral and platform-neutral, they enable data exchange between systems written in any modern language.

In Kotlin, data serialization tools are available in a separate component, [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization). It consists of several parts: the `org.jetbrains.kotlin.plugin.serialization` Gradle plugin, [runtime libraries](https://kotlinlang.org/docs/serialization.html#libraries), and compiler plugins.

Compiler plugins, `kotlinx-serialization-compiler-plugin` and `kotlinx-serialization-compiler-plugin-embeddable`, are published directly to Maven Central. The second plugin is designed for working with the `kotlin-compiler-embeddable` artifact, which is the default option for scripting artifacts. Gradle adds compiler plugins to your projects as compiler arguments.