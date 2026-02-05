# SurfaceView

`SurfaceView` provides a dedicated Android drawing surface. It is useful for content such as camera previews, video output, or custom rendering that benefits from a separate surface.

## Use it when…

Use `SurfaceView` when the rendered content should live on a dedicated surface and does not need to behave exactly like ordinary composited UI.

If transforms and view-hierarchy composition matter more than the dedicated-surface model, consider `TextureView`.

## How it behaves

A SurfaceView is backed by a separate surface layer. Normal inherited view APIs still control the wrapper's size and placement, while the rendered surface has its own lifecycle and rendering concerns.

Surface-holder callbacks are not exposed on this type yet.

## Examples

### Reserve a preview area

```swift
SurfaceView()
    .width(.matchParent)
    .height(220)
```

### Place controls over a surface

```swift
FrameLayout {
    SurfaceView()
        .width(.matchParent)
        .height(220)

    Button("Capture")
        .gravityInParent([.bottom, .end])
        .margin(16)
}
```

## Initializers

`SurfaceView` uses the inherited view initializer:

```swift
SurfaceView()
SurfaceView(id: 1001)
```
