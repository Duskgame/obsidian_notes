#jetpack_compose
Resources are the additional files and static content that your code uses, such as bitmaps, user-interface strings, animation instructions, and more. For more information about resources in [[Android]], see [App resources overview](https://developer.android.com/guide/topics/resources/providing-resources).

You should always separate app resources, such as images and strings, from your code so that you can maintain them independently. At runtime, Android uses the appropriate resource based on the current configuration. For example, you might want to provide a different [[User Interface]] layout based on the screen size or different strings based on the language setting.

### Grouping resources

You should always place each type of resource in a specific subdirectory of your project's `res/` directory. For example, here's the file hierarchy for a simple project:

```
MyProject/
	src/
		MyActivity.kt
	res/
		drawable/
			graphic.png        
		mipmap/
			icon.png
		values/
			strings.xml
```

As you can see in this example, the `res/` directory contains all the resources in subdirectories, which includes a `drawable/` directory for an image resource, a `mipmap/` directory for launcher icons, and a `values/` directory for string resources. To learn more about the usage, format, and syntax for app resources, see [Resource types overview](https://developer.android.com/guide/topics/resources/available-resources).

### Accessing resources

[[Jetpack Compose]] can access the resources defined in your Android project. Resources can be accessed with resource IDs that are generated in your project's `R` class.

An `R` class is an automatically generated class by Android that contains the IDs of all resources in the project. In most cases, the resource ID is the same as the filename. For example, the image in the previous file hierarchy can be accessed with this code:

```
R.drawable.graphic
```


- The **Resource Manager** tab in Android Studio helps you add and organize your images and other resources.
- An `Image` composable is a UI element that displays images in your app.
- An `Image` composable should have a content description to make your app more accessible.
- Text that's shown to the user, such as the birthday greeting, should be extracted into a string resource to make it easier to translate your app into other languages.

## Learn more

- [Manage your app's UI resources with Resource Manager](https://developer.android.com/studio/write/resource-manager)
- [Build more accessible apps](https://developer.android.com/guide/topics/ui/accessibility)
- [Support different languages and cultures](https://developer.android.com/training/basics/supporting-devices/languages)
- [Getting started with GitHub](https://help.github.com/en/github/getting-started-with-github)