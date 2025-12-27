https://developer.android.com/codelabs/basic-android-kotlin-compose-kotlin-fundamentals-practice-problems?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-2-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-kotlin-fundamentals-practice-problems#0

- [[#**Mobile notifications**|**Mobile notifications**]]
- [[#**Movie-ticket price**|**Movie-ticket price**]]
- [[#**Temperature converter**|**Temperature converter**]]
- [[#**Song catalog**|**Song catalog**]]
- [[#**Internet profile**|**Internet profile**]]
- [[#**Foldable phones**|**Foldable phones**]]
- [[#**Special auction**|**Special auction**]]

## **Mobile notifications**

The solution uses an `if/else` statement to print the appropriate notification summary message based on the number of notification messages received:

```
fun main() {
    val morningNotification = 51
    val eveningNotification = 135
    
    printNotificationSummary(morningNotification)
    printNotificationSummary(eveningNotification)
}


fun printNotificationSummary(numberOfMessages: Int) {
    if (numberOfMessages < 100) {
        println("You have ${numberOfMessages} notifications.")
    } else {
        println("Your phone is blowing up! You have 99+ notifications.")
    }
}

```
## **Movie-ticket price**

The solution uses a `when` expression to return the appropriate ticket price based on the moviegoer's age. It also uses a simple `if/else` expression for one of the `when` expression's branches to add the additional condition for the standard ticket pricing.

The ticket price in the `else` branch returns a `-1` value, which indicates that the price set is invalid for the `else` branch. A better implementation is for the `else` branch to throw an exception. You learn about exception handling in future units.

```
fun main() {
    val child = 5
    val adult = 28
    val senior = 87
    
    val isMonday = true
    
    println("The movie ticket price for a person aged $child is \$${ticketPrice(child, isMonday)}.")
    println("The movie ticket price for a person aged $adult is \$${ticketPrice(adult, isMonday)}.")
    println("The movie ticket price for a person aged $senior is \$${ticketPrice(senior, isMonday)}.")
}
 
fun ticketPrice(age: Int, isMonday: Boolean): Int {
    return when(age) {
        in 0..12 -> 15
        in 13..60 -> if (isMonday) 25 else 30
        in 61..100 -> 20
        else -> -1
    }
}

```

## **Temperature converter**

```
fun main() {  
    printFinalTemperature(  
        27.00,  
        initialUnit = "Celsius",  
        finalUnit = "Fahrenheit",  
        conversionFormula = ({ x: Double -> ( x * 9.00/5.00 + 32.00).toDouble() })  
    )  
    printFinalTemperature(  
        350.00,  
        initialUnit = "Kelvin",  
        finalUnit = "Celsius",  
        conversionFormula = ({ x: Double -> ( x - 273.15).toDouble() })  
    )  
    printFinalTemperature(  
        10.00,  
        initialUnit = "Fahrenheit",  
        finalUnit = "Kelvin",  
        conversionFormula = ({ x: Double -> ( (5.00/9.00 * (x - 32.00)) + 273.15).toDouble() })  
    )  
}  
  
  
fun printFinalTemperature(  
    initialMeasurement: Double,  
    initialUnit: String,  
    finalUnit: String,  
    conversionFormula: (Double) -> Double  
) {  
    val finalMeasurement = String.format("%.2f", conversionFormula(initialMeasurement)) // two decimal places  
    println("$initialMeasurement degrees $initialUnit is $finalMeasurement degrees $finalUnit.")  
}
```

The solution requires you to pass a function as a parameter to the `printFinalTemperature()` function. The most succinct solution passes lambda expressions as the arguments, uses the `it` parameter reference in place of the parameter names, and makes use of trailing lambda syntax.

```
fun main() {    
        printFinalTemperature(27.0, "Celsius", "Fahrenheit") { 9.0 / 5.0 * it + 32 }
        printFinalTemperature(350.0, "Kelvin", "Celsius") { it - 273.15 }
        printFinalTemperature(10.0, "Fahrenheit", "Kelvin") { 5.0 / 9.0 * (it - 32) + 273.15 }
}


fun printFinalTemperature(
    initialMeasurement: Double, 
    initialUnit: String, 
    finalUnit: String, 
    conversionFormula: (Double) -> Double
) {
    val finalMeasurement = String.format("%.2f", conversionFormula(initialMeasurement)) // two decimal places
    println("$initialMeasurement degrees $initialUnit is $finalMeasurement degrees $finalUnit.")
}

```

## **Song catalog**

```
class Song {  
    val title : String = ""  
    val artist : String = ""  
    val year_published : Int = 0  
    var play_count : Int = 0  
    var popular : Boolean = if (play_count < 1000) { false } else { true }  
  
    fun printDetails() {  
        println("$title, performed by $artist, was released in $year_published.")  
    }  
}
```

The solution contains a `Song` class with a default constructor that accepts all required parameters. The `Song` class also has an `isPopular` property that uses a custom getter function, and a method that prints the description of itself. You can create an instance of the class in the `main()` function and call its methods to test whether the implementation is correct. You can use underscores when writing large numbers such as the `1_000_000` value to make it more readable.

```
fun main() {    
    val brunoSong = Song("We Don't Talk About Bruno", "Encanto Cast", 2022, 1_000_000)
    brunoSong.printDescription()
    println(brunoSong.isPopular)
}


class Song(
    val title: String, 
    val artist: String, 
    val yearPublished: Int, 
    val playCount: Int
){
    val isPopular: Boolean
        get() = playCount >= 1000

    fun printDescription() {
        println("$title, performed by $artist, was released in $yearPublished.")
    }   
}

```


## **Internet profile**

```
fun main() {  
    val amanda = Person("Amanda", 33, "play tennis", null)  
    val atiqah = Person("Atiqah", 28, "climb", amanda)  
  
    amanda.showProfile()  
    atiqah.showProfile()  
}  
  
  
class Person(val name: String, val age: Int, val hobby: String?, val referrer: Person?) {  
    fun showProfile() {  
        println("Name: $name")  
        println("Age: $age")  
        println("Likes to $hobby." +  
                if (referrer != null) " Has a referrer named ${referrer.name}, who likes to ${referrer.hobby}"  
                else "Doesn't have a referrer.")  
    }  
}
```

The solution contains null checks in various `if/else` statements to print different text based on whether various class properties are `null`:

```
fun main() {    
    val amanda = Person("Amanda", 33, "play tennis", null)
    val atiqah = Person("Atiqah", 28, "climb", amanda)
    
    amanda.showProfile()
    atiqah.showProfile()
}


class Person(val name: String, val age: Int, val hobby: String?, val referrer: Person?) {
    fun showProfile() {
        println("Name: $name")
        println("Age: $age")
        if(hobby != null) {
            print("Likes to $hobby. ")
        }
        if(referrer != null) {
            print("Has a referrer named ${referrer.name}")
            if(referrer.hobby != null) {
                print(", who likes to ${referrer.hobby}.")
            } else {
                print(".")
            }
        } else {
            print("Doesn't have a referrer.")
        }
        print("\n\n")
    }
}

```

## **Foldable phones**

```
fun main() {  
    var foldable = FoldablePhone(false)  
    foldable.foldPhone()  
    foldable.printFoldState()  
    foldable.foldPhone()  
    foldable.printFoldState()  
    foldable.checkPhoneScreenLight()  
    foldable.switchOn()  
    foldable.checkPhoneScreenLight()  
}  
  
  
open class Phone(var isScreenLightOn: Boolean = false){  
    open fun switchOn() {  
        isScreenLightOn = true  
    }  
  
    open fun switchOff() {  
        isScreenLightOn = false  
    }  
  
    open fun checkPhoneScreenLight() {  
        val phoneScreenLight = if (isScreenLightOn) "on" else "off"  
        println("The phone screen's light is $phoneScreenLight.")  
    }  
}  
  
class FoldablePhone(var isFolded: Boolean = false): Phone(isScreenLightOn = false){  
  
    override fun switchOn() {  
        if (!isFolded){  
            isScreenLightOn = true  
        }  
    }  
  
    fun foldPhone() {  
        if (!isFolded){  
            isFolded = true  
        } else {  
            isFolded = false  
        }  
    }  
  
    fun printFoldState() {  
        println("${if (isFolded) "Folded" else "Not Folded"}")  
    }  
}
```

For the `Phone` class to be a parent class, you need to make the class open by adding the `open` keyword before the class name. To override the `switchOn()` method in the `FoldablePhone` class, you need to make the method in the `Phone` class open by adding the `open` keyword before the method.

The solution contains a `FoldablePhone` class with a default constructor that contains a default argument for the `isFolded` parameter. The `FoldablePhone` class also has two methods to change the `isFolded` property to either a `true` or `false` value. It also overrides the `switchOn()` method inherited from the `Phone` class.

You can create an instance of the class in the `main()` function and call its methods to test if the implementation is correct.

```
open class Phone(var isScreenLightOn: Boolean = false){
    open fun switchOn() {
        isScreenLightOn = true
    }
    
    fun switchOff() {
        isScreenLightOn = false
    }
    
    fun checkPhoneScreenLight() {
        val phoneScreenLight = if (isScreenLightOn) "on" else "off"
        println("The phone screen's light is $phoneScreenLight.")
    }
}

class FoldablePhone(var isFolded: Boolean = true): Phone() {
    override fun switchOn() {
        if (!isFolded) {
            isScreenLightOn = true
        }
    }
    
    fun fold() {
        isFolded = true
    }
    
    fun unfold() {
        isFolded = false
    }
}

fun main() {    
    val newFoldablePhone = FoldablePhone()
    
    newFoldablePhone.switchOn()
    newFoldablePhone.checkPhoneScreenLight()
    newFoldablePhone.unfold()
    newFoldablePhone.switchOn()
    newFoldablePhone.checkPhoneScreenLight()
}

```


## **Special auction**

```
fun main() {  
    val winningBid = Bid(5000, "Private Collector")  
  
    println("Item A is sold at ${auctionPrice(winningBid, 2000)}.")  
    println("Item B is sold at ${auctionPrice(null, 3000)}.")  
}  
  
class Bid(val amount: Int, val bidder: String)  
  
fun auctionPrice(bid: Bid?, minimumPrice: Int): Int {  
    if (bid == null) {  
        return minimumPrice  
    } else {  
        if (bid.amount < minimumPrice) {  
            return minimumPrice  
        } else {  
            return bid.amount  
        }  
    }  
}
```

The solution uses the `?.` safe call operator and the `?:` Elvis operator to return the correct price:
```
fun main() {
    val winningBid = Bid(5000, "Private Collector")
    
    println("Item A is sold at ${auctionPrice(winningBid, 2000)}.")
    println("Item B is sold at ${auctionPrice(null, 3000)}.")
}

class Bid(val amount: Int, val bidder: String)

fun auctionPrice(bid: Bid?, minimumPrice: Int): Int {
    return bid?.amount ?: minimumPrice
}

```


Solutions
https://developer.android.com/codelabs/basic-android-kotlin-compose-kotlin-fundamentals-practice-problems?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-2-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-kotlin-fundamentals-practice-problems#8