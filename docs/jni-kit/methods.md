# Methods

## Instance methods

First of all, you need to know that the method call name depends on what type your method returns:

| Name              | Return type |
|-------------------|-------------|
| callObjectMethod  | JObject     |
| callBooleanMethod | Bool        |
| callByteMethod    | Int8        |
| callCharMethod    | UInt16      |
| callShortMethod   | Int16       |
| callIntMethod     | Int32       |
| callLongMethod    | Int64       |
| callFloatMethod   | Float       |
| callDoubleMethod  | Double      |
| callVoidMethod    | Void        |

Then you have a choice of wether to call it on `env` which is a bit more complex or call it on an `object` much shorter convenience way.

### Calling method that returns an object

The way with `env`:
```swift
let env = JEnv.current()
let methodId = clazz.methodId(
    env: env,
    name: "getSomeObject",
    signature: .returning(.object("com/mylib/mypackage/SomeObject"))
)
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = env.callObjectMethod(
    object: object,
    methodId: methodId,
    returningClass: returningClazz
)
```

The way with `object`:
```swift
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = object.callObjectMethod(
    name: "getSomeObject",
    returningClass: returningClazz
)
```
So the difference is that you don't have to get the `methodID` yourself, as it is handled automatically under the hood. You also have the option to not pass the `env`.

!!!note
    The way with `object` is always shorter than with `env`

### Calling method that pass an object and returns nothing

The way with `env`:
```swift
let env = JEnv.current()
let methodId = clazz.methodId(
    env: env,
    name: "setSomeObject",
    signature: .init(
        .object("com/mylib/mypackage/SomeObject"),
        returning: .void // optional, .void by default
    )
)
let resultObject = env.callVoidMethod(
    object: object,
    methodId: methodId,
    args: [someObject.object]
)
```

The way with `object`:
```swift
object.callVoidMethod(
    name: "setSomeObject",
    args: someObject.signed(as: "com/mylib/mypackage/SomeObject")
    // or just someObject if you sure that automatic class is correct
)
```
It is even shorter than the getter.

### Calling method that pass a string and returns nothing
The way with `object`:
```swift
object.callVoidMethod(
    name: "setString",
    args: "Hello" // or "Hello".wrap().signedAsString()
    // or "Hello".signedAsCharSequence()
)
// String is always signed as java/lang/String by default
```

## Static methods

### Calling static method that returns an object

The way with `env`:
```swift
let env = JEnv.current()
let methodId = clazz.staticMethodId(
    name: "getSomeObject",
    signature: .returning(.object("com/mylib/mypackage/SomeObject"))
)
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = env.callStaticObjectMethod(
    clazz: clazz,
    methodId: methodId,
    returningClass: returningClazz
)
```

The way with `clazz`:
```swift
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = clazz.staticObjectMethod(
    name: "getSomeObject",
    returningClass: returningClazz
)
```