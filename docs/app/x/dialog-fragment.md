# DialogFragment

`DialogFragment` wraps AndroidX `DialogFragment` and inherits Droid's `Fragment` behavior.

Use it for dialog UI that should participate in the fragment lifecycle and be managed by a `FragmentManager`.

## Use it when…

Use a `DialogFragment` when a dialog belongs to fragment navigation or lifecycle state rather than being managed as a standalone `Dialog`.

## Show a dialog fragment

Pass your dialog-fragment instance and an activity's FragmentManager:

```swift
func showInfo(
    _ dialog: DialogFragment,
    from activity: FragmentActivity
) {
    dialog.cancelable(false)
    dialog.show(activity.supportFragmentManager(), "info")
}
```

You can dismiss it through the fragment API:

```swift
dialog.dismiss()
```

`dismissAllowingStateLoss()` and `dismissNow()` are also available when their AndroidX transaction semantics are what you need.

## Dialog state and style

The current wrapper lets you inspect the controlled `Dialog`, theme, cancelable state, and whether the fragment is configured to show as a dialog.

You can also configure:

- `cancelable(_:)`
- `showsDialog(_:)`
- `style(_:_:)`
- `show(_:_:)`
- `showNow(_:_:)`

## Current limitations

The wrapper does not currently implement the DialogFragment-specific `on…` callback surface through Droid's native Kotlin bridge.

Use the APIs that are explicitly exposed on the type rather than assuming every AndroidX DialogFragment callback is wrapped.
