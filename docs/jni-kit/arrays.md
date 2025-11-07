# Arrays

## 1D Arrays

Swift array wrappers for Java types:

| Swift Type | JNI Type | Swift Array                |
|------------|----------|----------------------------|
| `[Int8]`       | `jbyte[]`    | `JInt8Array` or `JByteArray`   |
| `[Int16]`      | `jshort[]`   | `JInt16Array` or `JShortArray` |
| `[Int32]`      | `jint[]`     | `JInt32Array` or `JIntArray`   |
| `[Int64]`      | `jlong[]`    | `JInt64Array` or `JLongArray`  |
| `[UInt16]`     | `jchar[]`    | `JUInt16Array` or `JCharArray` |
| `[Bool]`       | `jboolean[]` | `JBoolArray`                 |
| `[Float]`      | `jfloat[]`   | `JFloatArray`                |
| `[Double]`     | `jdouble[]`  | `JDoubleArray`               |
| `[Any]`        | `jobject[]`  | `JObjectArray`               |

### Ways of initialization

With a Swift `Array`
```swift
JIntArray([1, 2, 3])
```

With a Swift `InlineArray`
```swift
let inlineArray: InlineArray<5, Int32> = [1, 2 ,3, 4, 5]
JIntArray(inlineArray)
```

Manually
```swift
let intArray = JIntArray(length: 3)!
#if os(Android)
var cArray: [Int32] = [1, 2, 3]
JEnv.current()?.setIntArrayRegion(intArray.object.ref.ref, start: 0, length: Int32(intArray.length), buffer: &cArray)
#endif
```

### Reading

#### Copy into a Swift `Array`
```swift
let intArray: JIntArray = ...
let swiftArray: [Int32] = intArray.toArray()
```
> ⚠️ Be careful with large arrays, copying them may consume a lot of memory.

### Iteration

Use this approach when working with large arrays:
```swift
let intArray: JIntArray = ...
for value in intArray {
    Log.d("value: \(value)")
}
```

## 2D Arrays

You can pass and receive not only `[Int]`, but also `[[Int]]`, `[[Float]]`, and other similar types (listed above).
This is a common pattern in Java and is often used in Android APIs, for instance, `ColorStateList` expects nested arrays for its state definitions.

The naming convention is the same, just add `2D` in the end

| Swift Type | JNI Type | Swift Array                |
|------------|----------|----------------------------|
| `[[Int8]]`       | `jbyte[][]`    | `JInt8Array2D` or `JByteArray2D`   |
| `[[Int16]]`      | `jshort[][]`   | `JInt16Array2D` or `JShortArray2D` |
| `[[Int32]]`      | `jint[][]`     | `JInt32Array2D` or `JIntArray2D`   |
| `[[Int64]]`      | `jlong[][]`    | `JInt64Array2D` or `JLongArray2D`  |
| `[[UInt16]]`     | `jchar[][]`    | `JUInt16Array2D` or `JCharArray2D` |
| `[[Bool]]`       | `jboolean[][]` | `JBoolArray2D`                 |
| `[[Float]]`      | `jfloat[][]`   | `JFloatArray2D`                |
| `[[Double]]`     | `jdouble[][]`  | `JDoubleArray2D`               |

### Initialization
Just as easy as with 1D arrays:
```swift
let intArray2d = JIntArray2D([[1, 2, 3], [4, 5, 6]])
```

### Reading
Equally straightforward:
```swift
let swiftArray: [[Int32]] = intArray2d.toArray()
```

### Iteration
Iteration is supported as well:
```swift
for array in intArray2d {
    Log.d("arrray: \(array)")
}
```

### Passing Arrays as Arguments

If a JNI method or constructor accepts a 1D or 2D array,  
you can simply pass it directly, no extra work needed:
```
object.callVoidMethod(name: "send1DArray", args: [1, 2, 3])
object.callVoidMethod(name: "send2DArray", args: [[1, 2, 3], [4, 5, 6]])
```