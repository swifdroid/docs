# Class Loader and Cache

Here's a common gotcha: by default, when you try to find a Java class via JNI, it uses a system class loader. This system loader (surprise, surprise!) can't see dynamically loaded classes from your app, meaning it misses your own classes and any Gradle dependencies.

The solution? We need to get the application's class loader, which is available from any Java object via `.getClass().getClassLoader()`. The best practice is to grab this class loader once during initialization, create a global reference to it, store it in JNIKit's cache, and use it everywhere. It remains valid for the entire app lifecycle.

Here’s how to cache it inside your [→ entry point](entry-point.md) method:
```swift
// Access current environment
let localEnv = JEnv(envPointer)

// Convert caller's local ref into global ref
let callerBox = callerRef.box(localEnv)

// Defer block to clean up local references
defer {
    // Release local ref to caller object
    localEnv.deleteLocalRef(callerRef)
}

// Initialize `JObject` from boxed global reference to the caller object
guard let callerObject = callerBox?.object() else { return }

// Cache the class loader from the caller object
// it is important to load non-system classes later
// e.g. your own Java/Kotlin classes
if let classLoader = callerObject.getClassLoader(localEnv) {
    JNICache.shared.setClassLoader(classLoader)
    logger.info("🚀 class loader cached successfully")
}
```
!!! warning ""
    You should use `thizRef` instead of `callerRef` if your native method was an instance method.

The `getClassLoader` method retrieves the class loader associated with that object, and `JNICache.shared.setClassLoader` caches it for future use.

!!! success ""
    So later, when you will call `JClass.load(...)` it will use the right class loader from the cache.