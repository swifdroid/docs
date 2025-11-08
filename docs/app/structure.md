# Project Structure

At its heart, the project integrates both [Swift Package Manager (SPM)](https://swift.org/package-manager/) for Swift code and Gradle for Android application code.

## Sources

```json
📦 Project Root
├── 📄 Package.swift              # Swift package configuration
├── 📂 Sources
│   ├── 📂 App
│   |   ├── 📄 App.swift          # Entry point for the application
│   |   └── 📂 Activities
│   |       └── 📄 MainActivity.swift    # Main Android activity
│   └── 📂 AppManifest
│   |    └── 📄 Manifest.swift    # Proxy target for manifest declared in App
│   └── 📂 AppUI
│       └── 📄 AppUI.swift        # Proxy target which points to App
```

**App** target contains your main application code. Read more in [→ entry point](entry-point.md) section.

**AppManifest** target is a proxy target which you shouldn't modify, it is required to generate the AndroidManifest.xml file for your app.

**AppUI** target is a proxy target which also shouldn't be modified, it is required to generate the Android library module for your app.

## Application

```json
├── 📂 Application
│   ├── 📄 settings.gradle.kts    # App project settings
│   ├── 📄 build.gradle.kts       # Root app build configuration
│   └── 📂 androidapp
│       ├── 📄 build.gradle.kts   # Android app module build configuration
│       └── 📂 src
│           └── 📂 main
│               ├── 📄 AndroidManifest.xml  # Manifest Placeholder
│               └── 📂 res                  # Assets
```

Automatically generated Android application (a Gradle project) which could be opened in Android Studio. Files here are generated once and then managed by you.

## Library

```json
├── 📂 Library
│   ├── 📄 settings.gradle.kts    # Lib project settings
│   ├── 📄 build.gradle.kts       # Root lib build configuration
│   └── 📂 appui
│       ├── 📄 build.gradle.kts   # Android lib module build configuration
│       └── 📂 src
│           └── 📂 main
│               ├── 📄 AndroidManifest.xml  # Auto-Generated Manifest
│               ├── 📂 java                 # Auto-Generated Kotlin classes
│               └── 📂 jniLibs              # Native libraries for JNI
```

Automatically generated Android library (a Gradle project) which is imported into the Application project. All files here are managed by Swift Stream IDE.

## .devcontainer

```json
├── 📂 .devcontainer
│   └── 📄 devcontainer.json      # Development container configuration
```

Contains configuration for your development container.

`devcontainer.json` contains Swift, Gradle, SDK, and NDK versions used within the container. You can modify it to fit your needs.

It is more convenient to change these settings via the Swift Stream IDE sidebar panel.

## .sourcekit-lsp

```json
├── 📂 .sourcekit-lsp
│   └── 📄 config.json            # SourceKit-LSP server configuration
```

Contains configuration for SourceKit-LSP, which provides Swift language support in the IDE. It is necessary for the correct code completion, error highlighting, and other language features.

## .vscode

```json
└── 📂 .vscode
    └── 📄 android-stream.json    # VS Code Android development settings
```

Contains the Swift Stream IDE settings to build your project correctly.

You can adjust **`compileSDK`** and **`minSDK`** versions here.

!!! warning ""
    **`soMode`** setting is set to **`Packed`** and it should be left **unchanged**

Learn more about it in [→ library swift project](../lib/swift-project.md#configuration) guide.