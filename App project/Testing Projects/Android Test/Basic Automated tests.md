https://developer.android.com/codelabs/basic-android-kotlin-compose-write-automated-tests?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-2-pathway-3%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-write-automated-tests#3
## **Type of automated tests**

### **Local tests**

Local tests are a type of automated test that directly test a small piece of code to ensure that it functions properly. With local tests, you can test functions, classes, and properties. Local tests are executed on your workstation, which means they run in a development environment without the need for a device or emulator. This is a fancy way to say that local tests run on your computer. They also have very low overhead for computer resources, so they can run fast even with limited resources. Android Studio comes ready to run local tests automatically.

### **Instrumentation tests**

For Android development, an instrumentation test is a UI test. Instrumentation tests let you test parts of an app that depend on the Android API, and its platform APIs and services.

Unlike local tests, UI tests launch an app or part of an app, simulate user interactions, and check whether the app reacted appropriately. Throughout this course, UI tests are run on a physical device or emulator.

When you run an instrumentation test on Android, the test code is actually built into its own Android Application Package (APK) like a regular Android app. An APK is a compressed file that contains all the code and necessary files to run the app on a device or emulator. The test APK is installed on the device or emulator along with the regular app APK. The test APK then runs its tests against the app APK.



```
import com.example.tiptime.calculateTip  
import org.junit.Assert.assertEquals  
import org.junit.Test  
import java.text.NumberFormat  
  
class TipCalculatorTests {  
  
    @Test  
    fun calculateTip_20PercentNoRoundup() {  
        val amount = 10.00  
        val tipPercent = 20.00  
        val expectedTip = NumberFormat.getCurrencyInstance().format(2)  
        val actualTip = calculateTip(amount = amount, tipPercent = tipPercent, false)  
        assertEquals(expectedTip, actualTip)  
    }  
}
```