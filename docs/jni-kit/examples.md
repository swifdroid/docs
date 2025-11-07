# Examples

## Wrapping Java class

A common use case is to wrap an existing Java class into a convenient Swift class.

The following example demonstrates this using `java/util/Date`:

<details>
    <summary>JDate.swift</summary>

```swift
// Example of Date object wrapper

/// A classic example of how to wrap a Java object into a Swift class.
/// 
/// Here we wrap `java.util.Date` object and provide some convenience methods.
public final class JDate: JObjectable, Sendable {
    /// The JNI class name
    public static let className: JClassName = "java/util/Date"

    /// JNI global reference object wrapper, it contains class metadata as well.
    public let object: JObject

    /// Initializer for when you already have a `JObject` reference.
    /// 
    /// This is useful when you receive a `Date` object from Java code.
    public init (_ object: JObject) {
        self.object = object
    }

    /// Allocates a `Date` object and initializes it so that it represents the time
    /// at which it was allocated, measured to the nearest millisecond.
    public init? () {
        #if os(Android)
        guard
            // Access current environment
            let env = JEnv.current(),
            // It finds the `java.util.Date` class and loads it directly or from the cache
            let clazz = JClass.load(Self.className),
            // Call to create a new instance of `java.util.Date` and get a global reference to it
            let global = clazz.newObject(env)
        else { return nil }
        // Store the object to access it from methods
        self.object = global
        #else
        // For non-Android platforms, return nil
        return nil
        #endif
    }

    /// Allocates a `Date` object and initializes it to represent the specified number of milliseconds since the standard base time known as "the epoch", namely January 1, 1970, 00:00:00 GMT.
    /// 
    /// - Parameter milliseconds: The number of milliseconds since January 1, 1970, 00:00:00 GMT.
    public init? (_ milliseconds: Int64) {
        #if os(Android)
        guard
            // Access current environment
            let env = JEnv.current(),
            // It finds the `java.util.Date` class and loads it directly or from the cache
            let clazz = JClass.load(Self.className),
            // Call to create a new instance of `java.util.Date`
            // with `milliseconds` parameter and get a global reference to it
            let global = clazz.newObject(env, args: milliseconds)
        else { return nil }
        // Store the object to access it from methods
        self.object = global
        #else
        // For non-Android platforms, return nil
        return nil
        #endif
    }

    /// Returns the day of the week represented by this date.
    public func day() -> Int32? {
        // Convenience call to `java.util.Date.getDay()`
        object.callIntMethod(name: "getDay")
    }

    /// Returns the hour represented by this Date object.
    public func hours() -> Int32? {
        // Convenience call to `java.util.Date.getHours()`
        object.callIntMethod(name: "getHours")
    }

    /// Returns the number of minutes past the hour represented by this date
    public func minutes() -> Int32? {
        // Convenience call to `java.util.Date.getMinutes()`
        object.callIntMethod(name: "getMinutes")
    }

    /// Returns the number of seconds past the minute represented by this date.
    public func seconds() -> Int32? {
        // Convenience call to `java.util.Date.getSeconds()`
        object.callIntMethod(name: "getSeconds")
    }

    /// Returns the number of milliseconds since January 1, 1970, 00:00:00 GMT for this date instance.
    public func time() -> Int32? {
        // Convenience call to `java.util.Date.getTime()`
        object.callIntMethod(name: "getTime")
    }

    /// Tests if this date is before the specified date.
    public func before(_ date: JDate) -> Bool {
        // Convenience call to `java.util.Date.before(Date date)`
        // which passes another `Date` object as a parameter
        // and returns a boolean result
        object.callBoolMethod(name: "before", args: date.object.signed(as: JDate.className)) ?? false
    }

    /// Tests if this date is after the specified date.
    public func after(_ date: JDate) -> Bool {
        // Convenience call to `java.util.Date.after(Date date)`
        // which passes another `Date` object as a parameter
        // and returns a boolean result
        object.callBoolMethod(name: "after", args: date.object.signed(as: JDate.className)) ?? false
    }

    /// Converts this java `Date` object to a Swift `Date`.
    public func date() -> Date? {
        // Get milliseconds since epoch using `getTime` method
        guard let time = time() else { return nil }
        // Convert milliseconds to seconds and create a Swift `Date` object
        return Date(timeIntervalSince1970: TimeInterval(time) / 1000.0)
    }
}
```
</details>