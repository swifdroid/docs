# Casting

There are two situations where you need to cast an object from one class to another.

1. **When you want to treat an existing object as an instance of another class** (usually a parent) to call a method or access a field.

This is done using the `cast(to:)` method of `JObject`. It creates a proxy `JObject` of the target class but retains the same underlying JNI reference:
```swift
let editText: EditText
let textView = editText.cast(to: TextView.className)
```

2. **When you need to pass an object to a method that expects a different class** (also usually a parent).

This is done using the `signed(as:)` method of `JObject`:
```swift
let customView: CustomView
object.callVoidMethod(name: "setView", args: customView.signed(as: View.className))
```