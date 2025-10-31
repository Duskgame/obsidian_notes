https://ktor.io/

https://ktor.io/docs/welcome.html
## Why Ktor?

### Kotlin and Coroutines

Ktor is built from the ground up using [Kotlin](https://kotlinlang.org/) and [Coroutines](https://kotlinlang.org/docs/coroutines-guide.html). You get to use a concise, multiplatform language, as well as the power of asynchronous programming with an intuitive imperative flow.

### Lightweight and Flexible

Ktor allows you to use only what you need, and to structure your application the way you need it. In addition you can also extend Ktor with your own plugin very easily.

### Built and backed by JetBrains

Brought to you by [JetBrains,](https://jetbrains.com) creators of IntelliJ IDEA, Kotlin, and more. Ktor is not only used by our customers, but also internally at JetBrains. In addition, you have top-notch [tooling support!](https://ktor.io/idea)


## Create new client application
https://ktor.io/docs/client-create-new-application.html

Problem:
	https://ktor.io/docs/client-create-new-application.html#add-dependencies
	Open the gradle.properties file and add the following line to specify the Ktor version:

```
ktor_version=3.3.1
```

Where is the gradle.properties file?

==Solution==:
	needs to be added to the root directory
	
	main problem was the project wasn't correctly set up
	was set up as java
	
	with the correct setup there is the correct file in the correct
	place