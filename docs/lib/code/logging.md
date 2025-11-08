# Logging

You have two options: **`swift-log`** for full **LogCat** experience or classic **`print`**.

## Using `swift-log`

Add the following dependencies to your `Package.swift`:

```swift
.package(url: "https://github.com/swifdroid/AndroidLogging.git", from: "0.1.0"),
.package(url: "https://github.com/apple/swift-log.git", from: "1.6.2"),
// target
.product(name: "Logging", package: "swift-log"),
.product(name: "AndroidLogging", package: "AndroidLogging", condition: .when(platforms: [.android])),
```

Import the necessary modules at the top of your Swift file:

```swift
#if os(Android)
// Official Swift bindings for Android NDK
import Android
// Specific logging handler for Android platform
import AndroidLogging
#endif
// Lightweight and fast logging system
import Logging
```

Add the following code to your library's entry point (e.g., in the `initialize` function):

```swift
// Activate logger
LoggingSystem.bootstrap(AndroidLogHandler.taggedBySource)
```

This will let you use the `swift-log` API to log messages, which will be directed to LogCat.

```swift
let logger = Logger(label: "🐦‍🔥 SWIFT")
logger.info("🚀 Hello World!")
```

## Using `print`

Built-in `print` function doesn't work out of the box on Android.  
To use it, you have to adopt `PrintRedirector`. 

!!! info
    It was originally made by [Sergey Romanenko](https://github.com/purpln) [→ here](https://github.com/purpln/android-example/blob/main/Sources/Application/LogRedirector.swift)

It is the part of **`Droid`** package which is for Android GUI applications, but not a part of **`JNIKit`** library, so you have to copy this code into your library project manually.

Once you [→ copied it](https://github.com/swifdroid/droid/blob/master/Sources/Droid/PrintRedirector.swift) from the `Droid` package add the following code to your library's entry point (e.g., in the `initialize` function):

```swift
// Enable `print` redirect into LogCat
#if os(Android)
try? PrintRedirector.shared.redirectPrint()
#endif
```

And then feel free to use `print` anywhere in your code:

```swift
print("Hello from Swift for Android!")
```

!!! note
    It will always print to **LogCat** with the **`Swift`** tag and the **`INFO`** log level.