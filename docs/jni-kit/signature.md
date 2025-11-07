# Signature

We use it every time when discovering `methodID` or `fieldID`.

For methods it consists of two parts, the arguments types and the returning type:
```swift
// method with no arguments, returns Void
JSignature(returning: .void)
// method with no arguments, returns Int32
JSignature(returning: .int)
// method with Int32 argument, returns Void
JSignature(.int, returning: .void)
// method with JObject argument, returns Void
JSignature(.object("com/mylib/mypackage/SomeObject"), returning: .void)
// method with String argument, returns Void
JSignature(.object(JString.className), returning: .void)
```
Read more [→ below](#signing-methods).

For fields only returning type matters:
```swift
.int // for Int32
.float // for Float
// similar for the other primitive types
.object("com/mylib/mypackage/SomeObject") // for JObject
.object(JString.className) // for String
.object(JString.charSequenseClassName) // for String
```

## Signing objects

When passing a `JObject` as a method argument, you have two options for specifying its type signature

**Rely on automatic signature inference**, which uses the `JClass` associated with the `JObject`

Example with a generic `JObject`:
```swift
let object: JObject
object.callVoidMethod(name: "setView", args: object)
```
Example with a `JString`:
```swift
let string = "Hello"
object.callVoidMethod(name: "setView", args: string) // signed as java/lang/String by default
```

**Manually sign the object with a specific class** using the `signed(as:)` method

Example with a generic `JObject`:
```swift
let object: JObject
object.callVoidMethod(name: "setView", args: object.signed(as: "com/my/lib/SomeObject"))
```
Example with a `JString` (signed as a `CharSequence`):
```swift
let string = "Hello"
object.callVoidMethod(name: "setView", args: string.signedAsCharSequence())
```

## Signing Methods

When you want to call some JNI method you have to provide its signature.

For example, to call a method that takes no arguments and returns `void`, you can use:

```swift
JMethodSignature(returning: .void)
// or just
.returning(.void)
```
In JNI it is represented as `()V`.

For method that takes an `Int32` argument and returns a `JObject`, you can use:

```swift
JMethodSignature(.int, returning: .object("com/mylib/mypackage/SomeObject"))
```
In JNI it is represented as `(I)Lcom/mylib/mypackage/SomeObject;`.

You can also create method signatures with multiple arguments:

```swift
JMethodSignature(
    .int, .float, .object(JString.className), .double, 
    returning: .void
)
```

In JNI it is represented as `(IFLjava/lang/String;D)V`.

And so on, I think you got the idea.

<details>
    <summary>A bit more examples</summary>

```swift
// Method with no arguments, returns Int32
JMethodSignature(returning: .int)
// Represents as: ()I

// Method with no arguments, returns JObject
JMethodSignature(returning: .object("com/mylib/mypackage/SomeObject"))
// Represents as: ()Lcom/mylib/mypackage/SomeObject;

// Method with no arguments, returns String
JMethodSignature(returning: .object(JString.className))
// Represents as: ()Ljava/lang/String;

// or
JMethodSignature(returning: .object(JString.charSequenseClassName))
// Represents as: ()Ljava/lang/CharSequence;

// Method with Int32 argument, returns Void
JMethodSignature(.int, returning: .void)
// Represents as: (I)V

// Method with JObject argument, returns Void
JMethodSignature(.object("com/mylib/mypackage/SomeObject"), returning: .void)
// Represents as: (Lcom/mylib/mypackage/SomeObject;)V

// Method with String argument, returns Void
JMethodSignature(.object(JString.className), returning: .void)
// Represents as: (Ljava/lang/String;)V

// or
JMethodSignature(.object(JString.charSequenseClassName), returning: .void)
// Represents as: (Ljava/lang/CharSequence;)V
```
</details>