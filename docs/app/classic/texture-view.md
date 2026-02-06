# TextureView

`TextureView` displays content through an Android texture while still participating in the normal view hierarchy.

That makes it useful when rendered content needs ordinary view transforms or composition. When a dedicated surface is the priority, consider `SurfaceView`.

## Use it when…

Use `TextureView` for rendered content that needs to move, scale, fade, rotate, or otherwise behave like a normal view in your layout.

## How it behaves

The texture is composited as part of the view hierarchy, so inherited view APIs control size and placement normally.

The current API exposes the base view surface only; texture-listener and lower-level rendering callbacks are not exposed on this type yet.

## Examples

### Create a preview tile

```swift
TextureView()
    .width(160)
    .height(90)
```

### Put an overlay above the texture

```swift
FrameLayout {
    TextureView()
        .width(.matchParent)
        .height(220)

    TextView("LIVE")
        .padding(h: 8, v: 4)
        .gravityInParent([.top, .start])
        .margin(16)
}
```

## Initializers

`TextureView` uses the inherited view initializer:

```swift
TextureView()
TextureView(id: 1001)
```
