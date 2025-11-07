# Types

## Primitive types

JNI primitive types, how they represented in Swift:

| JNI      | Swift  |
|----------|--------|
| jbyte    | Int8   |
| jshort   | Int16  |
| jint     | Int32  |
| jlong    | Int64  |
| jchar    | UInt16 |
| jboolean | Bool   |
| jfloat   | Float  |
| jdouble  | Double |

Pure primitive types can be passed as method arguments directly:
```swift
let intValue: Int32 = 9.41
let floatValue: Float = 9.41
let doubleValue: Double = 9.41
object.callVoidMethod(name: "setValues", args: intValue, floatValue, doubleValue)
```

These values cannot be `nil` and are always represented in the JNI signature as primitives.

## Primitive type objects

However, if the Java side expects full object types like `java.lang.Integer` or `java.lang.Long`, you need to use their Swift wrapper equivalents listed below:

| Swift Type | Swift Wrapper               | Java Type           |
|------------|-----------------------------|---------------------|
| Int8       | JInt8 (JByte)               | java/lang/Byte      |
| Int16      | JInt16 (JShort)             | java/lang/Short     |
| Int32      | JInt32 (JInt, JInteger)     | java/lang/Integer   |
| Int64      | JInt64 (JLong)              | java/lang/Long      |
| Bool       | JBool                       | java/lang/Boolean   |
| Float      | JFloat                      | java/lang/Float     |
| Double     | JDouble                     | java/lang/Double    |
| UInt16     | JUInt16 (JChar, JCharacter) | java/lang/Character |

These values can be `nil`.

**Usage Example:**
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
**Corresponding Kotlin Code**:
```kotlin
fun sendDouble(value: Double) {
    Log.d("CHECK", "Primitive Double: $value")
}
fun sendDouble(value: Double?) {
    if (value != null) {
        Log.d("CHECK", "Object Double is NOT NULL: $value")
    } else {
        Log.d("CHECK", "Object Double is NULL")
    }
}
```

## Non-primitive type objects

This includes all other Java object types like `String`, custom classes, collections, etc. They are represented in Swift as `JObject` or their specific subclasses (e.g., `JString` for `java.lang.String`).

These values can also be `nil`.

To know more please head to [→ Objects](objects.md), [→ Classes](classes.md), and [→ Signature](signature.md)