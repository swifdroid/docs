# Sending an `Int` from Kotlin to Swift

Declare a method in `SwiftInterface.kt`:
```kotlin
external fun sendInt(number: Int)
```
On the Swift side, implement it:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_sendInt")
public func sendInt(
    envPointer: UnsafeMutablePointer<JNIEnv?>,
    clazzRef: jobject,
    number: jint
) {
    let logger = Logger(label: "🐦‍🔥 SWIFT")
    logger.info("#️⃣ sendInt: \(number)")
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
SwiftInterface.sendInt(123)
```
Check LogCat:
```
 I  [🐦‍🔥 SWIFT] #️⃣ sendInt: 123
```