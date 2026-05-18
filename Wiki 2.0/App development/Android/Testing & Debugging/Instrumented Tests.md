Android has two types of tests that live in different source sets:

| | Unit Tests | Instrumented Tests |
|---|---|---|
| Location | `src/test/` | `src/androidTest/` |
| Runs on | Local JVM (your machine) | Device or emulator |
| Has Android context | No | Yes |
| Speed | Fast | Slower |

## When you need instrumented tests

Anything that requires the Android framework — Room databases, SharedPreferences, Context, Activities — must run as an instrumented test. You cannot fake these on the JVM.

Examples:
- Testing Room DAO queries
- Testing navigation
- UI tests with Espresso or Compose test APIs

## When unit tests are enough

Pure logic with no Android dependencies — ViewModel state changes, business logic, sorting algorithms — can run as unit tests on the JVM. These are faster and don't need a device.

## Running instrumented tests

From Android Studio: right-click the test class → **Run**

From terminal:
```bash
./gradlew connectedAndroidTest
```

Requires a connected device or running emulator.

## Annotations

```kotlin
@RunWith(AndroidJUnit4::class)  // required for instrumented tests
class MyTest {

    @Before fun setup() { ... }   // runs before each test
    @After fun teardown() { ... } // runs after each test
    @Test fun myTest() { ... }    // the actual test
}
```

## Related

- [[Test Strategies]]
- [[In-memory Room Database]]
- [[Fake Repository]]
