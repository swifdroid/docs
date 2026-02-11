# FragmentActivity

`FragmentActivity` is Droid's AndroidX activity base for screens that host support fragments.

It extends `ComponentActivity` and adds access to the AndroidX `FragmentManager`.

## Use it when…

Use `FragmentActivity` when an activity needs to host or manage AndroidX fragments without requiring an AppCompat-specific activity API.

## Access the FragmentManager

```swift
final class MainActivity: FragmentActivity {
    override func onStart() {
        super.onStart()

        if let manager = supportFragmentManager() {
            print(manager.backStackEntryCount())
        }
    }
}
```

`supportFragmentManager()` returns the manager associated with the activity.

From that manager you can begin fragment transactions, query fragments, inspect the back stack, and use the other FragmentManager APIs currently exposed by Droid.

## Starting activity work from a fragment

`FragmentActivity` also exposes `startActivityFromFragment` overloads that forward an `Intent` on behalf of a fragment.

Use those only when you specifically need the AndroidX FragmentActivity behavior; ordinary activity launches can continue to use the normal activity or fragment APIs.
