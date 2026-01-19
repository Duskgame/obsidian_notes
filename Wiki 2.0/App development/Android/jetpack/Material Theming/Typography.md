[[Jetpack Compose]]

## The Material Design type scale

A type scale is a selection of font styles that can be used across an app, ensuring a flexible, yet consistent, style. The [Material Design type scale](https://m3.material.io/styles/typography/type-scale-tokens) includes fifteen font styles that are supported by the type system. The naming and grouping have been simplified to: display, headline, title, body, and label, with large, medium, and small sizes for each. You only need to use these choices if you want to customize your app. If you don't know what to set for each type scale category, know that there is a default typography scale that you can use.

![[Pasted image 20251217133130.png]]

The type scale contains reusable categories of text, each with an intended application and meaning.

### **Display**

As the largest text on the screen, display styles are reserved for short, important text or numerals. They work best on large screens.

### **Headline**

Headlines are best-suited for short, high-emphasis text on smaller screens. These styles can be good for marking primary passages of text or important regions of content.

### **Title**

Titles are smaller than headline styles, and should be used for medium-emphasis text that remains relatively short.

### **Body**

Body styles are used for longer passages of text in your app.

### **Label**

Label styles are smaller, utilitarian styles, used for things like the text inside components or for very small text in the content body, such as captions.

### **Fonts**

The Android platform provides a variety of fonts, but you may want to customize your app with a font not provided by default. Custom fonts can add personality and be used for branding.


### Create a font Android Resource Directory.

Before you add fonts to your app, you will need to add a font directory.

1. In the project view of Android Studio, right-click on the **res** folder.
2. Select **New** > **Android Resource Directory**.
3. Name the Directory **font**, set the Resource type as **font**, and click **OK**.
4. Open your new font resource directory located at **res > font**.
5. Select fonts and drag them into the font resource directory in your project in Android Studio.
6. In your font folder, rename **Montserrat-Bold.ttf** to **montserrat_bold.ttf**


### Initialize fonts

1. In the project window, open **ui.theme** > **Type.kt**. Initialize the downloaded fonts below the import statements and above the `Typography` `val`. First, initialize **Abril Fatface** by setting it equal to `FontFamily` and passing in `Font` with the font file `abril_fatface_regular`.
2. Initialize **Montserrat**, underneath **Abril Fatface**, by setting it equal to `FontFamily` and passing in `Font` with the font file `montserrat_regular`. For `montserrat_bold`, also include `FontWeight.Bold`. Even though you do pass in the bold version of the font file, Compose doesn't know that the file is bold, so you need to explicitly link the file to `FontWeight.Bold`.

```
​​import androidx.compose.ui.text.font.Font
import androidx.compose.ui.text.font.FontFamily
import com.example.woof.R
import androidx.compose.ui.text.font.FontWeight

val AbrilFatface = FontFamily(
   Font(R.font.abril_fatface_regular)
)

val Montserrat = FontFamily(
   Font(R.font.montserrat_regular),
   Font(R.font.montserrat_bold, FontWeight.Bold)
)
```

Next, you set the different types of headlines to the fonts you just added. The `Typography` object has parameters for 13 different typefaces discussed above. You can define as many as you need.


For the displayLarge attribute, set it equal to TextStyle, and fill in the fontFamily, fontWeight, and fontSize with the information from the table above. This means that all the text set to displayLarge will have Abril Fatface as the font, with a normal font weight, and a fontSize of 36.sp.

Repeat this process for displayMedium, labelSmall, and bodyLarge.

import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.unit.sp


```
val Typography = Typography(
   displayLarge = TextStyle(
       fontFamily = AbrilFatface,
       fontWeight = FontWeight.Normal,
       fontSize = 36.sp
   ),
   displayMedium = TextStyle(
       fontFamily = Montserrat,
       fontWeight = FontWeight.Bold,
       fontSize = 20.sp
   ),
   labelSmall = TextStyle(
       fontFamily = Montserrat,
       fontWeight = FontWeight.Bold,
       fontSize = 14.sp
   ),
   bodyLarge = TextStyle(
       fontFamily = Montserrat,
       fontWeight = FontWeight.Normal,
       fontSize = 14.sp
   )
```