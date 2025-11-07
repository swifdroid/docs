# Environment

Environment is the real bridge for any JNI call which in JNIKit's implementation is hidden under the hood but can also be used directly if needed.

The environment pointer is wrapped into convenient `JEnv` object.

Retrieve it this way:
```swift
let env = JEnv.current()
```

!!! warning
    Each instance works only in the thread where it was retrieved, so call **`JEnv.current()`** again if you switched thread or moved into `Task {}`

The possibilities of `JEnv` are a huge topic to describe in detail. I would say if you know what you want from it you will get it.

All its methods are ergonomically wrapped for easy use with Swift types and convenience types like `JClass`, `JClassName`, `JObject`, `JMethodId`, and `JFieldId`.

## Version

```swift
/// Returns the version of the JNI environment provided by the JVM
///
/// This is typically used to verify compatibility
/// (e.g. `0x00010008` = JNI 1.8)
getVersion() -> Int32

/// Returns the JNI version as a human-readable string (e.g. `"1.8"`)
getVersionString() -> String

/// Returns the JNI version parsed into a `JNIVersion` struct
getVersionStruct() -> JNIVersion // { major: 1, minor: 8 }
```

## Class Definition

```swift
/// Defines a new Java class dynamically from raw bytecode
///
/// - Parameters:
///   - name: The fully qualified class name (e.g., `"com/example/MyClass"`)
///   - loader: A class loader object (or `nil` for bootstrap loader)
///   - buf: Pointer to the `.class` bytecode
///   - size: Size of the bytecode buffer
/// - Returns: A newly defined Java class, or `nil` if failed
defineClass(
    name: JClassName,
    loader: JObject?,
    buf: UnsafePointer<jbyte>,
    size: jint
) -> JClass?

/// Finds a class by name using the current class loader
///
/// - Parameter name: JNI slash-separated class path (e.g. `"java/lang/String"`)
/// - Returns: A wrapped class reference or `nil` if not found
findClass(name: JClassName) -> JClass?
```

## Class Hierarchy

```swift
/// Gets the superclass of a Java class
getSuperclass(of clazz: JClass) -> JClass?

/// Determines if `clazz2` is assignable to `clazz1`,
/// equivalent to `clazz1.isAssignableFrom(clazz2)` in Java
isAssignable(from clazz1: JClass, to clazz2: JClass) -> Bool
```

Read more about [→ classes](classes.md)

## Reflection Conversion

```swift
/// Converts a Java `Method` object to a native method ID
fromReflectedMethod(method: JObject) -> JMethodId?

/// Converts a Java `Field` object to a native field ID
fromReflectedField(field: JObject) -> JFieldId?

/// Converts a native method ID into a Java `Method` or `Constructor` object
///
/// - Parameters:
///   - clazz: The class that declares the method
///   - methodId: The native method ID
///   - isStatic: Whether the method is static
toReflectedMethod(clazz: JClass, methodId: JMethodId, isStatic: Bool) -> JObject?

/// Converts a native field ID into a Java `Field` object
///
/// - Parameters:
///   - clazz: The declaring class
///   - fieldId: Native field ID
///   - isStatic: Whether the field is static
toReflectedField(clazz: JClass, fieldId: JFieldId, isStatic: Bool) -> JObject?
```

## Exceptions

```swift
/// Throws an existing exception object in the JVM
///
/// - Parameter throwable: A `jthrowable` object
/// - Returns: JNI status (`0` = OK, `-1` = error)
throwException(throwable: jthrowable) -> Int32

/// Throws a new exception given a class and error message
///
/// - Parameters:
///   - clazz: Exception class (e.g. `java/lang/IllegalArgumentException`)
///   - message: Error message
/// - Returns: JNI status
throwNew(clazz: JClass, message: String) -> Int32

/// Checks whether a Java exception has been thrown in the current thread
///
/// - Returns: A throwable object if an exception is pending, or `nil`
exceptionOccurred() -> JThrowable?

/// Describes the current exception (if any) to stderr (for debugging)
exceptionDescribe()

/// Clears the current exception from the JVM (if one is pending)
exceptionClear()

/// Triggers a fatal error in the JVM with a custom message
/// This terminates the VM
fatalError(message: String)

/// Throws a Java exception object that is a subclass of `Throwable`
throwObject(throwable: jobject) -> Int32

/// Throws a `JThrowable` instance and returns a typed JNI status
throwObject(throwable: JThrowable) -> JNIStatus
```
<details>
    <summary>Example: Detecting and Handling Java Exceptions</summary>

```swift
let env = JEnv.current()

// Call a Java method that might throw an exception
let result = env.callVoidMethod(...)

// Check if an exception occurred
if let exception = env.exceptionOccurred() {
    // Describe the exception (prints details to stderr)
    env.exceptionDescribe()
    
    // Clear the exception to allow further JNI operations
    env.exceptionClear()
    
    // Optionally, rethrow the exception or handle it in Swift
    print("Java exception: \(exception.getMessage())")
}
```
</details>
<details>
    <summary>Example: Throwing a New Java Exception</summary>

```swift
let env = JEnv.current()

// Define the exception class and message
if let exceptionClass = env.findClass(name: "java/lang/IllegalArgumentException") {
    let message = "Invalid argument provided"
    
    // Throw the exception
    let status = env.throwNew(clazz: exceptionClass, message: message)
    
    if status != 0 {
        print("Failed to throw Java exception")
    }
}
```
</details>
<details>
    <summary>Example: Throwing an Existing Java Exception Object</summary>

```swift
let env = JEnv.current()

// Create or retrieve a Java exception object
let exceptionObject: JObject = ... // Obtain a `JThrowable` or `JObject`

// Throw the exception
let status = env.throwObject(throwable: exceptionObject)

if status != 0 {
    print("Failed to throw Java exception object")
}
```
</details>
<details>
    <summary>Example: Triggering a Fatal Error</summary>

```swift
let env = JEnv.current()

// Terminate the JVM with a custom message
env.fatalError(message: "Critical error occurred in Swift")
```
</details>

## Reference Frames

```swift
/// Pushes a new local reference frame,
/// allowing scoped object reference tracking
///
/// - Parameter capacity: Number of local refs to allocate.
/// - Returns: JNI status code
pushLocalFrame(capacity: jint) -> Int32

/// Pops the current local frame, returning a single retained reference
///
/// - Parameter result: Optional object to retain from the popped frame
popLocalFrame(result: JObject?) -> JObject?
```

## Reference Management

```swift
/// Promotes a local reference to a global one (GC-safe)
newGlobalRef(obj: JObject) -> JObject?

/// Deletes a global reference previously promoted
deleteGlobalRef(globalRef: JObject)

/// Deletes a local reference to allow early GC
deleteLocalRef(localRef: JObject)

/// Returns whether two `jobject`s refer to the same underlying Java object
isSameObject(obj1: JObject, obj2: JObject) -> Bool

/// Creates a new local reference to the given object
newLocalRef(obj: JObject) -> JObject?

/// Ensures there's room for `capacity`
/// more local references in the current frame
///
/// - Returns: JNI status
ensureLocalCapacity(capacity: jint) -> Int32
```
<details>
    <summary>Example: Promoting a Local Reference to a Global Reference</summary>

```swift
let env = JEnv.current()

// `localRef` is a pointer to a Java object obtained from JNI
// which now needs to be converted to a global reference
if let globalRef = env.newGlobalRef(localRef) {
    print("Successfully promoted to a global reference")
    // Use `globalRef` safely across threads
    // Use `env.deleteGlobalRef(globalRef)` when done
} else {
    print("Failed to promote to a global reference")
}
```
</details>
<details>
    <summary>Example: Deleting a Global Reference</summary>

```swift
let env = JEnv.current()

env.deleteGlobalRef(globalRef)
print("Global reference deleted")
```
</details>
<details>
    <summary>Example: Deleting a Local Reference</summary>

```swift
let env = JEnv.current()

env.deleteLocalRef(localRef)
print("Local reference deleted")
```
</details>
<details>
    <summary>Example: Checking if Two Java Objects are the Same</summary>

```swift
let env = JEnv.current()

// Assume `obj1` and `obj2` are `JObject` instances
if env.isSameObject(obj1, obj2) {
    print("Both objects refer to the same Java object")
} else {
    print("Objects are different")
}
```
</details>
<details>
    <summary>Example: Ensuring Local Reference Capacity</summary>

```swift
let env = JEnv.current()

// Ensure there is room for 10 more local references
let status = env.ensureLocalCapacity(10)
if status == 0 {
    print("Successfully ensured local reference capacity")
} else {
    print("Failed to ensure local reference capacity")
}
```
</details>

## Object Allocation

```swift
/// Allocates an instance of a class without calling its constructor
allocObject(clazz: JClass) -> JObject?
```

## Object Creation

```swift
/// Creates a new Java object by calling its constructor
/// using a `jvalue[]` argument list
///
/// - Parameters:
///   - clazz: The class of the object to instantiate
///   - constructor: A `JMethodId` representing the `<init>` constructor method
///   - args: array of values for the constructor,
///           or `nil` if none
/// - Returns: A new `JObject` instance or `nil` if object creation failed
newObject(
    clazz: JClass,
    constructor: JMethodId,
    args: [any JValuable]? = nil
) -> JObject?
```

Constructor `methodID` can be looked up using `env.getMethodId(...)` with name `<init>` and appropriate signature [→ read below](env.md#method-lookup)

Read more about [→ JValuable](values.md)

## Object Info

```swift
/// Checks whether a Java object is an instance of a given class
///
/// Equivalent to `clazz.isInstance(obj)` in Java
///
/// - Parameters:
///   - object: The Java object to check
///   - clazz: The class to compare against
/// - Returns: `true` if `object` is an instance of `clazz`
///             or its subclass; otherwise, `false`
isInstanceOf(object: JObject, clazz: JClass) -> Bool
```

## Method Lookup

```swift
/// Finds an instance method ID for a method declared in the given class
///
/// This wraps the JNI `GetMethodID` call and converts it into a `JMethodId`
///
/// - Parameters:
///   - clazz: The class in which the method is declared
///   - name: The method name (e.g., `"toString"`)
///   - sig: The method signature (e.g., `"()Ljava/lang/String;"`)
/// - Returns: A `JMethodId` if found,
///            or `nil` if the method doesn't exist or lookup fails
getMethodId(
    clazz: JClass,
    name: String,
    sig: JMethodSignature
) -> JMethodId?
```

Read more about [→ signatures](signature.md#signing-methods)

## Instance Method Calls

JNI provides a variety of methods to call instance methods on Java objects, depending on the return type of the method being called.

Supported return types include `void`, primitive types (like `int`, `float`, etc.), and object types.

Depending on the return type, you should choose different JNI method name.

If you want to call a method that returns an `int`, you would use `callIntMethod`. If the method returns an object, you would use `callObjectMethod`.

Let's break down the object method call process:

```swift
callObjectMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil,
    returningClass: JClass
) -> JObject?
```

- `object`: the Java object on which you want to call the method
- `methodId`: the method identifier obtained either using `env.getMethodId(...)` or `clazz.methodId(...)` [→ read more](methods.md)
- `args`: an optional array of arguments to pass to the method [→ read more](methods.md)
- `returningClass`: the `JClass` of the object that the method returns [→ read more](classes.md)

The method returns a `JObject?`, which is the result of the method call, or `nil` if the call failed.

The same pattern applies to primitive return types (void, int, float, etc.), but hese methods donot require the `returningClass` parameter since primitive types are known.

```swift
callBooleanMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Bool
```

<details>
    <summary>Full List of Methods</summary>

```swift
/// Call a Java method returning `byte`.
callByteMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int8

/// Call a Java method returning `char`.
callCharMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> UInt16

/// Call a Java method returning `short`.
callShortMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int16

/// Call a Java method returning `int`.
callIntMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int32

/// Call a Java method returning `long`.
callLongMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int64

/// Call a Java method returning `float`.
callFloatMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Float

/// Call a Java method returning `double`.
callDoubleMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Double

/// Call a Java method returning `void`.
callVoidMethod(
    object: JObject,
    methodId: JMethodId,
    args: [any JValuable]? = nil
)
```
</details>

As you can see the principles are the same for all primitive types. Boolean method returns `Bool`, int method returns `Int32`, float method returns `Float`, void method returns nothing, and so on.

### Non-Virtual Method Calls

JNI also provides non-virtual method calls, which allow you to call a method as if it were a member of a specific class, bypassing any overridden implementations in subclasses.

It is the same as above but with `NonVirtual` in the method name, for example `callNonvirtualObjectMethod` or `callNonvirtualBooleanMethod`, and so on.

## Instance Fields

### Lookup

As well as methods you can access instance fields of Java objects.

First you have to find the field ID using `getFieldId`:

```swift
getFieldId(
    clazz: JClass,
    name: String,
    sig: JSignatureItem
) -> JFieldId?
```

And then you can get or set the field value using the appropriate JNI method based on the field type, same principles as with methods.

### Getters

```swift
getObjectField(
    object: JObject,
    fieldId: JFieldId,
    returningClass: JClass
) -> JObject?
```

With object fields you need to provide the `returningClass` parameter to specify the class of the object being retrieved.

For primitive fields, you would use methods like `getBooleanField`, `getIntField`, etc., which do not require the `returningClass` parameter.

```swift
getBooleanField(object: JObject, fieldId: JFieldId) -> Bool
```

<details>
    <summary>Full List of Methods</summary>

```swift
/// Get a `byte` field from a Java instance
getByteField(object: JObject, fieldId: JFieldId) -> Int8

/// Get a `char` field from a Java instance
getCharField(object: JObject, fieldId: JFieldId) -> UInt16

/// Get a `short` field from a Java instance
getShortField(object: JObject, fieldId: JFieldId) -> Int16

/// Get an `int` field from a Java instance
getIntField(object: JObject, fieldId: JFieldId) -> Int32

/// Get a `long` field from a Java instance
getLongField(object: JObject, fieldId: JFieldId) -> Int64

/// Get a `float` field from a Java instance
getFloatField(object: JObject, fieldId: JFieldId) -> Float

/// Get a `double` field from a Java instance
getDoubleField(object: JObject, fieldId: JFieldId) -> Double
```
</details>

### Setters

For setting object fields, you would use:

```swift
setObjectField(
    object: JObject,
    fieldId: JFieldId,
    value: JObject?
)
```

Here, `value` is the new object to set for the field, and `object` is the instance containing the field.

For primitive fields, you would use methods like `setBooleanField`, `setIntField`, etc.

```swift
setBooleanField(object: JObject, fieldId: JFieldId, value: jboolean)
```

<details>
    <summary>Full List of Methods</summary>

```swift
/// Set a `byte` field on a Java instance.
setByteField(object: JObject, fieldId: JFieldId, value: Int8)

/// Set a `char` field on a Java instance.
setCharField(object: JObject, fieldId: JFieldId, value: UInt16)

/// Set a `short` field on a Java instance.
setShortField(object: JObject, fieldId: JFieldId, value: Int16)

/// Set an `int` field on a Java instance.
setIntField(object: JObject, fieldId: JFieldId, value: Int32)

/// Set a `long` field on a Java instance.
setLongField(object: JObject, fieldId: JFieldId, value: Int64)

/// Set a `float` field on a Java instance.
setFloatField(object: JObject, fieldId: JFieldId, value: Float)

/// Set a `double` field on a Java instance.
setDoubleField(object: JObject, fieldId: JFieldId, value: Double)
```
</details>

## Static Method Lookup

Static methods are looked up similarly to instance methods, but on the class level and its lookup is done using `getStaticMethodId`:

```swift
getStaticMethodId(
    clazz: JClass,
    name: String,
    sig: JMethodSignature
) -> JMethodId?
```

## Static Method Calls

Static methods are called similarly to instance methods, but you provide the class instead of an object.

```swift
callStaticObjectMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil,
    returningClass: JClass
) -> JObject?
```

The same applies to primitive return types like `callStaticIntMethod`, `callStaticBooleanMethod`, etc.

```swift
callStaticBooleanMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Bool
```

<details>
    <summary>Full List of Methods</summary>

```swift
/// Call a static method returning `byte`.
callStaticByteMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int8

/// Call a static method returning `char`.
callStaticCharMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> UInt16

/// Call a static method returning `short`.
callStaticShortMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int16

/// Call a static method returning `int`.
callStaticIntMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int32

/// Call a static method returning `long`.
callStaticLongMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Int64

/// Call a static method returning `float`.
callStaticFloatMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Float

/// Call a static method returning `double`.
callStaticDoubleMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
) -> Double

/// Call a static method returning `void`.
callStaticVoidMethod(
    clazz: JClass,
    methodId: JMethodId,
    args: [any JValuable]? = nil
)
```
</details>

## Static Fields

### Lookup

Static fields are looked up similarly to instance fields, but on the class level and its lookup is done using `getStaticFieldId`:

```swift
getStaticFieldId(
    clazz: JClass,
    name: String,
    sig: JSignatureItem
) -> JFieldId?
```

### Getters

```swift
getStaticObjectField(
    staticClazz: JClass,
    fieldId: JFieldId,
    clazz: JClass
) -> JObject?
```

With object fields you need to provide the `clazz` parameter to specify the class of the object being retrieved.

For primitive fields, you would use methods like `getStaticBooleanField`, `getStaticIntField`, etc., which do not require the `clazz` parameter.

```swift
getStaticBooleanField(clazz: JClass, fieldId: JFieldId) -> Bool
```

<details>
    <summary>Full List of Static Methods</summary>

```swift
/// Get the value of a static field returning a `byte`.
getStaticByteField(clazz: JClass, fieldId: JFieldId) -> Int8

/// Get the value of a static field returning a `char`.
getStaticCharField(clazz: JClass, fieldId: JFieldId) -> UInt16

/// Get the value of a static field returning a `short`.
getStaticShortField(clazz: JClass, fieldId: JFieldId) -> Int16

/// Get the value of a static field returning an `int`.
getStaticIntField(clazz: JClass, fieldId: JFieldId) -> Int32

/// Get the value of a static field returning a `long`.
getStaticLongField(clazz: JClass, fieldId: JFieldId) -> Int64

/// Get the value of a static field returning a `float`.
getStaticFloatField(clazz: JClass, fieldId: JFieldId) -> Float

/// Get the value of a static field returning a `double`.
getStaticDoubleField(clazz: JClass, fieldId: JFieldId) -> Double
```
</details>

### Setters

For setting object fields, you would use:

```swift
setStaticObjectField(
    clazz: JClass,
    fieldId: JFieldId,
    value: JObject?
)
```

Here, `value` is the new object to set for the static field, and `clazz` is the class containing the static field.

For primitive fields, you would use methods like `setStaticBooleanField`, `setStaticIntField`, etc.

```swift
setStaticBooleanField(clazz: JClass, fieldId: JFieldId, value: jboolean)
```

<details>
    <summary>Full List of Static Methods</summary>

```swift
/// Set a static `byte` field.
setStaticByteField(clazz: JClass, fieldId: JFieldId, value: Int8)

/// Set a static `char` field.
setStaticCharField(clazz: JClass, fieldId: JFieldId, value: UInt16)

/// Set a static `short` field.
setStaticShortField(clazz: JClass, fieldId: JFieldId, value: Int16)

/// Set a static `int` field.
setStaticIntField(clazz: JClass, fieldId: JFieldId, value: Int32)

/// Set a static `long` field.
setStaticLongField(clazz: JClass, fieldId: JFieldId, value: Int64)

/// Set a static `float` field.
setStaticFloatField(clazz: JClass, fieldId: JFieldId, value: Float)

/// Set a static `double` field.
setStaticDoubleField(clazz: JClass, fieldId: JFieldId, value: Double)
```
</details>

## Java Strings

```swift
/// Create a new Java string from a UTF-16 buffer
///
/// This corresponds to the JNI `NewString` function.
/// It creates a `jstring` from the provided buffer of 16-bit characters
///
/// - Parameters:
///   - chars: Pointer to the UTF-16 characters (`jchar` array)
///   - length: Number of characters in the array (not bytes)
/// - Returns: A new `jstring`, or `nil` if creation fails
newString(chars: UnsafePointer<jchar>, length: jint) -> jstring?

/// Get the UTF-16 length of a Java string
///
/// Equivalent to `GetStringLength`.
/// This returns the number of UTF-16 code units, not bytes
///
/// - Parameter string: The Java string
/// - Returns: Number of UTF-16 characters in the string
getStringLength(string: jstring) -> Int32

/// Retrieve a pointer to the UTF-16 contents of a Java string
///
/// This returns a read-only buffer of `jchar` values
/// that represent the UTF-16 encoding of the string
///
/// - Important: After use, you **must** call `releaseStringChars`
///              to avoid memory leaks
///
/// - Parameters:
///   - string: Java `jstring` instance
///   - isCopy: Optional pointer that receives whether the data is a copy
/// - Returns: Pointer to UTF-16 characters or `nil`
getStringChars(
    string: jstring,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafePointer<jchar>?

/// Release the buffer obtained from `getStringChars`
///
/// - Parameters:
///   - string: The original Java string
///   - chars: Pointer returned from `getStringChars`
releaseStringChars(string: jstring, chars: UnsafePointer<jchar>)

/// Create a new Java UTF-8 string from a Swift `String`
///
/// This wraps `NewStringUTF`, which creates a Java string
/// from a C-style UTF-8 encoded string
///
/// - Parameter string: A Swift string (will be converted to C UTF-8)
/// - Returns: A new `jstring`, or `nil` if creation fails
newStringUTF(string: String) -> jstring?

/// Get the UTF-8 byte length of a Java string (not character count)
///
/// Equivalent to `GetStringUTFLength`, this returns the number of bytes
/// required to encode the Java string in UTF-8
///
/// - Parameter string: The Java string
/// - Returns: Number of bytes in UTF-8 encoding
getStringUTFLength(string: jstring) -> Int32

/// Retrieve a pointer to the UTF-8 encoded contents of a Java string
///
/// This pointer is valid until `releaseStringUTFChars` is called
///
/// - Important: After use, call `releaseStringUTFChars`
///
/// - Parameters:
///   - string: The Java `jstring`
///   - isCopy: Optional pointer to receive copy status
/// - Returns: Pointer to null-terminated UTF-8 C string
getStringUTFChars(
    string: jstring,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafePointer<CChar>?

/// Release a UTF-8 string buffer previously acquired from `getStringUTFChars`
///
/// - Parameters:
///   - string: The original Java string
///   - chars: Pointer obtained from `getStringUTFChars`
releaseStringUTFChars(
    string: jstring,
    chars: UnsafePointer<CChar>
)
```

## Arrays

```swift
/// Get the number of elements in a Java array
///
/// Works for any array type including `jobjectArray`, `jintArray`, etc.
///
/// - Parameter array: A JNI `jarray` handle
/// - Returns: The number of elements in the array
getArrayLength(array: jarray) -> Int32
```

### Object Arrays

```swift
/// Create a new Java object array
///
/// This corresponds to JNI's `NewObjectArray`
///
/// - Parameters:
///   - length: Number of elements in the array
///   - clazz: Class type of array elements
///   - initialElement: Optional element to initialize all entries with
/// - Returns: A `JObjectArray` wrapping the created array
newObjectArray(
    length: jint,
    clazz: JClass,
    initialElement: JObject? = nil
) -> JObjectArray?

/// Get an element from a Java object array
///
/// - Parameters:
///   - array: The object array
///   - index: The index of the element to retrieve
/// - Returns: A `JObject` wrapper for the element at that index
getObjectArrayElement(array: JObjectArray, index: jint) -> JObject?

/// Get an element from a Java object array
///
/// - Parameters:
///   - array: The object array
///   - index: The index of the element to retrieve
/// - Returns: A `JObject` wrapper for the element at that index
getObjectArrayElement(
    arrayObject: JObject,
    index: jint,
    returningClass: JClass? = nil
) -> JObject?

/// Set an element in a Java object array
///
/// - Parameters:
///   - array: The target object array
///   - index: Index of the element to set
///   - value: The value to insert (may be `nil`)
setObjectArrayElement(array: JObjectArray, index: jint, value: JObject?)

/// Set an element in a Java object array
///
/// - Parameters:
///   - arrayObject: The target object array
///   - index: Index of the element to set
///   - value: The value to insert (may be `nil`)
setObjectArrayElement(arrayObject: JObject, index: jint, value: JObject?)
```

### Primitive Arrays

```swift
/// Get a pointer to the contents of a `boolean[]` Java array
///
/// - Parameters:
///   - array: A JNI `jbooleanArray`
///   - isCopy: Optional pointer to receive JNI copy status (1=copy, 0=direct)
/// - Returns: Pointer to native memory holding the array contents
getBooleanArrayElements(
    array: jbooleanArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<jboolean>?
```

<details>
    <summary>More Examples</summary>

```swift
/// Get a pointer to a `byte[]` Java array
getByteArrayElements(
    array: jbyteArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<jbyte>?

/// Get a pointer to a `char[]` Java array
getCharArrayElements(
    array: jcharArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<jchar>?

/// Get a pointer to a `short[]` Java array
getShortArrayElements(
    array: jshortArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<jshort>?

/// Get a pointer to an `int[]` Java array
getIntArrayElements(
    array: jintArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<Int32>?

/// Get a pointer to a `long[]` Java array
getLongArrayElements(
    array: jlongArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<Int64>?

/// Get a pointer to a `float[]` Java array
getFloatArrayElements(
    array: jfloatArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<Float>?

/// Get a pointer to a `double[]` Java array
getDoubleArrayElements(
    array: jdoubleArray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutablePointer<Double>?
```
</details>

### Release

```swift
/// Release memory returned by `getBooleanArrayElements`
///
/// - Parameters:
///   - array: The original `jbooleanArray`
///   - elems: Pointer previously returned
///   - mode: `0` to copy back,
///           `JNI_COMMIT` to copy without free,
///           `JNI_ABORT` to discard changes
releaseBooleanArrayElements(
    array: jbooleanArray,
    elems: UnsafeMutablePointer<jboolean>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseBooleanArrayElements!(env, array, elems, mode)
}
```

<details>
    <summary>More Examples</summary>

```swift
/// Release memory for `byte[]`
releaseByteArrayElements(
    array: jbyteArray,
    elems: UnsafeMutablePointer<jbyte>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseByteArrayElements!(env, array, elems, mode)
}

/// Release memory for `char[]`
releaseCharArrayElements(
    array: jcharArray,
    elems: UnsafeMutablePointer<jchar>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseCharArrayElements!(env, array, elems, mode)
}

/// Release memory for `short[]`
releaseShortArrayElements(
    array: jshortArray,
    elems: UnsafeMutablePointer<jshort>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseShortArrayElements!(env, array, elems, mode)
}

/// Release memory for `int[]`
releaseIntArrayElements(
    array: jintArray,
    elems: UnsafeMutablePointer<jint>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseIntArrayElements!(env, array, elems, mode)
}

/// Release memory for `long[]`
releaseLongArrayElements(
    array: jlongArray,
    elems: UnsafeMutablePointer<jlong>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseLongArrayElements!(env, array, elems, mode)
}

/// Release memory for `float[]`
releaseFloatArrayElements(
    array: jfloatArray,
    elems: UnsafeMutablePointer<jfloat>,
    mode: jint = 0
) {
    env.pointee!.pointee.ReleaseFloatArrayElements!(env, array, elems, mode)
}

/// Release memory for `double[]`
releaseDoubleArrayElements(
    array: jdoubleArray,
    elems: UnsafeMutablePointer<jdouble>,
    mode: jint = 0
)
```
</details>

### Region Getters

```swift
/// Copy a region from a Java `boolean[]` array into a native buffer
///
/// - Parameters:
///   - array: The Java boolean array
///   - start: Starting index to copy from
///   - length: Number of elements to copy
///   - buffer: Pointer to destination buffer to write into
getBooleanArrayRegion(
    array: jbooleanArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jboolean>
)
```

<details>
    <summary>More Examples</summary>

```swift
/// Copy a region from a Java `byte[]` array
getByteArrayRegion(
    array: jbyteArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jbyte>
)

/// Copy a region from a Java `char[]` array
getCharArrayRegion(
    array: jcharArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jchar>
)

/// Copy a region from a Java `short[]` array
getShortArrayRegion(
    array: jshortArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jshort>
)

/// Copy a region from a Java `int[]` array
getIntArrayRegion(
    array: jintArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jint>
)

/// Copy a region from a Java `long[]` array
getLongArrayRegion(
    array: jlongArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jlong>
)

/// Copy a region from a Java `float[]` array
getFloatArrayRegion(
    array: jfloatArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jfloat>
)

/// Copy a region from a Java `double[]` array
getDoubleArrayRegion(
    array: jdoubleArray,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jdouble>
)
```
</details>

### Region Setters

```swift
/// Set a region of a Java `boolean[]` array from native buffer data
///
/// - Parameters:
///   - array: The Java array to update
///   - start: The starting index to write into
///   - length: Number of elements to write
///   - buffer: Native buffer containing the data to write
setBooleanArrayRegion(
    array: jbooleanArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jboolean>
)
```

<details>
    <summary>More Examples</summary>

```swift
/// Set region of `byte[]` array
setByteArrayRegion(
    array: jbyteArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jbyte>
)

/// Set region of `char[]` array
setCharArrayRegion(
    array: jcharArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jchar>
)

/// Set region of `short[]` array
setShortArrayRegion(
    array: jshortArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jshort>
)

/// Set region of `int[]` array
setIntArrayRegion(
    array: jintArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jint>
)

/// Set region of `long[]` array
setLongArrayRegion(
    array: jlongArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jlong>
)

/// Set region of `float[]` array
setFloatArrayRegion(
    array: jfloatArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jfloat>
)

/// Set region of `double[]` array
setDoubleArrayRegion(
    array: jdoubleArray,
    start: jint,
    length: jint,
    buffer: UnsafePointer<jdouble>
)
```
</details>

## Native Method Registration

```swift
/// Register native methods with a Java class
///
/// - Parameters:
///   - clazz: The class to register native methods with
///   - methods: Pointer to an array of `JNINativeMethod`
///   - count: Number of methods to register
/// - Returns: JNI status code (`JNI_OK` on success)
registerNatives(
    clazz: JClass,
    methods: UnsafePointer<JNINativeMethod>,
    count: jint
) -> Int32

/// Unregister all native methods previously registered for the given class
///
/// - Parameter clazz: The class to unregister methods from
/// - Returns: JNI status code (`JNI_OK` on success)
unregisterNatives(clazz: JClass) -> Int32
```

## Monitor (synchronized) Helpers

```swift
/// Enter the monitor associated with a Java object (like `synchronized` block)
monitorEnter(object: JObject) -> Int32

/// Exit the monitor associated with a Java object
monitorExit(object: JObject) -> Int32
```

## JVM Access

```swift
/// Get the `JavaVM*` associated with this `JNIEnv`
///
/// - Returns: The pointer to the `JavaVM`, or `nil` if retrieval fails
getJavaVM() -> UnsafeMutablePointer<JavaVM?>?
```

## String Region Access

```swift
/// Copy a region of a Java string's UTF-16 characters into a buffer
getStringRegion(
    string: jstring,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<jchar>
)

/// Copy a region of a Java string's UTF-8 characters into a buffer
getStringUTFRegion(
    string: jstring,
    start: jint,
    length: jint,
    buffer: UnsafeMutablePointer<CChar>
)
```

## Critical Array Access

```swift
/// Get a pointer to a primitive array's contents
/// in a critical section (fastest access)
///
/// - Warning: The GC is disabled while the pointer is held
getPrimitiveArrayCritical(
    array: jarray,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafeMutableRawPointer?

/// Release the pointer acquired from `getPrimitiveArrayCritical`
///
/// - Parameter mode: Either 0, `JNI_COMMIT`, or `JNI_ABORT`
releasePrimitiveArrayCritical(
    array: jarray,
    pointer: UnsafeMutableRawPointer,
    mode: jint = 0
)
```

## Critical String Access

```swift
/// Get a pointer to the characters of a Java string in a critical section
///
/// - Warning: The GC is disabled while the pointer is held
getStringCritical(
    string: jstring,
    isCopy: UnsafeMutablePointer<jboolean>? = nil
) -> UnsafePointer<jchar>?

/// Release a string pointer acquired from `getStringCritical`
releaseStringCritical(string: jstring, chars: UnsafePointer<jchar>)
```

## Weak Global Refs

```swift
/// Create a weak global reference to a Java object
///
/// Weak references allow the JVM to GC the object
/// when no strong references exist
newWeakGlobalRef(obj: JObject) -> jweak?

/// Delete a previously created weak global reference
deleteWeakGlobalRef(ref: jweak)
```

## Exception Check

```swift
/// Check if a Java exception is currently pending on this thread
///
/// - Returns: `true` if an exception has occurred
exceptionCheck() -> Bool
```

## Direct ByteBuffer

```swift
/// Create a `java.nio.DirectByteBuffer` from a raw memory address
///
/// - Parameters:
///   - address: Pointer to the memory to wrap
///   - capacity: Number of bytes the buffer should expose
newDirectByteBuffer(address: UnsafeMutableRawPointer, capacity: Int64) -> jobject?

/// Get the memory address backing a direct byte buffer
getDirectBufferAddress(buffer: JObject) -> UnsafeMutableRawPointer?

/// Get the capacity in bytes of a direct byte buffer
getDirectBufferCapacity(buffer: JObject) -> Int64
```

## Reference Type Inspection

```swift
/// Inspect the type of a JNI object reference
/// (`local`, `global`, `weak global`)
///
/// - Returns: A `JObjectRefType` describing the reference type
getObjectRefType(obj: JObject) -> JObjectRefType
```

