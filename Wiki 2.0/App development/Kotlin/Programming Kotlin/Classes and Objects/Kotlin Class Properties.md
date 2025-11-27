---
aliases:
  - " "
  - propery
  - properties
---
While methods define the actions that a class can perform, the properties define the class's characteristics or data attributes. For example, a smart device has these properties:

- **Name.** Name of the device.
- **Category.** Type of smart device, such as entertainment, utility, or cooking.
- **Device status**. Whether the device is on, off, online, or offline. The device is considered online when it's connected to the internet. Otherwise, it's considered offline.

==Properties are basically variables that are defined in the class body instead of the function body. This means that the syntax to define properties and variables are identical.== You define an immutable property with the `val` keyword and a mutable property with the `var` keyword.

```
class SmartDevice {
    val name = "Android TV"
    val category = "Entertainment"
    var deviceStatus = "online"
    
    fun turnOn() {    
        println("Smart device is turned on.")        
    }    
    fun turnOff() {   
        println("Smart device is turned off.")        
    }
}
    
```


## Getter and setter functions in properties

