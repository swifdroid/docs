# Sending an `IntArray` from Kotlin to Swift

Declare the method in `SwiftInterface.kt`:
```kotlin
external fun sendIntArray(array: IntArray)
```
On the Swift side, handle the array:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_sendIntArray")
public func sendIntArray(
    envPointer: UnsafeMutablePointer<JNIEnv?>,
    clazzRef: jobject,
    arrayRef: jintArray
) {
    // Create lightweight logger object
    let logger = Logger(label: "🐦‍🔥 SWIFT")
    // Access current environment
    let localEnv = JEnv(envPointer)
    // Defer block to clean up local references
    defer {
        // Release local ref to array object
        localEnv.deleteLocalRef(arrayRef)
    }
    // Get array length
    logger.info("🔢 sendIntArray 1")
    let length = localEnv.getArrayLength(arrayRef)
    logger.info("🔢 sendIntArray 2 length: \(length)")
    // Get array elements
    var swiftArray = [Int32](repeating: 0, count: Int(length))
    localEnv.getIntArrayRegion(arrayRef, start: 0, length: length, buffer: &swiftArray)
    // Now you can use `swiftArray` as a regular Swift array
    logger.info("🔢 sendIntArray 3 swiftArray: \(swiftArray)")
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
SwiftInterface.sendIntArray(intArrayOf(7, 6, 5))
```
Check LogCat:
```
 I  [🐦‍🔥 SWIFT] 🔢 sendIntArray: 1
 I  [🐦‍🔥 SWIFT] 🔢 sendIntArray: 2 length: 3
 I  [🐦‍🔥 SWIFT] 🔢 sendIntArray: 3 swiftArray: [7, 6, 5]
```