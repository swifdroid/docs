# DialogFragment

`DialogFragment` displays a dialog window in front of its activity and lets a `FragmentManager` manage the fragment that owns it. Supply visible content through the inherited `body` builder, then show the fragment with a tag.

Control showing, hiding, and dismissing through the fragment APIs. Direct calls on its `Dialog` can bypass the fragment's control of the window; configure cancelability through `DialogFragment` too.

## Use it when…

Use it for a focused message or task that belongs in a dialog window while remaining managed as a fragment. For content embedded in the activity instead, follow the [fragment content and transaction flow](fragments.md).

## How it behaves

The manager-based `show` creates an add transaction with your tag and commits it without adding it to the back stack. Its commit is scheduled on the main thread. `showNow` adds immediately using `commitNow`, also without adding to the back stack. Dismissing either causes a new transaction to remove the fragment.

The transaction-based `show` adds the dialog fragment using your supplied transaction and commits that transaction. Do not commit it again yourself. Ordinary commits and synchronous commits must happen before the host activity saves its state.

Set `showsDialog(_:)` from `onCreate`; setting it later has no effect. Apply `style(_:_:)` before the dialog is created; changing it afterward has no effect.

DialogFragment-specific `on…` methods and the `requireDialog` / `requireComponentDialog` helpers are marked TODO and are not declared in Droid's DialogFragment source. Inherited `NativeFragment` hooks such as `onCreate(_:)` remain available. The example uses inherited fragment content rather than an `onCreateDialog` override.

## Examples

Pass the host's `FragmentManager` into this function. It creates a dialog containing a visible sync-status message, disables cancellation, and shows it under the `sync-status` tag. Keep the returned fragment if you need to dismiss it when the operation finishes.

```swift
import Droid

@MainActor
func showSyncStatus(using manager: FragmentManager) -> DialogFragment? {
    guard let dialog = DialogFragment() else { return nil }
    dialog.body {
        TextView("Syncing your notes. Please wait.")
    }
    dialog.cancelable(false)
    dialog.show(manager, "sync-status")
    return dialog
}
```

The snippets below use `dialog` for this fragment and `manager` for the supplied manager. Each showing snippet is an alternative way to present it.

## Methods

### Presenting the dialog

`show(_:_:)` with a `FragmentManager` adds the fragment with a tag and schedules its transaction without a back-stack entry.

```swift
dialog.show(manager, "sync-status")
```

`show(_:_:)` with a `FragmentTransaction` uses and commits that transaction, returning an `Int32` result. Start the transaction through the manager:

```swift
if let transaction = manager.beginTransaction() {
    let result = dialog.show(transaction, "sync-status")
}
```

`showNow(_:_:)` adds the fragment synchronously using `commitNow`, without a back-stack entry. Use it when the addition must finish before the call returns, before the activity has saved its state.

```swift
dialog.showNow(manager, "sync-status")
```

### Configuring the window

`cancelable(_:)` controls whether the shown dialog can be canceled. Use this setter so the fragment can adjust its own behavior.

```swift
dialog.cancelable(false)
```

`showsDialog(_:)` controls whether the fragment displays its content in a dialog window. Set it inside the inherited `onCreate(_:)` hook:

```swift
class SyncStatusDialog: DialogFragment {
    override func onCreate(_ savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        showsDialog(true)
    }
}
```

`style(_:_:)` selects a basic appearance and an `Int32` theme value before dialog creation. The styles are `.normal`, `.noTitle`, `.noFrame` (content draws the window), and `.noInput` (also disables input). This call requests no title area:

```swift
dialog.style(.noTitle, 0)
```

### Reading current state

`dialog()` returns the controlled `Dialog` as an optional. Inspect its presence without using it to show or dismiss the window.

```swift
let hasDialog = dialog.dialog() != nil
```

`showsDialog()` reads whether the fragment is configured to display a dialog.

```swift
let displaysDialog = dialog.showsDialog()
```

`theme()` returns the `Int32` theme value used to create the dialog.

```swift
let dialogTheme = dialog.theme()
```

`isCancelable()` reads whether the dialog is cancelable.

```swift
let canCancel = dialog.isCancelable()
```

### Dismissing the dialog

`dismiss()` dismisses the fragment and its dialog. Call it when the sync operation finishes.

```swift
dialog.dismiss()
```

`dismissAllowingStateLoss()` uses `commitAllowingStateLoss`, permitting the commit after activity state has been saved. The commit can be lost when the activity is restored, so use it only when an unexpected UI-state change is acceptable.

```swift
dialog.dismissAllowingStateLoss()
```

`dismissNow()` uses synchronous `commitNow`. It must run before the activity saves its state; it does not allow state loss.

```swift
dialog.dismissNow()
```
