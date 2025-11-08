# Sending a `Date` Object from Kotlin to Swift

Declare the method in `SwiftInterface.kt`:
```kotlin
external fun sendDate(date: Date)
```
On the Swift side:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_sendDate")
public func sendDate(envPointer: UnsafeMutablePointer<JNIEnv?>, clazzRef: jobject, dateRef: jobject) {
    // Create lightweight logger object
    let logger = Logger(label: "🐦‍🔥 SWIFT")
    // Access current environment
    let localEnv = JEnv(envPointer)
    // Defer block to clean up local references
    defer {
        // Release local ref to date object
        localEnv.deleteLocalRef(dateRef)
    }
    // Wrap JNI date reference into `JObjectBox`
    logger.info("📅 sendDate 1")
    guard let box = dateRef.box(localEnv) else {
        logger.info("📅 sendDate 1.1 exit: unable to box Date object")
        return
    }
    // Initialize `JObject` from boxed global reference to the date
    logger.info("📅 sendDate 2")
    guard let dateObject = box.object() else {
        logger.info("📅 sendDate 2.1 exit: unable to unwrap Date object")
        return
    }
    // Call `getTime` method to get milliseconds since epoch
    logger.info("📅 sendDate 3")
    guard let milliseconds = dateObject.callLongMethod(name: "getTime") else {
        logger.info("📅 sendDate 3.1 exit: getTime returned nil, maybe wrong method")
        return
    }
    // Now you can use `milliseconds` as a regular Swift Int64 value
    logger.info("📅 sendDate 4: \(milliseconds)")
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
SwiftInterface.sendDate(Date())
```
Check LogCat:
```
 I  [🐦‍🔥 SWIFT] 📅 sendDate 1
 I  [🐦‍🔥 SWIFT] 📅 sendDate 2
 I  [🐦‍🔥 SWIFT] 📅 sendDate 3
 I  [🐦‍🔥 SWIFT] 📅 sendDate 4: 1757533833096
```