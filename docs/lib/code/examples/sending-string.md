# Sending a `String` from Kotlin to Swift

Declare the method in `SwiftInterface.kt`:
```kotlin
external fun sendString(string: String)
```
On the Swift side:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_sendString")
public func sendString(envPointer: UnsafeMutablePointer<JNIEnv?>, clazzRef: jobject, strRef: jobject) {
    // Create lightweight logger object
    let logger = Logger(label: "🐦‍🔥 SWIFT")
    // Access current environment
    let localEnv = JEnv(envPointer)
    // Defer block to clean up local references
    defer {
        // Release local ref to string object
        localEnv.deleteLocalRef(strRef)
    }
    // Wrap JNI string reference into `JString` and get Swift string
    logger.info("✍️ sendString 1")
    guard let string = strRef.wrap().string() else {
        logger.info("✍️ sendString 1.1 exit: unable to unwrap jstring")
        return
    }
    // Now you can use `string` as a regular Swift string
    logger.info("✍️ sendString 2: \(string)")
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
SwiftInterface.sendString("With love from Java")
```
Check LogCat:
```
 I  [🐦‍🔥 SWIFT] ✍️ sendString 1
 I  [🐦‍🔥 SWIFT] ✍️ sendString 2: With love from Java
```