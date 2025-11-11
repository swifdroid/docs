# Basics of JNI in Swift

All communication between Swift and Java code happens via JNI – Java Native Interface.

It is low-level C-based API that allows calling Java methods, accessing fields, creating objects, and more. As well as allowing Java code to call Swift functions which are exposed via **`@_cdecl`**.

## JNI Environment

**The main concept** of JNI is the `JNIEnv` pointer, which represents the JNI environment. It is used to interact with the JVM (Java Virtual Machine) from native code.

Lifecycle looks like this:

1. You initialize JNI connection from Java side by calling a Swift function exposed via **`@_cdecl`**.
2. You receive `JNIEnv` pointer in that function as the first argument.
3. You can use that pointer to create **`JEnv`** instance from JNIKit, which provides a beautiful Swift layer over JNI C API.
4. Using **`JEnv`** instance, you can create Java objects, call methods, access fields, and more.

Since Swift is concurrent by design, each thread must attach itself to the JVM to obtain its own `JNIEnv` pointer. JNIKit handles this automatically when you call `JEnv.current()`.

Read more about [→ environment](env.md).

## Local and Global References

**The second concept** of JNI is that it provides pointers to Java objects.

By JNI design, each Java object pointer passed to Swift as a **local reference**, meaning it can only be used within the current thread.

Original lifecycle looks like this:

1. Java object pointer passed to Swift as a local reference.
2. You use that local reference within the current thread.
3. You convert it to a global reference manually.
4. You release the local reference.
5. When you no longer need the global reference, you release it manually.


JNIKit takes care of the heavy lifting for you. All **local** references are seamlessly converted into **global** references, ensuring smooth operation in concurrent scenarios. These global references are automatically managed and released when their corresponding Swift objects are deinitialized, so you can focus on writing Swift code without worrying about reference lifecycles.

With JNIKit lifecycle looks like this:

1. Java object pointer passed to Swift as a local reference.
2. JNIKit is wrapping it into **`JObject`** which automatically converts it into a global reference.
3. When **`JObject`** is deinitialized, it automatically releases the global reference.

!!! important
    **`JObject`** can be used across different threads safely.

## Naming convention

Let's talk about declaring JNI methods on the Swift side with `@_cdecl`. 

> **`@_cdecl`** naming convention is important as it follows  
JNI naming pattern **`Java_<package>_<class>_<method>`**, where:  
> **`<package>`** is the fully qualified package name with underscores instead of dots  
> **`<class>`** is the class name  
> **`<method>`** is the method name

Let's say you have a package **`com.mylib.somecode`** which contains `AwesomeCode` class which contains exported **`hello`** method.  
**`@_cdecl`** for it will be:
```swift
@_cdecl("Java_com_mylib_somecode_AwesomeCode_hello")
```

Method arguments vary depending on your class and method declaration.

**The first** argument is required and is always a pointer to the JNI environment, which is used to interact with the JVM.

```js
envPointer: UnsafeMutablePointer<JNIEnv?>
```

**The second** argument is required and depends on whether the method is static or not.

So it could be
```js
thizRef: jobject
```
if the method is **instance-based (non-static)**, in which case it will pass the instance from which it was called.

Or it could be
```js
clazzRef: jobject
```
if method was `static` so it will pass just a reference to the Java class.

**The next** arguments are optional and directly represent the arguments declared in the Java method.

**The return** type can be empty (`void`) or can return a value (even an optional one).

The method can't be marked `async`, as it is not supported by **`@_cdecl`**. However, it is still possible to run asynchronous code in Swift if the original Java method was running on a **non-UI** thread.

### Wrapping up

First two arguments are required and passed into Swift automatically by JVM↔JNI mechanism, while third and subsequent arguments are actually arguments from your Java method.

Continue to [→ initialization](initialization.md) to start coding.