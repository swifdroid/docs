# Reactivity with @State

**`@State`** plays a central role in modern declarative UI development.

The **Droid** framework utilizes the unified **`@State`** property wrapper from the SwiftStream crossplatform ecosystem.

Even though it looks like SwiftUI's one, it is implemented differently under the hood. There is no global state machine that tracks and redraws everything. Android views are not temporary structs but classes. When you mark a property with **`@State`**, it automatically notifies only the views that actually depend on its value.

Here the text view listens to changes in a state variable.
```swift
@State var text = "Hello, World!"

VStack {
    TextView($text)
        .textColor(.blue)
    Button("Change Text").onClick {
        text = "Text Updated!"
    }
}
```
The text inside the `TextView` updates automatically when the button is pressed because `setText` called under the hood whenever `text` state changes.
!!! important ""
    Note the `$` prefix when passing the state variable to the `TextView`  
    This is required to pass the state reference instead of its current value

## Mapping

The **`map`** function allows you to transform the value of a **`@State`** property into another value. This is useful for creating derived states based on the original state.

Classic example with counter:
```swift
@State var count = 0

VStack {
    TextView($count.map { "Count: \($0)" })
    Button("Increment") {
        count += 1
    }
}
```
As you can see you can't use just string interpolation `"Count: \(count)"` with **`@State`** properties. Instead, you need to use **`map`** to transform the integer value into a string.

Example with boolean:
```swift
@State var enabled = true

VStack {
    TextView($enabled.map { $0 ? "Enabled" : "Disabled" })
    Button("Toggle") {
        enabled.toggle()
    }
}
```
Even though the state holds a boolean, you can show a string that depends on it.

More complex example with enum:
```swift
enum Status: String {
    case active = "Active"
    case inactive = "Inactive"
}

@State var active: Status = .active

TextView($active.map { $0.rawValue })
    .textColor($active.map { status in
        switch status {
        case .active: return .green
        case .inactive: return .red
        }
    })
    // or shorter
    .textColor($active.map { $0 == .active ? .green : .red })
```

Here when status changes, both the text and text color will update accordingly.

## Mapping two states

The **`and`** function allows you to combine two **`@State`** properties into a single derived state. This is useful when you want to create a new state based on the logical relationship between two existing states.

```swift
@State var one = true
@State var two = true

TextView("Hey")
    .visibility($one.and($two).map { one, two in
        // returns .visible if both one and two are true
        one && two ? .visible : .gone
    })
```
Here the visibility of the **`TextView`** will be updated based on the values of both **`one`** and **`two`** states, showing the view only when both are **`true`**.

## Mapping more

You can chain multiple **`and`** calls to combine more than two **`@State`** properties.

```swift
@State var a = true
@State var b = true
@State var c = 15

TextView("Hey")
    .visibility($a.and($b).and(c).map { a, b, c in
        // returns .visible if a is true and b is true and c is 15
        a && b && c == 15 ? .visible : .gone
    })
```
Combine up to 7 states this way.

Or infinitely many using nested maps:
```swift
@State var a = true
@State var b = true
@State var c = 15

TextView("Hey")
    .visibility($a.and($b).map { a, b in
        // returns true if both a and b are true
        a && b
    }
    // here ab is a result of the previous mapping
    .and($c).map { ab, c in
        // returns .visible if ab is true and c is 15
        ab && c == 15 ? .visible : .gone
    })
```

## Merge

The **`merge`** function allows you to combine the value of a **`@State`** property with another **`@State`** property.  
This is useful when you want to synchronize the values of two states.

```swift
class SomeView: View {
    @State var active: Bool = false

    override var body: any Body {
        SomeSubview()
            .enabled($active)
    }
}
class SomeSubview: View {
    @State var enabled: Bool = true

    func enabled(_ state: State<Bool>) -> Self {
        $enabled.merge(with: state).hold(in: self)
        return self
    }

    override var body: any Body {
        TextView($enabled.map { $0 ? "Enabled" : "Disabled" })
    }
}
```

In this example, when the **`active`** state in **`SomeView`** changes, it will automatically update the **`enabled`** state in **`SomeSubview`**, and if **`SomeSubview`** changes its own **`enabled`** state, it will update the external **`active`** state as well. So both states stay in sync, creating a two-way binding.

Merge operation generates two listeners which you should hold to control their lifecycle. In the example above we use `.hold(in: self)` to attach them to the **`SomeSubview`** instance. So when the subview is destroyed, the listeners are released automatically.  
Read more about that in [→ management](#management)

If you want to destroy these listeners earlier you can pass `TempStatesHolder` to the `hold(in:)` method. Store this holder where appropriate, listeners will be released when the holder is deallocated or when you call `releaseStates()` on it.

## Reset

You can reset a **`@State`** property to its initial value using the **`reset()`** method.

```swift
@State var counter: Int = 0
/// ... some code that changes counter ...
counter.reset() // counter is now 0
```

## Listeners

The **`listen`** function allows you to observe changes to a **`@State`** property. This is useful for executing custom logic whenever the state changes.

You can listen to the new value of the state, or both the old and new values, depending on your requirements.

```swift
enum Countries {
    case australia, mexico, brazil
}

@State var selectedCountry: Countries = .australia

$selectedCountry.listen {
    Log.i("Country changed")
}
$selectedCountry.listen { newValue in
    Log.i("Country changed to \(newValue)")
}
$selectedCountry.listen { oldValue, newValue in
    Log.i("Country changed from \(oldValue) to \(newValue)")
}
```

**Listening Only When the Value Actually Changes**

In many scenarios, a `@State` property may be set to the same value multiple times, either because of UI updates, background synchronization, or simply the way the component logic is structured.

Calling listeners for value assignments that don’t actually change anything can lead to unnecessary work, redundant UI updates, or wasted processing.

If the value conforms to `Equatable`, you can use the **`listenDistinct`** method to observe state changes only when the value is truly different.

```swift
// Only called when the value actually changes
$selectedCountry.listenDistinct { newValue in
    Log.i("Country changed to \(newValue)")
}
$selectedCountry.listenDistinct { oldValue, newValue in
    Log.i("Country changed from \(oldValue) to \(newValue)")
}
```
If `selectedCountry` is set to `.australia` while it is already `.australia`, the `listenDistinct` will not fire, while the regular `listen` method would still be called.

!!! tip ""
    Avoid using **`self`** directly inside listener closures to prevent retain cycles. Use **`[weak self]`** instead.

### Management

When you attach a listener to a `@State` value using `.listen { ... }`, the call returns a `StateListener` object. This object represents the connection between the state and the listener you just created and can be controlled either manually or automatically with `StatesHolder`.

```swift
let listener = $counter.listen { newValue in
    Log.i("Counter changed: \(newValue)")
}
```

By default, listeners stay alive as long as the `@State` exists.
This is great for global state, but in other cases without control **it can easily lead to memory leaks**.

#### Built-in Cleanup

The good news is that when you build UI using **framework-provided views**, you **do not need to manage listeners yourself**. All built-in views such as `TextView`, `Button`, `EditText`, etc., already know how to:

- attach listeners to the states they receive
- call `.hold(in: self)` internally
- automatically clean everything up when the view is destroyed

This means that code like:

```swift
@State var counter: Int = 0

override var body: any Body {
    TextView($counter.map { "Count: \($0)" })
}
```
**requires absolutely no additional work**.

Here `TextView` internally attaches a listener to `$counter` and holds it in the view instance.  
When the `TextView` object is deallocated, the listener is released automatically.

!!! success "**TL;DR**"
    No need to think about `StatesHolder`, `.hold(in:)`, or `releaseStates()` when using standard Droid views.

#### Automatic Cleanup

Every view or object that wants to own **`@State`** should conform to `StatesHolder`.

If you write a custom view class that **accepts a** `State` and wants to react to it, then you must explicitly hold the listener. `View` already conforms to `StatesHolder`, so you can use `self` to hold the listener.

```swift
class CustomView: View {
    init(id: Int32? = nil, _ state: State<String>) {
        super.init(id: id)
        state.listenDistinct { [weak self] old, new in
            guard old != new else { return }
            self?.updateLabel(new)
        }
        .hold(in: self) // required: attach listener to this view
    }

    private func updateLabel(_ newValue: String) {
        // Update Android UI here
    }
}
```
How it works:

- `.hold(in: self)` stores the listener inside your view
- when your view is destroyed, `deinit` calls `releaseStates()`

If you create another class that own listeners (for example, a controller, service, or lifetime-managed object), you should also conform it to `StatesHolder`. In that case, you are responsible for calling `releaseStates()` in its `deinit`, just like `View` does internally:

```swift
final class MyController: StatesHolder {
    let statesValues = StatesHolderValuesBox()

    deinit { 
        releaseStates() // required
    }
}
```
Now you can safely store listeners inside it.

#### Manual Cleanup

If you want manual control, you can keep the returned `StateListener` and call `.cancel()` yourself:

```swift
let listener = $status.listen { new in
    Log.i("Status changed: \(new)")
}
// ...
listener.cancel() // manually disconnect the listener
```
This can be useful in cases where you want to:

- remove a listener at a specific moment
- swap listeners dynamically

But for view lifecycles, automatic cleanup through `StatesHolder` is strongly recommended.

Additionally you can remove all listeners attached to a **`@State`** property using the **`removeListeners()`** method.

```swift
@State var value: String = "Initial"
/// ... some code that adds listeners to value ...
value.removeListeners() // All listeners are now removed
```

This may be useful if you don't want to keep references to certain listeners and want to clear them all at once.

## Universal Value

Make your methods accept both regular values and **`@State`** references using the **`StateValuable`** protocol.

Instead of writing two methods:
```swift
func text(_ value: String) -> Self
func text(_ state: State<String>) -> Self
```
You write just one:
```swift
func text<S: StateValuable>(_ value: S) where S.Value == String
```
For example, consider a custom `TitleView`. You want to set the title as either a **`String`** or a **`State<String>`**. Instead of writing two separate methods, you can use `StateValuable` to handle both cases in one method:

```swift
class TitleView: View {
    func text<S>(_ value: S) where S: StateValuable, S.Value == String {
        // Set the initial value (for both static and state values)
        updateLabel(value.simpleValue)
        // Listen for updates if the value is a State
        value.stateValue?.listenDistinct { [weak self] new in
            self?.updateLabel(new)
        }.hold(in: self)
    }
}
```

Now your view can be used in both ways:

```swift
TitleView()
    .text("Hello")    // static
    .text($titleText) // dynamic
```

This approach keeps your code clean and maintainable while providing you with the desired flexibility.

!!! success ""
    Base types like `String`, `Int`, `Bool`, etc., already conform to `StateValuable`

### Conforming Custom Types

If you have a custom type, you can make it conform to `StateValuable` as follows:

```swift
extension CustomType: StateValuable {
    public var simpleValue: CustomType { self }
    public var stateValue: State<CustomType>? { nil }
}
```

This also ensures that `State<CustomType>` automatically conforms to `StateValuable`.
