# Application Development

You are in the most incredible place to start building Android apps in Swift!

This code is absolute reality now:
```swift
ConstraintLayout {
    VStack {
        TextView("Hello from Swift!")
            .width(.matchParent)
            .height(.wrapContent)
            .textColor(.green)
            .marginBottom(16)
        MaterialButton("Tap Me", style: .tonal)
            .onClick {
                print("Button tapped!")
            }
    }
    .centerVertical()
    .leftToParent()
    .rightToParent()
}
```
!!! success ""
    **You can create stunning user interfaces natively in Swift!**

**Droid framework** is the foundation for building rich Android apps with native UI and UX.

It provides an extensive set of components, including AndroidX, Flexbox, and Material Design.

Offering a **SwiftUI**-like declarative syntax for everything, **Droid framework** simplifies the process of developing Android applications in Swift by providing a high-level API that abstracts away many complexities of the Android platform and completely hides the underlying JNI layer.