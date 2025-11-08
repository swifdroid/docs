# Swift Project

## Configuration

All the settings are stored in the `.vscode/android-stream.json` file.

### Package Name

Defines the namespace for your Java/Kotlin library project.

```json
"packageName": "com.mydomain.superlib"
```

### Compile SDK Version

Specifies the Android SDK version used to compile your project.  
It determines which APIs are available during compilation.

```json
"compileSDK": 35
```

### Minimum SDK Version

Defines the minimum Android SDK version required to run your library.  
It determines the range of devices that can use your library.

```json
"minSDK": 28
```

### Java Version

Sets the Java version used to compile your project.

```json
"javaVersion": 11
```

### Swift Compiler Arguments

You can pass additional arguments to the Swift compiler using the `swiftArgs` property in each scheme.  
This allows fine-grained control over the build process.

```json
"schemes": [
    {
        "title": "Debug",
        "swiftArgs": []
    }
]
```

### So Mode

This setting controls how the required `.so` (shared library) files are included in your Android library project.

There are three modes available, let's look at each in detail.

#### Packed

This is the default setting.

```json
"soMode": "Packed"
```

This mode analyzes your compiled `.so` target file to identify all linked libraries. It automatically determines which [→ runtime-libs](https://github.com/swifdroid/runtime-libs) modules provide the required `.so` dependencies, and then adds the corresponding entries to your `build.gradle.kts` file.

These dependencies are hosted in the SwifDroid [→ runtime-libs](https://github.com/swifdroid/runtime-libs) JitPack repository, which is maintained for each supported Swift version.

!!! note ""
    This mode keeps your library lightweight, as all required libraries are added as Gradle dependencies rather than copied into your project.

#### PickedAutomatically

```json
"soMode": "PickedAutomatically"
```

This mode performs the same dependency scan as `Packed`, but instead of adding Gradle dependencies,
it copies the required `.so` files directly into your project’s `Library/<target>/src/main/jniLibs/<architecture>/` folders.

!!! note ""
    Use this mode if you prefer to avoid adding external dependencies to your Gradle project.

#### PickedManually

This mode gives you full control over which .so files are included.  
You can explicitly specify the exact files needed for your library.

You can define them globally:
```json
"soMode": "PickedManually",
"soFiles": [
    "libBlocksRuntime.so",
    "$sdk/libswiftCore.so",
    "$ndk/libc++_shared.so",
    "$project/localLibs/$arch/libSomeThirdParty.so"
]
```
Or in each scheme separately.

Read about `$sdk`, `$ndk`, `$project` prefixes, and `$arch` variable in [→ fine tuning](#prefixes-and-variables) section.

You also can setup each scheme separately:

```swift
"soMode": "PickedManually",
"schemes": [
    {
        "title": "Debug",
        "soFiles": [
            "libBlocksRuntime.so",
            "$sdk/libswiftCore.so",
            "$ndk/libc++_shared.so",
            "$project/hoho/$arch/trololo.so"
        ]
    }
]
```

You can also **exclude** certain `.so` files from a specific scheme:

```swift
"soMode": "PickedManually",
"soFiles": [
    "libBlocksRuntime.so",
    "libswiftCore.so"
],
"schemes": [
    {
        "title": "Debug",
        "excludeSoFiles": [
            "libBlocksRuntime.so"
        ]
    },
    {
        "title": "Release",
        "excludeSoFiles": [
            "libswiftCore.so"
        ]
    }
]
```

In the example above `libBlocksRuntime.so` will be excluded from `Debug` scheme, while `libswiftCore.so` will be excluded from `Release` scheme (don’t actually do this, it’s just an example).

#### Local .so files

Sometimes you may need to include .so libraries from a third-party SDK that isn’t available via Maven.

To do so, place your `.so` files somewhere within your project, for example, in a `localLibs` folder at the same level as your `Package.swift`.

You must provide `.so` files for each architecture you intend to support:

```bash
📦 Project Root
├── 📄 Package.swift                 # Swift package configuration
└── 📂 localLibs
    ├── 📂 arm64-v8a
    │   └── 📄 libSomeThirdParty.so  # ARM 64-bit native library
    ├── 📂 armeabi-v7a
    │   └── 📄 libSomeThirdParty.so  # ARM 32-bit native library
    └── 📂 x86_64
        └── 📄 libSomeThirdParty.so  # x86_64 native library
```

Then reference them in the configuration file :
```json
"soFiles": [
    "$project/localLibs/$arch/libSomeThirdParty.so"
]
```

#### Prefixes and Variables

Here’s how `.so` file prefixes and variables work:

```json
"soFiles": [
    "libBlocksRuntime.so",
    "$sdk/libswiftCore.so",
    "$ndk/libc++_shared.so",
    "$project/localLibs/$arch/libSomeThirdParty.so",
    "/some/absolute/path/to/file.so"
]
```

**No prefix** (e.g. libBlocksRuntime.so)
Swift Stream IDE will search first in the SDK folder, then in the NDK folder.

**`$sdk`** — explicitly points to the Swift Android SDK folder only.

**`$ndk`** — explicitly points to the NDK folder only.

**`$project`** — resolves paths relative to the root of your Swift project (where `Package.swift` is located).

**`$arch`** — dynamically resolves to the current architecture (`arm64-v8a`, `armeabi-v7a`, `x86_64`).

Absolute paths are supported as well, just make sure they are valid within your Docker container environment (e.g. pointing to a shared volume).

## Building

Alright, time to build!

Switch to the `Swift Stream` tab on the left sidebar and hit `Project -> Build`.

![Swift Stream IDE Project Build](../assets/screenshots/android-lib-side-build-button.webp "Swift Stream IDE Project Build")

You'll be prompted to choose a **`Debug`** or **`Release`** scheme.
![Swift Stream IDE Choose Scheme](../assets/screenshots/android-lib-select-scheme.webp "Swift Stream IDE Choose Scheme")

Let's go with **`Debug`**. The building process will begin.

In Swift Stream, you can choose the `Log Level` to control how much detail you see:

- **Normal**
- **Detailed** (This is the default)
- **Verbose**
- **Unbearable** (For when you really need to see everything)

With the default **Detailed** level, you'll see an output similar to this during the build:
```bash
🏗️ Started building debug
💁‍♂️ it will try to build each phase
🔦 Resolving Swift dependencies for native
🔦 Resolved in 772ms
🔦 Resolving Swift dependencies for droid
🔦 Resolved in 2s918ms
🧱 Building `MyFirstAndroidProject` swift target for arm64-v8a
🧱 Built `MyFirstAndroidProject` swift target for `.droid` in 10s184ms
🧱 Building `MyFirstAndroidProject` swift target for armeabi-v7a
🧱 Built `MyFirstAndroidProject` swift target for `.droid` in 7s202ms
🧱 Building `MyFirstAndroidProject` swift target for x86_64
🧱 Built `MyFirstAndroidProject` swift target for `.droid` in 7s135ms
🧱 Preparing gradle wrapper
🧱 Prepared gradle wrapper in 1m50s
✅ Build Succeeded in 2m20s
```
As you can see, the initial Swift compilation itself was pretty fast, about **~30 seconds** total for all three architecture targets (arm64-v8a, armeabi-v7a, and x86_64). The bulk of the time (1m50s) was spent on the initial `gradle wrapper` setup, which is a one-time cost.

The great news is that **subsequent builds will be super fast**, taking only about 3 seconds for all three targets! This is because everything gets cached.

This build command also automatically generates the Java Library Gradle project for you. It's now ready to use in the `Library` folder.

Now continue to [→ Kotlin Library Project](kotlin-project.md)