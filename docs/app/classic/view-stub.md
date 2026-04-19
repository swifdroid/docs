# ViewStub

`ViewStub` is a zero-size placeholder that inflates a real layout only when you ask for it. Until then it costs almost nothing on screen, so heavy panels, error states, and rare UI branches can stay out of the first layout pass.

Droid exposes the stub as a `View` you can place in a hierarchy, point at a layout resource, and later replace with the inflated content. After `inflate()`, the stub is gone from the tree and you get the live inflated view back.

Use it when a subtree is optional or expensive. Skip it when the UI is always visible — a normal child is simpler.

## Use it when…

Use `ViewStub` when a panel, error block, or advanced controls should not be built until the user actually needs them.

Do not use it for content that appears immediately on every screen. For always-visible UI, add the views directly.

## How it behaves

The stub occupies no meaningful space until inflation. `layoutResource(_:)` names the layout to build. `inflatedId(_:)` names the id Android assigns to the inflated root so later lookups still work.

`inflate()` builds that layout, replaces the stub in its parent, and returns the new `View?`. Calling inflate again does not create a second copy — the stub has already been consumed.

Read `layoutResource()` / `inflatedId()` back when you need to confirm configuration before inflating.

## Examples

### Defer a heavy settings panel

```swift
let stub = ViewStub(id: 3001)
    .layoutResource(R.layout.settings_panel)
    .inflatedId(3002)

// later, when the user opens settings
if let panel = stub.inflate() {
    // settings_panel is now in the hierarchy
}
```

The screen stays light until `inflate()` runs.

### Keep a stable id after inflation

```swift
let stub = ViewStub(id: 3001)
    .layoutResource(R.layout.error_banner)
    .inflatedId(4100)

let banner = stub.inflate()
```

Android gives the inflated root id `4100`, so other code can still find it after the stub is gone.

## Initializers

```swift
ViewStub()

ViewStub(id: 3001)

ViewStub(id: 3001) {
    // optional placeholder content
}
```

Most apps only need the id form plus `layoutResource` / `inflatedId`.

## Methods

### Point at a layout

`layoutResource(_:)` sets the resource id to inflate. `layoutResource()` reads the current value.

```swift
stub.layoutResource(R.layout.settings_panel)
let res = stub.layoutResource()
```

Set the resource before the first `inflate()`.

### Assign the inflated id

`inflatedId(_:)` sets the id of the view that replaces the stub. `inflatedId()` reads it.

```swift
stub.inflatedId(4100)
let id = stub.inflatedId()
```

Use this when later code looks up the inflated root by id.

### Inflate the stub

`inflate()` builds the layout, replaces the stub, and returns the inflated `View?`.

```swift
if let panel = stub.inflate() {
    // use the live view
}
```

Inflation is a one-shot replacement. Do not expect the stub to remain in the hierarchy afterward.

## Notes

Point `layoutResource` at a real layout id before calling `inflate()`. If inflation fails, the method returns `nil` and the stub stays in place so you can recover or retry after fixing the resource.
