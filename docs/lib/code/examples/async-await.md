# Executing Async/Await Swift Code from Kotlin

## With Callback

!!! success ""
    This is the recommended way to call async Swift code from Kotlin.

Declare the method with a callback in `SwiftInterface.kt`:
```kotlin
interface FetchCallback {
    fun onResult(result: String)
}
external fun fetchAsyncDataWithCallback(callback: FetchCallback)
```

On the Swift side, implement it:
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_fetchAsyncDataWithCallback")
public func fetchAsyncDataWithCallback(env: UnsafeMutablePointer<JNIEnv?>, obj: jobject, callback: jobject) {
    let env = JEnv(env)
    defer { env.deleteLocalRef(callback) }
    guard let object = callback.box(env)?.object() else { return }
    Task {
        // Simulate async operation
        try? await Task.sleep(nanoseconds: 5_000_000_000) // 5 seconds
        // Call callback.onResult method
        object.callVoidMethod(name: "onResult", args: "Async data fetched successfully!")
    }
}
#endif
```
Call it from your Java/Kotlin app:
```kotlin
SwiftInterface.fetchAsyncDataWithCallback(object : SwiftInterface.FetchCallback {
    override fun onResult(result: String) {
        Log.i("ASYNC", "fetchAsyncDataWithCallback result: $result")
    }
})
```
Check LogCat:
```
 I  fetchAsyncDataWithCallback result: Async data fetched successfully!
```

## With Semaphore

!!! danger ""
    This way is not recommended.  
    Make sure to call this method off the main thread to avoid deadlocks.

Declare the method in `SwiftInterface.kt`:
```kotlin
external fun fetchAsyncData(): String
```
You need to know that the `@_cdecl` attribute doesn't work with the async operator. That's why we're using a `semaphore` here to execute our Swift code in a way that feels asynchronous. This approach is totally fine, but only for `non-UI` code. If you try this on the main thread, you'll face a complete and total deadlock, so just don't do it. I'll show you how to deal with UI in the next articles.
```swift
#if os(Android)
@_cdecl("Java_to_dev_myandroidlib_myfirstandroidproject_SwiftInterface_fetchAsyncData")
public func fetchAsyncData(env: UnsafeMutablePointer<JNIEnv>, obj: jobject) -> jstring? {
    // Create semaphore to wait for async task
    let semaphore = DispatchSemaphore(value: 0)
    // Create result variable
    final class AtomicResult: @unchecked Sendable {
        var value: String? = nil
    }
    let atomicResult = AtomicResult()
    // Start async task
    Task { @Sendable in
        // Simulate async operation
        try? await Task.sleep(nanoseconds: 5_000_000_000) // 5 seconds
        // Set result
        atomicResult.value = "Async data fetched successfully!"
        // Release semaphore
        semaphore.signal()
    }
    // Wait for async task to complete by blocking current thread
    semaphore.wait()
    // Check if result is available
    guard let result = atomicResult.value else { return nil }
    // Wrap Swift string into `JSString` and return its JNI reference
    return result.wrap().reference()
}
#endif
```
Call it from your Java/Kotlin app **(off the main thread!)**:
```kotlin
CoroutineScope(Dispatchers.IO).launch {
    Log.i("ASYNC", "Swift async call started")
    try {
        val result = SwiftInterface.fetchAsyncData()
        Log.i("ASYNC", "Swift returned: $result")
    } catch (e: Exception) {
        // Handle error
    }
    Log.i("ASYNC", "Swift async call finished")
}
```
Check LogCat:
```
 I  Swift async call started
 I  Swift returned: Async data fetched successfully!
 I  Swift async call finished
```