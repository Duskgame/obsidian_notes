When your Android app runs in the emulator and tries to reach `localhost` or `127.0.0.1`, it refers to the emulator's own loopback — not your computer's localhost. The emulator is a virtual machine with its own network stack.

## Reaching the host machine

The Android emulator reserves a special IP address for the host machine:

```
10.0.2.2
```

If your backend is running on your laptop at `localhost:8080`, the emulator must connect to `10.0.2.2:8080`:

```kotlin
const val BASE_URL = "http://10.0.2.2:8080"
```

This is only necessary for the emulator. On a physical device, you would use the actual local network IP of your machine (e.g. `192.168.x.x`), or a public URL in production.

## Cleartext HTTP

By default, Android 9+ blocks unencrypted HTTP traffic (only HTTPS is allowed). For local development with an HTTP server, add this to `AndroidManifest.xml`:

```xml
<application
    android:usesCleartextTraffic="true"
    ...>
```

Remove this flag for production builds — always use HTTPS in production.

## Related

- [[Internet Permissions]]
- [[Ktor Client]]
