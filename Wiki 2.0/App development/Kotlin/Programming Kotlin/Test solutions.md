
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

