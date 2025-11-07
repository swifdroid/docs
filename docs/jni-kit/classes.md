# Classes

Loading a class is the first thing you will face.

There are a few ways to load it.

## Via `env`

```swift
let env = JEnv.current()!
let clazz: JClass = env.findClass("com/mylib/mypackage/MyClass")
```
This way you will use the system's class loader, which will fail loading your dynamically loaded class. It should be used only for fundamental classes like `java/lang/Object`.

## Via `classLoader`

The goal is to retrieve the application's class loader, which has access to dynamically loaded classes.

In pure Java you could retrieve a class loader from any `jobject`, as was shown above in the initialize method.

In Android the class loader is available in `ApplicationContext` or `ActivityContext`, as well as in any `jobject` of course, but there is a shorter convenience getter like `context.getClassLoader()`.

The usage is as simple as
```swift
let clazz: JClass = classLoader.loadClass("com/mylib/mypackage/MyClass")
```

## Via `cached` instance

The preferred way is to use a convenience `JClass` method:

```swift
let clazz: JClass = JClass.load("com/mylib/mypackage/MyClass")
```
This way it tries to get it first from cache, then from the cached class loader, and if there is no cached class loader, then from the system's class loader.

!!! tip
    You can call [→ static methods](methods.md#static-methods) on the **`JClass`** type directly.

## Class names

As you can see above, the class name is represented as a `String`, but actually it is not. The `JClassName` object contains the class name in all possible forms: dotted, slashed, and even just its pure name.

You can predefine classnames as constants in the code for easy reuse and typo safety:
```swift
let class1 = JClassName("com/mylib/mypackage/MyClass1")
let class2 = JClassName("com/mylib/mypackage/MyClass2")
```
and then pass it like this:
```swift
JClass.load(class1)
```
or even put it into a `JClassName` extension:
```swift
extension JClassName {
    static let class1 = JClassName("com/mylib/mypackage/MyClass1")
    static let class2 = JClassName("com/mylib/mypackage/MyClass2")
}
```
and then use it anywhere like this:
```swift
JClass.load(.class1)
```