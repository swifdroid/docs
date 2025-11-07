# Values

All values that you pass into Java methods should conform to `JValuable` protocol.

## Primitive types

Swift primitive types like `Int32`, `Float`, `Double`, `Bool`, etc. conform to `JValuable` protocol out of the box and can be passed directly as method arguments.

```swift
let intValue: Int32 = 42
let floatValue: Float = 3.14
let doubleValue: Double = 2.718
let boolValue: Bool = true

object.callVoidMethod(
    name: "setValues",
    args: intValue, floatValue, doubleValue, boolValue
)
```
These values cannot be `nil` and are always represented in the JNI signature as primitives.

However, methods may expect object types like `java.lang.Integer` or `java.lang.Boolean`. In such cases, you need to use their Swift wrapper equivalents that are listed in [→ types](types.md#primitive-type-objects)

```swift
let nilDoubleObject: JDouble? = nil
let doubleObject: JDouble? = 9.41
let double: Double = 9.41
// Object Double is NULL
object.callVoidMethod(name: "sendDouble", args: nilDoubleObject)
// Object Double is NOT NULL: 9.41
object.callVoidMethod(name: "sendDouble", args: doubleObject)
// Primitive Double: 9.41
object.callVoidMethod(name: "sendDouble", args: double)
```

## Arrays

Arrays with primitive types like `[Int32]`, `[Float]`, `[Double]`, etc. also conform to `JValuable` protocol out of the box and can be passed directly as method arguments.

```swift
let boolArray = JBoolArray([true, false, true])
print("bool array: \(boolArray.toArray())")
```

<details>
    <summary>More 1D Array Examples</summary>

```swift
let byteArray = JByteArray([3, 4, 5])
print("byte array: \(byteArray.toArray())")

let charArray = JCharArray([3, 4, 5])
print("char array: \(charArray.toArray())")

let doubleArr = JDoubleArray([3.3, 4.4, 5.5])
print("double array: \(doubleArr.toArray())")

let floatArray = JFloatArray([3.3, 4.4, 5.5])
print("float array: \(floatArray.toArray())")

let longArray = JLongArray([3, 4, 5])
print("long array: \(longArray.toArray())")

let shortArray = JShortArray([3, 4, 5])
print("short array: \(shortArray.toArray())")
```
</details>

### 2D Arrays

Even 2D arrays with primitive types like `[[Int32]]`, `[[Float]]`, `[[Double]]`, etc. conform to `JValuable` protocol and can be passed directly as method arguments.

```swift
let boolArr2d = JBoolArray2D([[true, false, true], [false, true, false]])
print("2d bool array: \(boolArr2d.toArray())")
```

<details>
    <summary>More 2D Array Examples</summary>

```swift
let byteArr2d = JByteArray2D([[1, 2, 3], [4, 5, 6]])
print("2d byte array: \(byteArr2d.toArray())")

let charArr2d = JCharArray2D([[1, 2, 3], [4, 5, 6]])
print("2d char array: \(charArr2d.toArray())")

let doubleArr2d = JDoubleArray2D([[1.1, 2.2, 3.3], [4.4, 5.5, 6.6]])
print("2d double array: \(doubleArr2d.toArray())")

let floatArr2d = JFloatArray2D([[1.1, 2.2, 3.3], [4.4, 5.5, 6.6]])
print("2d float array: \(floatArr2d.toArray())")

let longArr2d = JLongArray2D([[1, 2, 3], [4, 5, 6]])
print("2d long array: \(longArr2d.toArray())")

let shortArr2d = JShortArray2D([[1, 2, 3], [4, 5, 6]])
print("2d short array: \(shortArr2d.toArray())")
```
</details>

## Strings

Swift `String` type also conforms to `JValuable` protocol out of the box and can be passed directly as method arguments.

```swift
let swiftString: String = "Hello from Swift"
object.callVoidMethod(name: "setString", args: swiftString)
```

If you need to explicitly sign it as `java.lang.CharSequence`, you can use:

```swift
let swiftString: String = "Hello from Swift"
object.callVoidMethod(
    name: "setCharSequence",
    args: swiftString.signedAsCharSequence()
)
```

## Objects

Java objects represented as `JObject` should be passed with `signed(as:)` method to specify their class signature.

```swift
let javaObject: JObject = ...
object.callVoidMethod(
    name: "setObject",
    args: javaObject.signed(as: "java.lang.Object")
)
```

More about that in [→ signature](signature.md)