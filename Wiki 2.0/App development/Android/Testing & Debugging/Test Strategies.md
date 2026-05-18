https://developer.android.com/codelabs/basic-android-kotlin-compose-test-viewmodel?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-4-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-test-viewmodel#3

A good test strategy revolves around covering different paths and boundaries of your code. At a very basic level, you can categorize the tests in three scenarios: success path, error path, and boundary case.

- **Success path:** The success path tests - also known as happy path tests, focus on testing the functionality for a positive flow. A positive flow is a flow that has no exception or error conditions. Compared to the error path and boundary case scenarios, it is easy to create an exhaustive list of scenarios for success paths, since they focus on intended behavior for your app.

An example of a success path in the Unscramble app is the correct update of the score, word count, and the scrambled word when the user enters a correct word and clicks the **Submit** button.

- **Error path:** The error path tests focus on testing the functionality for a negative flow–that is, to check how the app responds to error conditions or invalid user input. It is quite challenging to determine all the possible error flows because there are lots of possible outcomes when intended behavior is not achieved.

One piece of general advice is to list all the possible error paths, write tests for them, and keep your unit tests evolving as you discover different scenarios.

An example of an error path in the Unscramble app is the user enters an incorrect word and clicks on the **Submit** button, which causes an error message to appear and the score and word count to not update.

- **Boundary case:** A boundary case focuses on testing boundary conditions in the app. In the Unscramble app, a boundary is checking the UI state when the app loads and the UI state after the user plays a maximum number of words.

Creating test scenarios around these categories can serve as guidelines for your test plan.

## Create tests

A good unit test typically has following four properties:

- **Focused:** It should focus on testing a unit, such as a piece of code. This piece of code is often a class or a method. The test should be narrow and focus on validating the correctness of individual pieces of code, rather than multiple pieces of code at the same time.
- **Understandable:** It should be simple and easy to understand when you read the code. At a glance, a developer should be able to immediately understand the intention behind the test.
- **Deterministic:** It should consistently pass or fail. When you run the tests any number of times, without making any code changes, the test should yield the same result. The test shouldn't be flaky, with a failure in one instance and a pass in another instance despite no modification to the code.
- **Self-contained:** It does not require any human interaction or setup and runs in isolation.


- [Fundamentals of testing Android apps](https://developer.android.com/training/testing/fundamentals#benefits)
- [Using the Bill of Materials | Jetpack Compose | Android Developers](https://developer.android.com/jetpack/compose/bom/bom)