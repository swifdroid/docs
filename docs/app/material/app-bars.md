# AppBarLayout

`AppBarLayout` is the “top strip” of a Material screen that can collapse, snap, and react to scrolling content.
It usually hosts a toolbar, a large title, tabs, or a search field — anything that should feel connected to the scroll.

In Droid, `AppBarLayout` shines when it lives inside a `CoordinatorLayout`.
That pairing is what turns it from “a vertical stack” into “a collapsing app bar”.

## Use it when…

Use it when the top of the screen should move with the content: collapse away as you scroll down, then return quickly when you scroll up.

Use it when you want the scroll view below to drive the app bar’s motion instead of you manually hiding/showing views.

## How it behaves

`AppBarLayout` lays out its children vertically.
Each child can opt into scrolling behavior via layout params like `scrollFlags(...)` and `scrollEffect(...)`.

For the scroll choreography to work, `AppBarLayout` should be a direct child of `CoordinatorLayout`, and your scrolling content should use `AppBarLayout.ScrollingViewBehavior()` as its `behavior(...)`.
If you skip that, `AppBarLayout` still renders — it just won’t respond to scroll.

## Examples

### Pinned top bar

```swift
body {
    AppBarLayout {
        MaterialToolbar()
            .title("Settings")
    }
    .width(.matchParent)
    .height(.wrapContent)
}
```

### Collapsing app bar with scrolling content

```swift
import Droid

final class FeedActivity: AppCompatActivity {
    override func buildUI() {
        super.buildUI()
        body {
            CoordinatorLayout {
                AppBarLayout {
                    MaterialToolbar()
                        .title("Inbox")
                        .scrollFlags(.scroll, .enterAlways)

                    TextView("Today")
                        .textSize(20)
                        .padding(16)
                        .scrollFlags(.scroll, .exitUntilCollapsed)
                        .scrollEffect(.compress)
                }
                .width(.matchParent)
                .height(.wrapContent)

                NestedScrollView {
                    LinearLayout {
                        ForEach(0..<40) { i in
                            TextView("Message #\(i)")
                                .padding(16)
                        }
                    }
                    .orientation(.vertical)
                }
                .width(.matchParent)
                .height(.matchParent)
                .behavior(AppBarLayout.ScrollingViewBehavior())
            }
        }
    }
}
```

### Snapping on scroll end

```swift
body {
    CoordinatorLayout {
        AppBarLayout {
            MaterialToolbar()
                .title("Library")
                .scrollFlags(.scroll, .snap)
        }

        NestedScrollView {
            LinearLayout {
                ForEach(0..<20) { i in
                    TextView("Book \(i)")
                        .padding(16)
                }
            }
            .orientation(.vertical)
        }
        .behavior(AppBarLayout.ScrollingViewBehavior())
    }
}
```

## Layout params

These params apply to views *inside* an `AppBarLayout`:

- `scrollFlags(...)` chooses how a child enters/exits (scroll, snap, quick return). Cases: `.scroll`, `.enterAlways`, `.enterAlwaysCollapsed`, `.exitUntilCollapsed`, `.snap`, `.snapMargins`, `.noScroll`.
- `scrollEffect(...)` adds `.none` or `.compress` for large titles.
- `scrollInterpolator(...)` controls the collapse animation feel.

```swift
AppBarLayout {
    MaterialToolbar()
        .title("Inbox")
        .scrollFlags(.scroll, .enterAlways)

    TextView("Big header")
        .textSize(20)
        .padding(16)
        .scrollFlags(.scroll, .exitUntilCollapsed)
        .scrollEffect(.compress)
}
```

`AppBarLayout.ScrollingViewBehavior` is the coordinator behavior for the scrolling content:

```swift
NestedScrollView {
    // …
}
.behavior(AppBarLayout.ScrollingViewBehavior())
```

That behavior also exposes `overlayTop(_:)`, `leftAndRightOffset(_:)`, `topAndBottomOffset(_:)`, `verticalOffsetEnabled(_:)`, and `horizontalOffsetEnabled(_:)`.

`AppBarLayout` itself accepts `init(id:)` and a single-child content initializer, plus `scrollEffect(_:)` / `scrollFlags(_:)` / `scrollInterpolator(_:)` on the bar when that is clearer than child layout params.

## Notes

`AppBarLayout` is happiest as a direct child of `CoordinatorLayout`. If you nest it inside other containers, the scroll behavior usually stops working.

`AppBarLayout.LayoutParams` field getters (`scrollEffect`, `scrollFlags`, `scrollInterpolator` as layout-param fields) are not wrapped at this pin — use the `View` helpers above. Nested `ChildScrollEffect` / `CompressChildScrollEffect` / `LiftOnScrollProgressListener` types exist for advanced integrations; do not assume full listener wiring.

