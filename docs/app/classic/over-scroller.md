# OverScroller

`OverScroller` wraps Android's scroll/fling physics helper. It is not a `View`: the current Droid wrapper operates on an existing Android `OverScroller` object and exposes live scrolling, fling, spring-back, boundary, and state APIs.

## Use it when…

Use `OverScroller` when custom scrolling code needs Android's fling/deceleration calculations while your own view or container applies the resulting coordinates.

## How it behaves

Every current `OverScroller` method is a live JNI call.

The wrapper does not queue view properties and does not currently construct a new Android `OverScroller` from an `ActivityContext`.

## Existing-object initializer

```swift
OverScroller(existingObject)
```

`existingObject` must be the `JObject` for an already-created Android `OverScroller`.

## Driving a scroll

```swift
func advance(_ scroller: OverScroller) {
    scroller.startScroll(0, 0, 240, 0, 300)

    if scroller.computeScrollOffset() {
        let x = scroller.currX()
        let y = scroller.currY()
        // Apply x/y to your custom scrolling surface.
    }
}
```

`startScroll(_:_:_:_:)` uses Android's default duration. The five-argument overload also accepts an explicit duration in milliseconds.

Call `computeScrollOffset()` as your animation advances, then read `currX()` and `currY()`.

## Fling and spring-back

`fling(_:_:_:_:_:_:_:_:)` starts a velocity-driven fling inside X/Y bounds.

A second overload adds `overX` and `overY` allowances.

`springBack(_:_:_:_:_:_:)` asks Android to animate coordinates back into a valid range and returns whether a spring-back was started.

## State and endpoints

Live reads include:

- `startX()` / `startY()`
- `currX()` / `currY()`
- `finalX()` / `finalY()`
- `currVelocity()`
- `duration()`
- `timePassed()`
- `isFinished()`
- `isOverScrolled()`
- `isScrollingInDirection(_:_:)`

`finalX(_:)` and `finalY(_:)` change the live destination.

## Controlling an active animation

`abortAnimation()` stops the live animation.

`forceFinished(_:)` changes Android's finished flag.

`extendDuration(_:)` extends the current animation duration.

`friction(_:)` changes fling friction.

`notifyHorizontalEdgeReached(_:_:_:)` and `notifyVerticalEdgeReached(_:_:_:)` notify Android when custom scrolling reaches a boundary.