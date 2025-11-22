# Dimension Units

Android supports several dimension units for specifying sizes, margins, paddings, and other measurements in your app's UI.

All the methods that expect dimension values in **Droid** framework assume `dp` (density-independent pixels) by default:

```swift
.width(16) // 16dp
.padding(8) // 8dp
.padding(h: 8, v: 16) // 8dp, 16dp
```
However, you can specify other units explicitly:
```swift
.width(16, .px) // 16 pixels
.padding(8, .sp) // 8 scale-independent pixels
.padding(h: 8, v: 16, .in) // 8 inches, 16 inches
```

The supported units are in `DimensionUnit` enum:

- **`dp`** - density-independent pixels. It is a virtual unit of measurement that adjusts based on the screen density, ensuring consistent sizing across different devices. **Best choice.**
- **`px`** - represents pixels, which are the smallest unit of measurement on a screen. It is an absolute unit and does not scale with screen density, making it less suitable for responsive designs.
- **`sp`** - scale-independent pixels. Similar to `dp`, but also scales based on the user's font size preferences. It is primarily used for text sizes to ensure accessibility and readability.
- **`pt`** - points. A physical unit of measurement equal to 1/72 of an inch. It is commonly used in typography and can vary in appearance depending on the screen density.
- **`in`** - inches. Represents a physical measurement of length. It is an absolute unit and is rarely used in UI design due to its lack of adaptability to different screen sizes and densities.
- **`mm`** - millimeters. Another physical unit of measurement. Like `in`, it is absolute and not commonly used in responsive design.

This enum also provides you with conversion methods **`to`** and **`from`** pixels:
```swift
let pixels: Int32 = DimensionUnit.dp.toPixels(16) // convert 16dp to pixels
let dp: Float = DimensionUnit.dp.fromPixels(48) // convert 48 pixels to dp
```

