# Kotlin Library Project

## Source Code

Swift Stream automatically generates the initial Kotlin `Library` project for you. Its gradle files keeps in sync with your Swift code, however the rest is assumed to be maintained by you.

!!! tip ""
    During experiments you can always just remove `Library` folder and rebuild Swift project to regenerate it

Here’s a sample of the generated Kotlin interface:
```kotlin
import android.util.Log

object SwiftInterface {
    init {
        System.loadLibrary("MyFirstAndroidProject")
    }

    external fun initialize(caller: Any)

    external fun sendInt(number: Int)
    external fun sendIntArray(array: IntArray)
    external fun sendString(string: String)
    external fun sendDate(date: Date)
    external fun ping(): String
    external fun fetchAsyncData(): String
}
```

## Gradle Files

Swift Stream IDE automatically manages your Gradle project files. It generates Java packages based on your Swift targets from `Package.swift` and keeps all the Gradle files in sync.

In `Library/settings.gradle.kts`, it manages the list of included targets within special comment tags:
```kotlin
// managed by swiftstreamide: includes-begin
include(":myfirstandroidproject")
// managed by swiftstreamide: includes-end
```
It reflects the list of library products declared in your `Package.swift`.

In each `Library/<target>/build.gradle.kts` file, it manages dependencies based on the imports in your Swift code and the Swift version you're using:
```swift
// managed by swiftstreamide: so-dependencies-begin
implementation("com.github.swifdroid.runtime-libs:core:6.2.0")
implementation("com.github.swifdroid.runtime-libs:foundation:6.2.0")
implementation("com.github.swifdroid.runtime-libs:foundationessentials:6.2.0")
implementation("com.github.swifdroid.runtime-libs:i18n:6.2.0")
// managed by swiftstreamide: so-dependencies-end
```
It works this way because `soMode` setting is set to `Packed` by default. Read

If you need more control, you can take over manually, still without the hassle of manual file copying.

Read more about [→ so-mode](swift-project.md#so-modes) and how to customize it.

## Assemble with Gradle

To build the distributable Android library files (`.aar`), just hit `Java Library Project -> Assemble` in the Swift Stream sidebar.

![Assemble Debug](../assets/screenshots/android-lib-assemble-debug.webp "Asswemble Debug")

This command runs either `gradlew assembleDebug` or `gradlew assembleRelease` in the background, packaging everything up for distribution.

## Getting the .AAR File

Now, grab your freshly built library file! You'll find the `.aar` file in your Swift Stream project at this path:

```
📦 Project Root
├── 📄 Package.swift
└── 📂 Library
    └── 📂 myfirstandroidproject
        └── 📂 build
            └── 📂 outputs
                └── 📂 aar
                    └── 📄 myfirstandroidproject-debug.aar
```
Copy this file. Then, in your Android Studio project, navigate to your app module's directory (e.g., `app/`) and create a folder named `libs` right next to the `build.gradle.kts` file. Paste the `.aar` file into this new `libs` folder.