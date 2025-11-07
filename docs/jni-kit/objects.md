# How to create a new object

JNIKit provides the `JObject` class, which holds a reference to a Java object.

## Construct JObject with no arguments

Convenient way with `clazz`:
```swift
guard
    let clazz = JClass.load("com/mylib/mypackage/MyClass"),
    let object = clazz.newObject()
else { return }
```
More explicit way with `env`:
```swift
guard
    let env = JEnv.current(),
    let clazz = JClass.load("com/mylib/mypackage/MyClass"),
    let methodId = clazz.methodId(
        env: env,
        name: "<init>",
        signature: .returning(.void)
    ),
    let object = env.newObject(
        clazz: clazz,
        constructor: methodId
    )
else { return }
```

## Construct JObject with arguments

Arguments have to be listed in both `methodId` and `newObject` in case of creating with `env`.

Let's assume a class constructor expects `jint`, `jfloat`, and `jobject` (which is `java/lang/String`).

Convenient way with `clazz`:
```swift
let object = clazz.newObject(
    123,
    1.23,
    "Hello" // or "Hello".wrap().signedAsString()
)
```

Explicit way with `env`:
```swift
let methodId = clazz.methodId(
    env: env,
    name: "<init>",
    signature: .init(
        .int,
        .float,
        .object("java/lang/String"),
        returning: .void // optional, .void by default
    )
)
let object = env.newObject(
    clazz: clazz,
    constructor: methodId,
    args: [
        123, // Int32 -> jint
        1.23, // Float -> jfloat
        "Hello".wrap()!.object // JObject -> jstring
    ]
)
```

The constructed object is now a `JObject` that holds a global reference to the instance and a global reference to its class. Keep this object for as long as you need it.

**When `JObject` is deinitialized, its underlying `jobject` reference is released automatically.**
> ⚠️ Do not delete the underlying `jobject` reference manually, and do not use `jobject` from `JObject` anywhere outside.