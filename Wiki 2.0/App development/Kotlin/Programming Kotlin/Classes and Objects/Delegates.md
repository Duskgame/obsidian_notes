[[Kotlin Class Properties|properties]] in [[Kotlin]] use a [[Backing Field]] to hold their values in memory. You use the `field` identifier to reference it.

You can reuse the range-check code in the setter [[Function]] with _delegates_. Instead of using a field, and a [[Accessor|getter and setter]] function to manage the value, the delegate manages it.

The syntax to create property delegates starts with the declaration of a variable followed by the `by` [[Keywords and operators|keyword]], and the delegate [[Kotlin Object|Object]] that handles the getter and setter functions for the property.

```
var name by (delegate object)
```

Before you implement the [[Kotlin Class|Class]] to which you can delegate the implementation, you need to be familiar with _interfaces_. ==An [[Interface]] is a contract to which classes that implement it need to adhere.== It focuses on _what to do_ instead of _how to do_ the action. In short, an interface helps you achieve [[OOP|abstraction]].

```
interface name{
	body
}
```


You already learned how to _extend_ a class and _override_ its functionality. With interfaces, the class _implements_ the interface. The class provides implementation details for the methods and properties declared in the interface.

```
class SmartTvDevice(deviceName: String, deviceCategory: String) :
    SmartDevice(name = deviceName, category = deviceCategory) {

    override val deviceType = "Smart TV"

    private var speakerVolume by RangeRegulator(initialValue = 2, minValue = 0, maxValue = 100)

    private var channelNumber by RangeRegulator(initialValue = 1, minValue = 0, maxValue = 200)

    ...

}
```


```
class RangeRegulator(
    initialValue: Int,
    private val minValue: Int,
    private val maxValue: Int
) : ReadWriteProperty<Any?, Int> {

    var fieldData = initialValue

    override fun getValue(thisRef: Any?, property: KProperty<*>): Int {
        return fieldData
    }

    override fun setValue(thisRef: Any?, property: KProperty<*>, value: Int) {
        if (value in minValue..maxValue) {
            fieldData = value
        }
    }
}
```

- In Kotlin, delegation means handing over the get and set logic of a property to another object, using the `by` [[Keywords and operators|keyword]] and a class (the "delegate") that defines what should happen on get/set.
    
- the delegate is `RangeRegulator`. Every time you access or set `speakerVolume` or `channelNumber`, the logic in `RangeRegulator` runs instead of the default getter/setter.
    
- This pattern is called "delegated properties" and allows you to put reusable logic in one place, promoting code reuse and better separation of concerns.

