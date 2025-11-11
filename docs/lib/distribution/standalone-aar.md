# Standalone AAR Integration

Once you have built your Android library `.aar` file, you can integrate it into your Android project using only the AAR file and the correct configuration.

!!! note ""
    This approach can be challenging for end users, as they must manually place the AAR file and add the required dependencies.

Open your existing Android project or create a new one in Android Studio.

Once your project is open, the first step is to add the JitPack repository. Navigate to your `settings.gradle.kts` file and ensure it includes the JitPack repository:
```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        maven { url = uri("https://jitpack.io") }
        mavenCentral()
    }
}
```
!!! warning ""
    You must add JitPack repository if you used `soMode` set to `Packed` [→ learn more](../swift-project.md#packed)

Next, add the dependencies to your app module's `build.gradle.kts` file (`app/build.gradle.kts`). Ensure you include both the `.aar` file and all the required runtime libraries:
```kotlin
dependencies {
    implementation(files("libs/myfirstandroidproject-debug.aar"))
    implementation("com.github.swifdroid.runtime-libs:core:6.2.0")
    implementation("com.github.swifdroid.runtime-libs:foundation:6.2.0")
    implementation("com.github.swifdroid.runtime-libs:i18n:6.2.0")
    // the rest of dependencies
}
```

!!! warning ""
    You must manually specify the `runtime-libs` dependencies when using `soMode` set to `Packed`, as Android cannot automatically detect them from within the `.aar` file.