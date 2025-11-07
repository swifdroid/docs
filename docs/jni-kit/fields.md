# Fields

## Instance fields

### Getting field value

The principle is the same, but you need a fieldID and can't pass any arguments. The signature is needed only if the field returns an object.

#### Getting an object

The way with `env`:
```swift
let env = JEnv.current()
let fieldId = object.clazz.fieldId(
    env: env,
    name: "someField",
    signature: .object("com/mylib/mypackage/SomeObject")
)
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = env.getObjectField(
    object: object,
    fieldId,
    clazz: returningClazz)
```

The way with `object`:
```swift
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
object.objectField(name: "someField", returningClass: returningClazz)
```

#### Getting an Int

The way with `env`:
```swift
let env = JEnv.current()
let fieldId = object.clazz.fieldId(
    env: env,
    name: "numberField",
    signature: .int
)
let resultInt = env.getIntField(object: object, fieldId)
```

The way with `clazz`:
```swift
object.intField(name: "numberField")
```

### Setting field value

#### Setting an object

The way with `env`:
```swift
let env = JEnv.current()
let fieldId = object.clazz.fieldId(
    env: env,
    name: "someField",
    signature: .object("com/mylib/mypackage/SomeObject")
)
env.setObjectField(
    object: object,
    fieldId,
    object.signed(as: "com/mylib/mypackage/SomeObject")
)
```

The way with `object`:
```swift
object.objectField(
    name: "someField",
    object.signed(as: "com/mylib/mypackage/SomeObject")
)
```

#### Setting an Int

The way with `env`:
```swift
let env = JEnv.current()
let fieldId = object.clazz.fieldId(
    env: env,
    name: "numberField",
    signature: .int
)
let resultInt = env.setIntField(object: object, fieldId, 123)
```

The way with `object`:
```swift
object.intField(name: "numberField", 123)
```

## Static fields

### How to get static field value

The way with `env`:
```swift
let env = JEnv.current()
let currentClazz = JClass.load("com/mylib/mypackage/MyClass")
let fieldId = currentClazz.fieldId(env: env, name: "someField", signature: .object("com/mylib/mypackage/SomeObject"))
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
let resultObject = env.getStaticObjectField(currentClazz, fieldId, clazz: returningClazz)
```

The way with `clazz`:
```swift
let currentClazz = JClass.load("com/mylib/mypackage/MyClass")
let returningClazz = JClass.load("com/mylib/mypackage/SomeObject")
currentClazz.staticFieldId(name: String, signature: JSignatureItem)
```