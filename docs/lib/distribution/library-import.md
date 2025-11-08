# Library Import

If your end users have the project source code, or you simply want to test your library right away during development.

!!! note ""
    This method is intended for development and testing purposes.  
    For production use, consider distributing your library via JitPack or as a standalone AAR file.

Include your Library project directly into Android project, without needing to build an `.aar` file.

In the `settings.gradle.kts` of your Android project, at the very bottom:

```kotlin
include(":myandroidlibrary")
project(":myandroidlibrary").projectDir = file("../Library/myandroidlibrary")
```
!!! note ""
    Ensure the path points to the correct location of your Library project.
This will include your Library project as a module.

In your app module's `build.gradle.kts` file (`app/build.gradle.kts`), add a dependency on the library module:

```kotlin
dependencies {
    implementation(project(":myandroidlibrary"))
}
```

!!! success ""
    Sync Gradle project, and you can now use the library's classes and functions directly in your Android app code.