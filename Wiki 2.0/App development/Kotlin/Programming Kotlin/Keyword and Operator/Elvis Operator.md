[[Keywords and operators]]

When working with nullable types, you can check for `null` and provide an alternative value. For example, if `b` is not `null`, access `b.length`. Otherwise, return an alternative value:

```

// Assigns null to a nullable variable  

val b: String? = null

// Checks for nullability. If not null, returns length. If null, returns 0

val l: Int = if (b != null) b.length else 0

println(l)

// 0
```



Instead of writing the complete `if` expression, you can handle this in a more concise way with the Elvis operator `?:`:

```

// Assigns null to a nullable variable  

val b: String? = null

// Checks for nullability. If not null, returns length. If null, returns a non-null value

val l = b?.length ?: 0

println(l)

// 0
```



==If the expression to the left of `?:` is not `null`, the Elvis operator returns it. Otherwise, the Elvis operator returns the expression to the right.== The expression on the right-hand side is evaluated only if the left-hand side is `null`.

Since `throw` and `return` are expressions in [[Kotlin]], you can also use them on the right-hand side of the Elvis operator. This can be handy, for example, when checking [[Function]] [[Arguments]]:

```
fun foo(node: Node): String? {
    // Checks for getParent(). If not null, it's assigned to            parent. If null, returns null
    val parent = node.getParent() ?: return null
    // Checks for getName(). If not null, it's assigned to name. If     null, throws exception
    val name = node.getName() ?: throw                                  IllegalArgumentException("name expected")
    // ...
}
```