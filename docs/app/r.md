# Resource IDs

Resources are accessed via the generated Java `R` class. Each resource in your `res` directory is assigned a unique integer ID in this class, which is typically used to reference the resource in Java or Kotlin code.

The **Droid** framework does not generate a separate Swift `R` class. It just bridges Swift code to the generated Java `R` class using `@dynamicMemberLookup` feature, allowing you to access resource IDs as Swift properties directly.

In Android you can use simple `R.type.name` syntax to access resource IDs, where `type` is the resource type (like `string`, `drawable`, etc.) and `name` is the resource name defined in XML files.

It is important to note that not all resources are accessible directly through `R`. Certain resources, such as those provided by the Android framework, must be accessed using a class prefix like `android.R`.

**Droid** framework covers both cases as easy as:
```swift
let stringId: Int32 = R.string.my_string_resource
let iconId: Int32 = android.R.drawable.ic_dialog_email
```

If you need to access resource ID with a custom class then you have to write a small helper:
```swift
@MainActor
struct mylib: ~Copyable {
    static let R = InnerR("com/mycompany/mylib")
}
```
and use it like this:
```swift
let myStringId: Int32 = mylib.R.string.my_string_resource
```
Here `mylib.R` represents `com.mycompany.mylib.R` Java class.

## Types

The generated `R` class contains nested classes for each resource type, such as:

- `R.id`: for view IDs
- `R.attr`: for custom attributes
- `R.menu`: for menu resources
- `R.drawable`: for drawable resources
- `R.string`: for string resources
- `R.anim`: for animation resources
- `R.animator`: for animator resources
- `R.array`: for array resources
- `R.bool`: for boolean resources
- `R.color`: for color resources
- `R.dimen`: for dimension resources
- `R.fraction`: for fraction resources
- `R.integer`: for integer resources
- `R.interpolator`: for interpolator resources
- `R.layout`: for layout resources
- `R.mipmap`: for mipmap resources
- `R.plurals`: for plural resources
- `R.raw`: for raw resources
- `R.style`: for style resources
- `R.transition`: for transition resources
- `R.xml`: for XML resources