[[OOP|Inheritance]] lets you build a [[Kotlin Class|Class]] upon the characteristics and behavior of another class. It's a powerful mechanism that helps you write reusable code and establish relationships between classes.

in [[Kotlin]], all the classes are final by default, which means that you can't extend them, so you have to define the relationships between them.

Define the relationship between the `SmartDevice` superclass and its subclasses:

1. In the `SmartDevice` superclass, add an `open` [[Keywords and operators|keyword]] before the `class` [[Keywords and operators|keyword]] to make it extendable:

```
open class SmartDevice(val name: String, val category: String) {    ...}
```

==The `open` keyword informs the compiler that this class is extendable, so now other classes can extend it.==

The syntax to create a subclass starts with the creation of the class header as you've done so far. The [[Kotlin Constructor|constructor]]'s closing parenthesis is followed by a space, a colon, another space, the superclass name, and a set of parentheses. If necessary, the parentheses include the parameters required by the superclass constructor. You can see the syntax in this diagram:

![[Pasted image 20251127154743.png]]

2. Create a `SmartTvDevice` subclass that extends the `SmartDevice` superclass:

```
class SmartTvDevice(deviceName: String, deviceCategory: String) :    SmartDevice(name = deviceName, category = deviceCategory) {}
```


```
class SmartTvDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    var speakerVolume = 2
        set(value) {
            if (value in 0..100) {
                field = value
            }
        }

    var channelNumber = 1
        set(value) {
            if (value in 0..200) {
                field = value
            }
        }
    
    fun increaseSpeakerVolume() {
        speakerVolume++
        println("Speaker volume increased to $speakerVolume.")
    }

    fun nextChannel() {
        channelNumber++
        println("Channel number increased to $channelNumber.")
    }
}
```



## Relationships between classes

When you use inheritance, you establish a relationship between two classes in something called an _IS-A relationship_. An [[Kotlin Object|Object]] is also an instance of the class from which it inherits. In a _HAS-A relationship_, an object can own an instance of another class without actually being an instance of that class itself. You can see a high-level representation of these relationships in this diagram:

![[Pasted image 20251127155408.png]]

### IS-A **relationships**

When you specify an IS-A relationship between the `SmartDevice` superclass and `SmartTvDevice` subclass, it means that whatever the `SmartDevice` superclass can do, the `SmartTvDevice` subclass can do. The relationship is unidirectional, so you can say that every smart TV _is a_ smart device, but you can't say that every smart device _is a_ smart TV. The code representation for an IS-A relationship is shown in this code snippet:

```
// Smart TV IS-A smart device.
class SmartTvDevice : SmartDevice() {
}
```

Don't use inheritance only to achieve code reusability. Before you decide, check whether the two classes are related to each other. If they exhibit some relationship, check whether they really qualify for the IS-A relationship. Ask yourself, "Can I say a subclass is a superclass?". For example, [[Android]] _is an_ operating system.

### HAS-A **relationships**

A HAS-A relationship is another way to specify the relationship between two classes. For example, you will probably use the smart TV in your home. In this case, there's a relationship between the smart TV and the home. The home contains a smart device or, in other words, the home _has a_ smart device. The _HAS-A_ relationship between two classes is also referred to as _composition_.

```
// The SmartHome class HAS-A smart TV device and smart light.
class SmartHome(
    val smartTvDevice: SmartTvDevice,
    val smartLightDevice: SmartLightDevice
) {

    ...

    fun turnOffAllDevices() {
        turnOffTv()
        turnOffLight()
    }
}

```



## Override superclass methods from subclasses

As discussed earlier, even though the turn-on and turn-off functionality is supported by all the smart devices, the way in which they perform the functionality differs. To provide this device-specific behavior, you need to override the `turnOn()` and `turnOff()` methods defined in the superclass. To override means to intercept the action, typically to take manual control. When you override a [[Kotlin Class Method|Method]], the method in the subclass interrupts the execution of the method defined in the superclass and provides its own execution.

1. In the body of the `SmartDevice` superclass before the `fun` [[Keywords and operators|keyword]] of each method, add an `open` keyword:

```
open class SmartDevice(val name: String, val category: String) {

    var deviceStatus = "online"

    open fun turnOn() {
        // function body
    }

    open fun turnOff() {
        // function body
    }
}
```


```
class SmartLightDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    var brightnessLevel = 0
        set(value) {
            if (value in 0..100) {
                field = value
            }
        }

    fun increaseBrightness() {
        brightnessLevel++
        println("Brightness increased to $brightnessLevel.")
    }

    override fun turnOn() {
        deviceStatus = "on"
        brightnessLevel = 2
        println("$name turned on. The brightness level is $brightnessLevel.")
    }

    override fun turnOff() {
        deviceStatus = "off"
        brightnessLevel = 0
        println("Smart Light turned off")
    }
}

```


```
class SmartTvDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    var speakerVolume = 2
        set(value) {
            if (value in 0..100) {
                field = value
            }
        }

    var channelNumber = 1
        set(value) {
            if (value in 0..200) {
                field = value
            }
        }

    fun increaseSpeakerVolume() {
        speakerVolume++
        println("Speaker volume increased to $speakerVolume.")
    }

    fun nextChannel() {
        channelNumber++
        println("Channel number increased to $channelNumber.")
    }

    override fun turnOn() {
        deviceStatus = "on"
        println(
            "$name is turned on. Speaker volume is set to $speakerVolume and channel number is " +
                "set to $channelNumber."
        )
    }

    override fun turnOff() {
        deviceStatus = "off"
        println("$name turned off")
    }
}

```


```
fun main() {
    var smartDevice: SmartDevice = SmartTvDevice("Android TV",          "Entertainment")
    smartDevice.turnOn()
    
    smartDevice = SmartLightDevice("Google Light", "Utility")
    smartDevice.turnOn()
}

```

```
Android TV is turned on. Speaker volume is set to 2 and channel number is set to 1.
Google Light turned on. The brightness level is 2.
```


This is an example of [[OOP|polymorphism]]. The code calls the `turnOn()` method on a variable of `SmartDevice` type and, depending on what the actual value of the variable is, different implementations of the `turnOn()` method can be executed.


### Reuse superclass code in subclasses with the `super` **keyword**

When you take a close look at the `turnOn()`and `turnOff()` methods, you notice that there's similarity in how the `deviceStatus` variable is updated whenever the methods are called in the `SmartTvDevice` and `SmartLightDevice` subclasses: the code duplicates. You can reuse the code when you update the status in the `SmartDevice` class.

To call the overridden method in the superclass from the subclass, you need to use the `super` [[Keywords and operators|keyword]]. Calling a method from the superclass is similar to calling the method from outside the class. Instead of using a `.` [[Keywords and operators|operator]] between the object and method, you need to use the `super` keyword, which informs the Kotlin compiler to call the method on the superclass instead of the subclass.

The syntax to call the method from the superclass starts with a `super` keyword followed by the `.` [[Keywords and operators|operator]], [[Function]] name, and a set of parentheses. If applicable, the parentheses include the [[Arguments]].

```
super.functionName([optional]Arguments)
```

```
open class SmartDevice(val name: String, val category: String) {

    var deviceStatus = "online"

    open fun turnOn() {
        deviceStatus = "on"
    }

    open fun turnOff() {
        deviceStatus = "off"
    }
}

```

```
class SmartTvDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    var speakerVolume = 2
        set(value) {
            if (value in 0..100) {
                field = value
            }
        }

     var channelNumber = 1
        set(value) {
            if (value in 0..200) {
                field = value
            }
        }

    fun increaseSpeakerVolume() {
        speakerVolume++
        println("Speaker volume increased to $speakerVolume.")
    }

    fun nextChannel() {
        channelNumber++
        println("Channel number increased to $channelNumber.")
    }

    override fun turnOn() {
        super.turnOn()
        println(
            "$name is turned on. Speaker volume is set to $speakerVolume and channel number is " +
                "set to $channelNumber."
        )
    }

    override fun turnOff() {
        super.turnOff()
        println("$name turned off")
    }
}

```


## Override superclass properties from subclasses

Similar to methods, you can also override [[Kotlin Class Properties|properties]] with the same steps.

```
open class SmartDevice(val name: String, val category: String) {

    var deviceStatus = "online"

    open val deviceType = "unknown"
    ...
}

```

```
class SmartTvDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    override val deviceType = "Smart TV"

    ...
}

```

```
class SmartLightDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    override val deviceType = "Smart Light"

    ...

}
```

