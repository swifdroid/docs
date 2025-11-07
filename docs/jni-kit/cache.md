# Cache

When working with JNI, it is important to minimize roundtrips to retrieve the same resources repeatedly. The `JNICache` implementation handles this by automatically storing `jclass` instances (by name), methodIDs, and fieldIDs for classes you have already used.

You don't need to do anything for this, it caches these things automatically under the hood.

And yes, it is thread-safe.

Beautiful, let's learn the [→ types](types.md).