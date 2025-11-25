# Permissions

In Android permissions are declared in the `Manifest`, read more in [→ uses-permission](/app/manifest/#usespermission) section.

When all permissions are declared you can request them at runtime.

**`Activity`** has built-in support for requesting permissions:

## Async/Await

The most Swiftly way to request permissions is using async/await:

```swift
let results = await self.requestPermissions(.camera, .bluetooth)
for result in results {
    print("\(result.permission): \(result.granted)")
}
```

## Closure Callback

If async/await is not an option you can use closure callback:

```swift
self.requestPermissions(.camera, .bluetooth) { results in
    for result in results {
        print("\(result.permission): \(result.granted)")
    }
}
```

!!! note ""
    Random `requestCode` is generated internally to identify the request, but you can also provide your own.

## Classic Experience

This way is exactly like in native Android development: you check permission, request it, and handle the result in `onRequestPermissionsResult` method.

```swift
final class MyActivity: AppCompatActivity {
    override var body: any Body {
        Button("Request Camera")
            .onClick { [weak self] in
                guard let self else { return }
                if self.checkPermission(.camera) {
                    print("Permission already granted")
                } else {
                    self.requestPermissions(.camera, requestCode: 123)
                }
            }
    }

    override func onRequestPermissionsResult(
        requestCode: Int,
        results: [ActivityPermissionResult],
        deviceId: Int
    ) {
        super.onRequestPermissionsResult(
            requestCode: requestCode,
            results: results,
            deviceId: deviceId
        )
        Log.d("""onRequestPermissionsResult 
            requestCode: \(requestCode)
            deviceId: \(deviceId)
            results.count: \(results.count)
        """)
        for result in results {
            Log.i("Permission for \(result.permission): \(result.granted)")
        }
    }
}
```

!!! note ""
    When `checkPermission` called it also checks if the permission is declared in the `Manifest`. If not, a warning is logged.

    ```
    ⚠️ You forgot to add `.usesPermissions(.camera)` to your `DroidApp` manifest!
    ```

    Read more how to fix it in [→ uses-permission](/app/manifest/#usespermission) section.
