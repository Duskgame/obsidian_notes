Icons are symbols that can help users understand a user interface by visually communicating the intended function. They often take inspiration from objects in the physical world that a user is expected to have experienced. Icon design often reduces the level of detail to the minimum required to be familiar to a user. For example, a pencil in the physical world is used for writing, so its icon counterpart usually indicates **create** or **edit**.

Material Design provides a [number of icons](https://material.io/resources/icons/?style=baseline), arranged in common categories, for most of your needs.

## Add Gradle dependency

Add the `material-icons-extended` library dependency to your project.

1. In the **Project** pane, open **Gradle Scripts > build.gradle.kts (Module :app)**.
2. Scroll to the end of the `build.gradle.kts (Module :app)` file. In the `dependencies{}` block, add the following line:

```
implementation("androidx.compose.material:material-icons-extended")
```

**Tip:** Whenever you modify the Gradle files, Android Studio may have to import or update libraries and run some background tasks. Android Studio displays a pop-up that asks you to sync your project. Click **Sync Now**.