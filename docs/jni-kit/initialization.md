# Initialization

Required imports
```swift
import JNIKit
import Android
```

Your first task is to initialize the JVM connection.

For that, you can declare a JNI initialization method like this:

```swift
@_cdecl("Java_com_mylib_mypackage_MyClass_initialize")
public func initialize(
    // pointer to the JNI environment
    // which is used to interact with the JVM
    envPointer: UnsafeMutablePointer<JNIEnv?>,
    // reference to the Java class
    // or could be `thizRef` for the call from the instance
    clazzRef: jobject,
    // optional, but recommended if you don't have a `thizRef`
    callerRef: jobject
) {
    // Initialize JVM
    let jvm = envPointer.jvm()
    JNIKit.shared.initialize(with: jvm)
}
```
At this point, your JNI connection is ready to use from the Swift side.

But I also highly recommend caching the class loader instance by taking it from the `thizRef` or `callerRef` objects:
```swift
// Access current environment
let localEnv = JEnv(envPointer)

// Convert caller's local ref into global ref
let callerBox = callerRef.box(localEnv)

// Defer block to clean up local references
defer {
    // Releases local ref to caller object
    localEnv.deleteLocalRef(callerRef)
}

// Initialize `JObject` from boxed global reference to the caller object
guard let callerObject = callerBox?.object() else { return }

// Cache the class loader from the caller object
// It is important for loading non-system classes later
// e.g. your own Java classes
if let classLoader = callerObject.getClassLoader(localEnv) {
    JNICache.shared.setClassLoader(classLoader)
    logger.info("🚀 class loader cached successfully")
}
```

**Why do you need the class loader instance?**

Because without it, JNIKit will use the system-wide class loader, which can't load dynamically added classes like those from your app or your app's Gradle dependencies.  
So, it is a really important knowledge.

Now, check out what is [→ cache](cache.md) for.