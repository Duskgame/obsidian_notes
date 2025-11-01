
A [[Kotlin]] program is required to have a _main function_, which is the specific place in your code where the program starts running. The main function is the entry point, or starting point, of the program.

```
fun main() {
    println("Hello, world!!!")
}
```


A _function_ is a segment of a program that performs a specific task. Your program may have one or more functions.

**Define versus call a function**

In your code, you _define_ a function first. That means you specify all the instructions needed to perform that task.

Once the function is defined, then you can _call_ that function, so the instructions within that function can be performed or executed.

### **Define a function**

These are the key parts needed to define a function:

- The function needs a **name**, so you can call it later.
- The function can also require some **inputs** ([[Parameter]]), or information that needs to be provided when the function is called. The function uses these inputs to accomplish its purpose. Requiring inputs is optional, and some functions do not require inputs.
- The function also has a **body** which contains the instructions to perform the task.
- May give a [[Returnvalue]]

```
fun (input) {
	body
}
```

The order of these elements matters. The `fun` word must come first, followed by the function name, followed by the inputs in parentheses, followed by curly braces around the function body.

[[Define and call custom functions]]

### **Function keyword**

To indicate that you're about to define a function in [[Kotlin]], use the special word `fun` (short for function) on a new line.

### **Function name**

Choose an appropriate name for your function based on the purpose of the function. The name is usually a verb or verb phrase.
Function names should follow the _camel case_ convention

### **Function inputs**

Notice that the function name is always followed by parentheses. These parentheses are where you list inputs for the function.

## Function Signature

So far you've seen how to define the function name, inputs (parameters), and outputs. The function name with its inputs (parameters) are collectively known as the _function signature_. The function signature consists of everything before the return type and is shown in the following code snippet.

```
fun birthdayGreeting(name: String, age: Int)
```
