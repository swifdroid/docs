# Receiving a String from Swift in Kotlin

Declare a method in `SwiftInterface.kt` that returns a value:
```kotlin
external fun ping(): String
```
On the Swift side, return a string:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_ping")
public func ping(envPointer: UnsafeMutablePointer<JNIEnv?>, clazzRef: jobject) -> jobject? {
    // Wrap Swift string into `JSString` and return its JNI reference
    return "🏓 Pong from Swift!".wrap().reference()
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
Log.i("HELLO", "Pinging: ${SwiftInterface.ping()}")
```
Check LogCat:
```
 I  Pinging: 🏓 Pong from Swift!
```