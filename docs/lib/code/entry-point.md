# Entry Point

Everything starts with an `initialize` method. This method is exposed to the Java/Kotlin side and must be called before any other native methods.

## On the Swift Side

The following code shows how to use `@_cdecl` to expose this method for JNI.

The `@_cdecl` naming convention is critical, as it follows the JNI pattern:
```swift
Java_<package>_<class>_<method>
```
- `package` is the fully qualified package name with underscores instead of dots
- `class` is the class name
- `method` is the method name

The method's arguments also follow JNI convention. The first two are required and are passed automatically by the JNI:

1. `envPointer`: This never changes. It's a pointer to the JNI environment, your main interface for interacting with the JVM.
2. `clazzRef` or `thizRef`: You get `clazzRef` if the Java method is static (like in our case, where the method is inside a Kotlin `object`). You get `thizRef` if it's an instance method. The first is a pointer to a class; the second is a pointer to an instance.

Any arguments after these represent the parameters of the Java/Kotlin method itself. In our case, the method has one extra argument: a `caller` object.

We pass this from the app to provide Swift with the necessary context, such as the class loader or application context.

This `caller` instance is necessary to cache the app's class loader, which is required for JNI interop to dynamically resolve and load Java classes from Swift code.

!!! note
    If we had `thizRef` instead of `clazzRef`, we might not need to pass this extra `caller` object.

```swift
#if os(Android)
// Official Swift bindings for Android NDK
import Android
// JNI Kit with a lot of conveniences
import JNIKit

@_cdecl("Java_com_mydomain_superlib_myfirstandroidproject_SwiftInterface_initialize")
public func initialize(
    envPointer: UnsafeMutablePointer<JNIEnv?>,
    clazzRef: jobject,
    callerRef: jobject
) {
    // ALSO: Activate Android logger
    // Initialize JVM
    let jvm = envPointer.jvm()
    JNIKit.shared.initialize(with: jvm)
    // ALSO: Activate class loader cache
}
#endif
```

Learn more about [→ logging](logging.md) and [→ class loader](class-loader.md).

The method body shows we first bootstrap the Swift logging system with the Android logger (this only needs to be done once).

After that, we can use the logger anywhere, simply like this:
```swift
let logger = Logger(label: "🐦‍🔥 SWIFT")
logger.info("🚀 Hello World!")
```

Then, we initialize the connection to the JVM.

## On the Other Side

#### Java
```java
public final class SwiftInterface {
    static {
        System.loadLibrary("MyFirstAndroidProject");
    }
    private SwiftInterface() {}
    public static native void initialize(Object caller);
}
```

#### Kotlin
```kotlin
object SwiftInterface {
    init {
        System.loadLibrary("MyFirstAndroidProject")
    }
    external fun initialize(caller: Any)
}
```

Now, from your Android app, just call:
```kotlin
SwiftInterface.initialize(this)
```
where `this` is an instance of an Activity, Fragment, or Application class.

And we're done! Your Swift library is now initialized and ready to use.