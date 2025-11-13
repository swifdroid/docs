# Colors

Android provides us with a rich palette of colors to enhance the visual appeal of our applications [→ official docs](https://developer.android.com/reference/android/graphics/Color)

With **Droid** framework, you can easily access and utilize these colors through the **`GraphicsColor`** struct.

You can set colors for various UI components using the predefined color properties or by directly specifying hex color values.

```swift
TextView("Hello, World!")
    .textColor(.red)            // Using predefined red color
    .backgroundColor(0xFF00FF)  // Using hex value for green color
```

## Hexadecimal

You can specify colors using hexadecimal values, following the Android convention.

The format is `0xAARRGGBB`, where `AA` defines the alpha (transparency), `RR` is red, `GG` is green, and `BB` is blue.

If the alpha component is omitted (`0xRRGGBB`), it is automatically set to `FF`, meaning the color is fully opaque.

Shorter forms are also supported: `0xRGB` (12-bit) and `0xARGB` (16-bit). These are expanded internally to 8-bit per component values.

```swift
TextView("Hello, World!")
    // Opaque blue
    .textColor(0xFF0000FF)
    // Semi-transparent red
    .textColor(0x80FF0000)
    // Opaque orange
    .textColor(.fromHex(0xFFFFA500))
    // 12-bit shorthand (expands to opaque pinkish tone)
    .textColor(.fromHex(0xF0A))
    // 16-bit shorthand with alpha
    .textColor(.fromHex(0x8F0A))
```

## RGB and ARGB

You can create custom colors using RGB and ARGB values.

The `GraphicsColor` struct provides initializers for both.

```swift
TextView("Hello, World!")
    // Bright red color
    .textColor(.from(red: 255, green: 0, blue: 0))
    // Dark green color
    .textColor(.from(red: 0, green: 128, blue: 0))
    // Semi-transparent blue color
    .textColor(.from(red: 0, green: 0, blue: 255, alpha: 128))
```

## Predefined Colors

```swift
.transparent    // 0x00000000
.black          // 0xFF000000
.white          // 0xFFFFFFFF
.red            // 0xFFFF0000
.green          // 0xFF00FF00
.blue           // 0xFF0000FF
.yellow         // 0xFFFFFF00
.cyan           // 0xFF00FFFF
.magenta        // 0xFFFF00FF
.gray           // 0xFF808080
.darkGray       // 0xFF444444
.lightGray      // 0xFFCCCCCC
.maroon         // 0xFF800000
.olive          // 0xFF808000
.navy           // 0xFF000080
.teal           // 0xFF008080
.purple         // 0xFF800080
.silver         // 0xFFC0C0C0
.lime           // 0xFF00FF00
.aqua           // 0xFF00FFFF
.fuchsia        // 0xFFFF00FF
.orange         // 0xFFFFA500
.brown          // 0xFFA52A2A
.gold           // 0xFFFFD700
.indigo         // 0xFF4B0082
.violet         // 0xFF8A2BE2
.pink           // 0xFFFFC0CB
```