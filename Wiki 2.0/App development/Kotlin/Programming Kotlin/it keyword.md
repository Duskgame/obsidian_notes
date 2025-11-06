### implicit name of a single parameter﻿

It's very common for a lambda expression to have only one [[Parameter]].

If the compiler can parse the signature without any parameters, the [[Parameter]] does not need to be declared and `->` can be omitted. The [[Parameter]] will be implicitly declared under the name `it`:

```
ints.filter { it > 0 } // this literal is of type '(it: Int) -> Boolean'
```


In conventional programming when you loop through a collection you might do:  
`for (element in collection) { println(element) }`

When using [[Kotlin]] functional features you can do something like:  
```
collection.forEach { println(it)}
```


Which is equivalent to the code above

```
val strings = someArray.map { it.toString() }
```

`map` is used to transform a array of one type into another. If you just want to execute something for each element the `forEach` [[Function]] is used.