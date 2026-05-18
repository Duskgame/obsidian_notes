The purpose of permissions on [[Android]] is to protect the privacy of an Android user. Android apps must declare or request permissions to access sensitive user data, such as contacts, call logs, and certain system features, such as camera or internet.

In order for your app to access the Internet, it needs the `INTERNET` permission. Connecting to the internet introduces security concerns, which is why apps do not have internet connectivity by default. You need to explicitly declare that the app needs access to the internet. This declaration is considered a normal permission. To learn more about Android permissions and its types, please refer to the [Permissions on Android](https://developer.android.com/guide/topics/permissions/overview).

your app declares the permission(s) it requires by including `<uses-permission>` tags in the `AndroidManifest.xml` file.

```
<uses-permission android:name="android.permission.INTERNET" />
```

