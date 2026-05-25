# Fragments

`Fragment` is a reusable part of a screen with its own view. An activity can host several fragments, and a fragment can host another fragment when a screen needs nested sections.

## Use it when…

Use a fragment when a screen has sections that should be created, replaced, or reused independently—for example, a list beside a detail pane or a settings area inside a larger flow.

Use a child fragment when one part of a screen needs its own fragment content area. Keep the container and the fragment manager in the same parent fragment so nested content stays within that screen section.

## How it behaves

Override `buildUI()` and set the fragment's view with `body { ... }`. Droid calls `buildUI()` while creating the view returned from `onCreateView`. If the body produces several sibling views, Droid wraps them in a `FrameLayout`; use an explicit layout such as `LinearLayout` when the children need a particular arrangement. Without a body, the current implementation displays a centered “Empty Fragment Content” placeholder.

Each fragment has a `parentFragmentManager()` for the manager associated with its host. A nested fragment can use `childFragmentManager()` to manage its own children. The host activity exposes its manager through [`FragmentActivity`](fragment-activity.md), and [`FragmentContainerView`](fragment-container-view.md) provides a fragment-specific content area.

At the current Droid source pin, the Swift wrappers expose only their implemented methods. The AndroidX [FragmentManager](https://developer.android.com/reference/androidx/fragment/app/FragmentManager) and [FragmentTransaction](https://developer.android.com/reference/androidx/fragment/app/FragmentTransaction) references explain those concepts; they do not describe the complete Swift API available through Droid.

## Examples

### Build a fragment view

This fragment displays two lines of account information in a vertical layout.

```swift
import Droid

final class AccountSummaryFragment: Fragment {
    override func buildUI() {
        body {
            LinearLayout {
                TextView("Account")
                TextView("Plan: Standard")
            }
            .orientation(.vertical)
        }
    }
}
```

### Show nested details

The parent owns a `FragmentContainerView`, then uses its child manager to replace the content after the parent view has been created.

```swift
import Droid

final class AccountHostFragment: Fragment {
    private let detailsContainerID: Int32 = 41

    override func buildUI() {
        let containerID = detailsContainerID
        body {
            FragmentContainerView(id: containerID)
        }
    }

    func showDetails(_ details: Fragment) {
        guard
            !isStateSaved(),
            let manager = childFragmentManager(),
            let transaction = manager.beginTransaction()
        else { return }

        transaction
            .replace(containerViewId: detailsContainerID, fragment: details)?
            .commit()
    }
}
```

Use the same `detailsContainerID` for `FragmentContainerView` and `replace`; it identifies the target container. Do not begin or commit the transaction after the host has saved its state.

## Methods

### Build the view

`buildUI()` is the hook Droid calls while creating the fragment's view. Use `body { ... }` to provide one root view; put multiple arranged children inside a layout.

```swift
override func buildUI() {
    body {
        TextView("Profile")
    }
}
```

### Choose the fragment manager

Use `parentFragmentManager()` to work with the manager that owns this fragment. Use `childFragmentManager()` for content nested inside it. Both return the manager used to begin a `FragmentTransaction`; the example above shows a focused `replace` flow rather than a full transaction catalog.

Use the parent manager when this fragment should be replaced in its host's container:

```swift
func replaceThisSection(with next: Fragment, in containerID: Int32) {
    guard
        !isStateSaved(),
        let transaction = parentFragmentManager()?.beginTransaction()
    else { return }

    transaction.replace(containerViewId: containerID, fragment: next)?.commit()
}
```

### Check the host and view state

`activity()`, `context()`, `view()`, `arguments()`, `host()`, and `parentFragment()` return optional values. The corresponding `requireActivity()`, `requireContext()`, `requireView()`, `requireArguments()`, `requireHost()`, and `requireParentFragment()` methods use implicitly unwrapped optionals at the Swift boundary. Use the optional accessors while detachment or missing arguments are possible.

The state queries include `isAdded()`, `isDetached()`, `isHidden()`, `isInLayout()`, `isRemoving()`, `isResumed()`, `isStateSaved()`, and `isVisible()`. Check `isStateSaved()` before changing fragment state.

### Supply arguments

Call `arguments(_:)` before the fragment is added, or before its manager has saved state. The bundle is retained across fragment destruction and recreation; `arguments()` reads it back as an optional value.

```swift
func configureBeforeAdding(_ bundle: Bundle) {
    arguments(bundle)
}
```

### Coordinate transitions

`postponeEnterTransition()` delays an entering fragment transition. Call it before adding the fragment, or during `onCreate`, `onAttach`, or `onCreateView`; call `startPostponedEnterTransition()` when the content is ready.

```swift
func beginLoadingContent() {
    postponeEnterTransition()
}

func contentDidLoad() {
    startPostponedEnterTransition()
}
```

The wrapper also exposes enter/return overlap controls and transition getters and setters. For example, `allowEnterTransitionOverlap(false)` waits for the exit transition to finish before starting the enter transition.

### Start an activity

`startActivity(_:)` starts an intent from the fragment. The overload with `options:` accepts a `Bundle` of launch options.

```swift
func open(_ intent: Intent, options: Bundle) {
    guard isAdded(), activity() != nil else { return }
    startActivity(intent, options: options)
}
```

## Notes

- `shouldShowRequestPermissionRationale(permission:)` returns `true` from its default implementation at this source pin. Do not use that default to infer whether Android has previously denied a permission.
- `FragmentManager.beginTransaction()` must be used before the host saves its state. `isStateSaved()` is available for a guard; `commitAllowingStateLoss()` exists but its source documentation warns that the transaction may be lost and the UI may change unexpectedly.
