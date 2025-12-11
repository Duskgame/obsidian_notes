A vector, in the case of a drawable icon, is a series of instructions that draw an image when it is compiled. `mdpi`, `hdpi`, `xhdpi`, etc., are density qualifiers that you can append onto the name of a resource directory, like `mipmap,` to indicate that they are resources for devices of a certain screen density. Below is a list of [density qualifiers](https://developer.android.com/training/multiscreen/screendensities#TaskProvideAltBmp) on [[Android]]:

- `mdpi` - resources for medium-density screens (~160 dpi)
- `hdpi` - resources for high-density screens (~240 dpi)
- `xhdpi` - resources for extra-high-density screens (~320 dpi)
- `xxhdpi` - resources for extra-extra-high-density screens (~480 dpi)
- `xxxhdpi` - resources for extra-extra-extra-high-density screens (~640 dpi)
- `nodpi` - resources that are not meant to be scaled, regardless of the screen's pixel density
- `anydpi` - resources that scale to any density

**Note:** You may wonder why launcher icon assets are located in `mipmap` directories, separate from other app assets located in `drawable` directories. This is because some launchers may display your app icon at a larger size than what's provided by the device's default density bucket. For example, on an `hdpi` device, a certain device launcher may use the `xhdpi` version of the app icon instead. These directories hold the icons that account for devices that require icons with a density that is higher or lower than the default density.