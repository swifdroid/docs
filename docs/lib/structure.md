# Library Project Structure

At its heart, the project integrates both [Swift Package Manager (SPM)](https://swift.org/package-manager/) for Swift code and Gradle for Android library code.

```json
📦 Project Root
├── 📄 Package.swift              # Swift package configuration
├── 📂 Sources
│   └── 📂 <target_name>
│       └── 📄 Library.swift      # Core Swift library source
├── 📂 Library
│   ├── 📄 settings.gradle.kts    # Android project settings
│   ├── 📄 build.gradle.kts       # Root Android build configuration
│   └── 📂 <target_name>
│       ├── 📄 build.gradle.kts   # Android module build configuration
│       └── 📂 src
│           └── 📂 main
│               ├── 📂 java       # Java/Kotlin source files
│               └── 📂 jniLibs    # Native libraries for JNI
├── 📂 .devcontainer
│   └── 📄 devcontainer.json      # Development container configuration
├── 📂 .sourcekit-lsp
│   └── 📄 config.json            # SourceKit-LSP server configuration
└── 📂 .vscode
    └── 📄 android-stream.json    # VS Code Android development settings
```

## Package.swift

In the root of your project, you have the `Package.swift` file.  
This is where you define your Swift package, including its name, platforms, products, dependencies, and targets.

The key dependency is `JNIKit`.

Optional dependencies include `AndroidLogging` and `swift-log`, which provide logging support for your project.

## Sources

Your Swift source code files are located in the `Sources/<target_name>/` directory.  
This is where you implement the functionality of your Swift package.

By default target name is the same as your project name, but you can change it in `Package.swift`.

Your Swift code lives in `Sources/<target_name>/Library.swift` by default.

## Library

The Android library (a Gradle project) folder.

This folder will be automatically generated after your first Swift build. Alternatively, you can create it manually from the Swift Stream sidebar panel.

Its `settings.gradle.kts` and `build.gradle.kts` files are partially managed by Swift Stream IDE.

## .devcontainer

Contains configuration for your development container.

`devcontainer.json` contains Swift, Gradle, SDK, and NDK versions used within the container. You can modify it to fit your needs.

It is more convenient to change these settings via the Swift Stream IDE sidebar panel.

## .sourcekit-lsp

Contains configuration for SourceKit-LSP, which provides Swift language support in the IDE. It is necessary for the correct code completion, error highlighting, and other language features.

## .vscode

The most important file here is `android-stream.json`, which contains the Swift Stream IDE settings to build your project correctly.

More about it [→ here](swift-project.md#configuration)