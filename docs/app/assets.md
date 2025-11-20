# Assets

In an Android app project, resources are non-code files and static content used by the application, stored primarily in the `res` directory.

Externalizing these resources from the code allows for independent maintenance and the provision of alternative resources for different device configurations (e.g., different languages, screen sizes, or densities). 

Resources management should be done in the `res` folder of your Android app project which is located at `Application/<target>/src/main/res` where `<target>` is the name of your target (e.g., `androidapp`).

## Types

The main types of resources, organized by their subdirectory within res/, are:

- **`animator`**: XML files that define property animations
- **`anim`**: XML files that define tween animations
- **`color`**: XML files that define a list of colors that can change based on the view's state (Color State Lists)
- **`drawable`**: bitmap files (PNG, JPG, GIF, WebP) or XML files that define various graphics like shapes, state lists, or layer lists
- **`font`**: font files (TTF, OTF, TTC) or XML files that define font families
- **`mipmap`**: drawable files for launcher icons, optimized for different screen densities
- **`raw`**: arbitrary raw files (e.g., MP3 audio files, JSON data, video files) that you need to access in their original format
- **`xml`**: arbitrary XML configuration files that can be read at runtime, such as a searchable configuration or preference headers
- **`values`**: XML files that contain simple values and primitives, including:
    - **`colors.xml`**: color values (hexadecimal colors).
    - **`strings.xml`**: string values, string arrays, and plurals for text in the UI
    - **`themes.xml`**: definitions for reusable themes and styles

## Accessing Resources

Accessing resources in Swift code is done via the generated Java `R` class.

For example, to access a string resource:
```swift
let myStringId: Int32 = R.string.my_string_resource
```
More about resource IDs can be found in the [→ R section](r.md)