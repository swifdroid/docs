# JNIKit

It is a **foundation library** that provides best in class Swift layer for Java Native Interface with automatic memory management, convenient methods to work with Java objects, beautiful wrappers for Java types, and more.

> Yeah nah it's not [swift-java](https://github.com/swiftlang/swift-java), it's an alternative foundation provided by the Swift Stream Family.

## Installation

Open your `Package.swift` and add the following dependency:

```swift
// package dependencies
.package(url: "https://github.com/swifdroid/jni-kit.git", from: "2.10.0")

// target dependencies
.product(name: "JNIKit", package: "jni-kit")
```

Then head to the [→ basics](basics.md)