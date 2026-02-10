# SlidingPaneLayout

`SlidingPaneLayout` wraps AndroidX `SlidingPaneLayout` as a Droid `ViewGroup`.

It is useful for two-pane screen structures where AndroidX owns the sliding-pane container and Droid builds the child hierarchy.

## Use it when…

Use it for layouts that conceptually have a primary pane and a secondary pane, such as a list and details screen.

## Build two panes

```swift
body {
    SlidingPaneLayout(id: 200) {
        FrameLayout(id: 201)
        FrameLayout(id: 202)
    }
    .width(.matchParent)
    .height(.matchParent)
}
```

The wrapper uses AndroidX `SlidingPaneLayout.LayoutParams` and exposes `weight` as an applicable layout parameter.

## Current limitations

The current Droid type exposes construction, child content, inherited `ViewGroup` behavior, and the available layout-parameter surface.

It does not currently expose SlidingPaneLayout-specific open/close state, lock mode, pane listeners, or transition helpers.
