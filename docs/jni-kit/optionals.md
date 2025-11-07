# Optionals and NULL

You may have Java methods that accept nullable arguments. 
For any `JObject`, you can pass Swift `nil`, which will be converted to JNI `NULL`:
```swift
let object1: JObject
let object2: JObject? // this could be nil
let object3: JObject
object.callVoidMethod(name: "setView", args: object1, object2, object3)
```