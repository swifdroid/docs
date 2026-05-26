# FragmentActivity

`FragmentActivity` is a [ComponentActivity](component-activity.md)-based activity that can host support fragments. Use it to give a screen a fragment-managed content area.

The activity owns the fragment manager; each [Fragment](fragments.md) supplies its own content. A [FragmentContainerView](fragment-container-view.md) provides a place for that content in the activity's layout.

## Use it when…

Use `FragmentActivity` when a screen needs to install or manage fragments and does not need AppCompat-specific activity features.

## How it behaves

`supportFragmentManager()` returns the manager associated with this activity as `FragmentManager!`, an implicitly unwrapped optional. It can return `nil` when the activity context is unavailable or the Java class or method lookup fails. Unwrap it before starting a transaction.

Create and commit transactions before the activity saves its state. `commit()` schedules the change on the main thread; the fragment does not appear synchronously when that call returns.

## Examples

### Show a welcome fragment

This activity creates a full-size fragment container, then installs a fragment displaying “Welcome to your library”. The shared container ID connects the transaction to the view. If the manager already has a fragment for that ID, the example leaves it in place instead of adding another one.

```swift
import Droid

final class LibraryActivity: FragmentActivity {
    private let contentId: Int32 = 1001

    override func onCreate(
        _ context: ActivityContext,
        savedInstanceState: Bundle?
    ) {
        super.onCreate(context, savedInstanceState: savedInstanceState)

        contentView(
            FragmentContainerView(id: contentId)
                .width(.matchParent)
                .height(.matchParent)
        )

        guard let manager = supportFragmentManager(),
              manager.findFragmentById(contentId) == nil,
              let fragment = Fragment(),
              let transaction = manager.beginTransaction()
        else { return }

        fragment.body {
            TextView().textKeepState("Welcome to your library")
        }

        guard let addition = transaction.add(
            containerViewId: contentId,
            fragment: fragment
        ) else { return }

        addition.commit()
    }
}
```

For fragment content and further transaction operations, continue with the [fragment guide](fragments.md).

## Methods

### Get the activity's fragment manager

Use `supportFragmentManager()` to obtain the manager for this host. For example, a fragment operation can first check whether the host has already saved its state:

```swift
if let manager = supportFragmentManager() {
    let stateSaved = manager.isStateSaved()
    print("Library fragment state saved: \(stateSaved)")
}
```

### Launch an activity on behalf of a fragment

`startActivityFromFragment(_:_:requestCode:)` forwards a fragment, an `Intent`, and an `Int` request code to the activity. The second overload adds `options: Bundle`. Options are optional in the sense that you can choose the overload without them; the `options` parameter itself does not accept `nil`.

The source describes these as the host-side implementation of fragment launches for a result. It does not expose `Fragment.startActivityForResult()`, so these methods alone are not a complete fragment result-handling flow. Both return without launching if the activity context is unavailable.

Given an existing fragment and an intent configured for the activity to open, call one of the two forms on the host:

```swift
@MainActor
func launchFromFragment(
    host: FragmentActivity,
    fragment: Fragment,
    intent: Intent,
    options: Bundle?
) {
    if let options {
        host.startActivityFromFragment(
            fragment, intent, requestCode: 42, options: options
        )
    } else {
        host.startActivityFromFragment(fragment, intent, requestCode: 42)
    }
}
```
