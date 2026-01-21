The [Database Inspector](https://developer.android.com/studio/inspect/database) lets you inspect, query, and modify your app's databases while your app runs. This feature is especially useful for database debugging. The Database Inspector works with plain SQLite and with libraries built on top of SQLite, such as Room. Database Inspector works best on emulators/devices running API level 26.

**Note**: The Database Inspector only works with the SQLite library included in the Android operating system on API level 26 and higher. It doesn't work with other SQLite libraries that you bundle with your app.

1. Run your app on an emulator or connected device running API level 26 or higher, if you have not done so already.
2. In Android Studio, select **View** > **Tool Windows** > **App Inspection** from the menu bar.
3. Select the **Database Inspector** tab.
4. In the **Database Inspector** pane, select the `com.example.inventory` from the dropdown menu if it is not already selected. The **item_database** in the Inventory app appears in the **Databases** pane.

5. Expand the node for the **item_database** in the **Databases** pane and select **Item** to inspect. If your **Databases** pane is empty, use your emulator to add some items to the database using the **Add Item** screen.
6. Check the **Live updates** checkbox in the Database Inspector to automatically update the data it presents as you interact with your running app in the emulator or device.


[Database Inspector](https://www.youtube.com/watch?v=UMc7Tu0nKYQ)

