# NumberPicker

`NumberPicker` is Android's wheel-style control for choosing a value from a bounded range. The current Droid wrapper exposes min/max/value configuration, wrapping, displayed labels, text appearance, long-press timing, enabled state, and listener bridges.

## Use it when…

Use it when users choose from a modest numeric range or from a short list of labels that map cleanly onto consecutive values.

## How it behaves

`minValue(_:)`, `maxValue(_:)`, `value(_:)`, `wrapSelectorWheel(_:)`, displayed values, text appearance, enabled state, long-press interval, formatter, and listener setters are queued Droid view properties.

Getter methods and event/lifecycle helpers operate on the live Android instance.

## Examples

### Pick a quantity

```swift
NumberPicker()
    .minValue(1)
    .maxValue(10)
    .value(1)
    .wrapSelectorWheel(false)
```

### Use labels instead of raw numbers

```swift
NumberPicker()
    .minValue(0)
    .maxValue(2)
    .displayedValues(["Small", "Medium", "Large"])
    .value(1)
```

## Initializers

```swift
NumberPicker()
NumberPicker(id: 1001)
```

## Methods

### Range and current value

`minValue(_:)`, `maxValue(_:)`, and `value(_:)` configure the picker while it is being built.

`minValue()`, `maxValue()`, and `value()` read the current Android values.

### Wrapping

`wrapSelectorWheel(_:)` controls whether moving past an end wraps to the other side. `wrapSelectorWheel()` reads the live setting.

### Displayed labels

`displayedValues(_:)` queues a Swift string array and forwards it to Android as the displayed labels.

`displayedValues()` exposes the current Android string-array object as `JObject?`.

### Appearance and interaction

`selectionDividerHeight(_:)`, `textColor(_:)`, `textSize(_:)`, `setEnabled(_:)`, and `onLongPressUpdateInterval(_:)` configure presentation and behavior.

Getter methods expose selection-divider height, text color/size, solid color, fading-edge strengths, and related live state.

### Formatter and listeners

`formatter(_:)`, `onScrollListener(_:)`, and `onValueChangedListener(_:)` accept JNI-compatible Android objects and queue them for the picker.

Droid exposes those bridges but does not construct formatter/listener objects for you.

### Low-level helpers

Scroll computation, click/long-click, touch/key/hover/trackball dispatch, accessibility, drawing, layout/measurement, and drawable-resolution hooks are available for lower-level integrations.