A vector, in the case of a drawable icon, is a series of instructions that draw an image when it is compiled. `mdpi`, `hdpi`, `xhdpi`, etc., are density qualifiers that you can append onto the name of a resource directory, like `mipmap,` to indicate that they are resources for devices of a certain screen density. Below is a list of [density qualifiers](https://developer.android.com/training/multiscreen/screendensities#TaskProvideAltBmp) on [[Android]]:

- `mdpi` - resources for medium-density screens (~160 dpi)
- `hdpi` - resources for high-density screens (~240 dpi)
- `xhdpi` - resources for extra-high-density screens (~320 dpi)
- `xxhdpi` - resources for extra-extra-high-density screens (~480 dpi)
- `xxxhdpi` - resources for extra-extra-extra-high-density screens (~640 dpi)
- `nodpi` - resources that are not meant to be scaled, regardless of the screen's pixel density
- `anydpi` - resources that scale to any density

**Note:** You may wonder why launcher icon assets are located in `mipmap` directories, separate from other app assets located in `drawable` directories. This is because some launchers may display your app icon at a larger size than what's provided by the device's default density bucket. For example, on an `hdpi` device, a certain device launcher may use the `xhdpi` version of the app icon instead. These directories hold the icons that account for devices that require icons with a density that is higher or lower than the default density.

**Note**: To avoid a blurry app icon, be sure to provide different bitmap images of the icon for each density bucket (`mdpi`, `hdpi`, `xhdpi`, etc.). Note that device screen densities won't be precisely 160 dpi, 240 dpi, 320 dpi, etc. Based on the device's screen density, Android selects the resource at the closest larger density bucket and then scales it down.

As of the Android 8.0 release (API level 26), there is support for adaptive icons, which allows for more flexibility and interesting visual effects. For developers, that means that your app icon is made up of two layers: a foreground layer and a background layer.

**Note**: Adaptive icons were added in API level 26 of the platform, so they should be declared in the `mipmap` resource directory that has the `-v26` resource qualifier on it. That means the resources in this directory will only be applied on devices that are running API 26 (Android 8.0) or higher. The resource files in this directory are ignored on devices running version 25 or older in favor of the density bucketed mipmap directories.



**Note:** There are [tradeoffs](https://medium.com/androiddevelopers/understanding-androids-vector-image-format-vectordrawable-ab09e41d5c68) to using a vector drawable versus a bitmap image. For example, icons can be ideal as vector drawables because they are made up of simple shapes, while a photograph would be harder to describe as a series of shapes. It would be more efficient to use a bitmap asset in that case.

Because the edges of your icon could get clipped, depending on the shape of the mask from the device manufacturer, it's important to put the key information about your icon in the " [safe zone](https://medium.com/google-design/designing-adaptive-icons-515af294c783)." The safe zone is a circle of diameter 66 dpi in the center of the foreground layer. The content outside of the safe zone should not be essential, such as the background color, and okay if it gets clipped.