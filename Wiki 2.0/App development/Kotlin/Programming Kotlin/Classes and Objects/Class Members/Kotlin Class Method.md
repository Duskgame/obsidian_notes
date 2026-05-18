---
aliases:
  - " "
  - Method
  - member function
---
Actions that the class can perform are defined as functions in the class.

For example, imagine that you own a smart device, a smart TV, or a smart light, which you can switch on and off with your mobile phone.

The syntax to define a function in a class is identical to what you learned before. The only difference is that the function is placed in the class body. When you define a _function_ in the class body, it's referred to as a member function or a _method_, and it represents the behavior of the class.

```
class SmartDevice {
    fun turnOn() {
        println("Smart device is turned on.")
    }    
    fun turnOff() {
        println("Smart device is turned off.")
    }
}
```


The call to a method in a class is similar to how you called other functions from the `main()` function. For example, if you need to call the `turnOff()` method from the `turnOn()` method, you can write something similar to this code snippet:

```
class SmartDevice {
    fun turnOn() {
        // A valid use case to call the turnOff() method 
        could be  to turn off the TV when available power doesn't           meet the requirement.        
        turnOff()
        ...
     }

    ...

}
```

To call a class method outside of the class, start with the class object followed by the `.` operator, the name of the function, and a set of parentheses. If applicable, the parentheses contain arguments required by the method. You can see the syntax in this diagram:

```
classObject.methodName([optional]Arguments)
```

```
fun main() {
    val smartTvDevice = SmartDevice()
    smartTvDevice.turnOn()
}
```

