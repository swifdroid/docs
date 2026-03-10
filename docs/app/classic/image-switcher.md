# ImageSwitcher

`ImageSwitcher` specializes `ViewSwitcher` for animated image changes. The current wrapper adds queued drawable, resource, and URI image setters while inheriting the Android view-factory bridge and `ViewAnimator` transition controls.

## Use it when…

Use `ImageSwitcher` when you want Android's two-view image-switching behavior with reusable enter/exit animations.

For a single non-switching image, use `ImageView`.

## How it behaves

`imageDrawable(_:)`, `imageResource(_:)`, and `imageURI(_:)` are queued Droid view properties.

The inherited `factory(_:)`, displayed-child, and animation configuration remain available.

## Examples

### Configure an image resource

```swift
ImageSwitcher()
    .imageResource(1001)
    .animateFirstView(false)
```

### Configure a drawable

```swift
let drawable: Drawable? = nil

ImageSwitcher()
    .imageDrawable(drawable)
```

## Initializers

```swift
ImageSwitcher()
ImageSwitcher(id: 1001)
```

## Methods

### Drawable

`imageDrawable(_:)` queues an optional Android `Drawable` for the switcher.

### Resource ID

`imageResource(_:)` queues an Android drawable resource ID.

### URI

`imageURI(_:)` queues an optional Android `Uri`.

### Inherited factory and animator controls

`ImageSwitcher` inherits `factory(_:)` from `ViewSwitcher`, plus displayed-child selection, manual next/previous switching, first-view animation control, enter/exit animations, and child-management helpers from `ViewAnimator`.

The factory accepts a JNI-compatible Android object; Droid does not construct it for you.
