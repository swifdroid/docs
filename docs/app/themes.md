# Themes

In Android development, themes are a powerful way to define the overall look and feel of your application. A theme is a collection of attributes that can be applied to an entire app, an activity, or a view hierarchy – allowing you to maintain a consistent design throughout your application.

Themes are typically defined in XML files located in the `res/values` directory of your Android project. They can include attributes such as colors, fonts, styles, and dimensions.

The **Droid** framework includes **AppCompat** and **Material Design 3** themes that you can easily apply to your app or activities.

## Applying Themes

You can set a theme for your entire application in the app manifest:

```swift
import Droid

@main
public final class App: DroidApp {
    @AppBuilder public override var body: Content {
        Manifest
            .theme(.material3DayNightNoActionBar)
    }
}
```

You can also set a theme for individual activities by overriding the `theme` property in your activity class:

```swift
final class MainActivity: AppCompatActivity {
    override class var theme: Style? { .material3DayNightNoActionBar }
    /// ...
}
```

## Custom Theme

If you use a custom theme, it is probably named `AppTheme`, so it can be used via the predefined constant 

```swift
.theme(.appTheme)
```

Otherwise, simply use your own style reference as a string, such as:

```swift
.theme("@style/MyOwnTheme")
```

## AppCompat

Base AppCompat theme - provides Material Design support back to API 7

```swift
.theme(.appCompat)
```

### noActionBar

A version of the AppCompat theme without an action bar

```swift
.theme(.appCompatNoActionBar)
```

### dialog

A dialog theme based on AppCompat

```swift
.theme(.appCompatDialog)
```

**alert**

A dialog alert theme based on AppCompat

```swift
.theme(.appCompatDialogAlert)
```

**minWidth**

A dialog theme with minimum width based on AppCompat

```swift
.theme(.appCompatDialogMinWidth)
```

**whenLarge**

A dialog theme optimized for large screens based on AppCompat

```swift
.theme(.appCompatDialogWhenLarge)
```

### DayNight

Enables automatic switching between light and dark themes based on system settings

```swift
.theme(.appCompatDayNight)
```

#### darkActionBar

Similar to `appCompatDayNight`, but includes a dark action bar for better contrast

```swift
.theme(.appCompatDayNightDarkActionBar)
```

#### noActionBar

A theme without an action bar that adapts to light and dark modes

```swift
.theme(.appCompatDayNightNoActionBar)
```

!!! success ""
    **Recommended for modern apps to support both light and dark themes!**

#### dialog

A dialog theme that adapts to light and dark modes

```swift
.theme(.appCompatDayNightDialog)
```

**alert**

A dialog alert theme that adapts to light and dark modes

```swift
.theme(.appCompatDayNightDialogAlert)
```

**minWidth**

A dialog theme with minimum width that adapts to light and dark modes

```swift
.theme(.appCompatDayNightDialogMinWidth)
```

**whenLarge**

A dialog theme that adapts to light and dark modes, optimized for large screens

```swift
.theme(.appCompatDayNightDialogWhenLarge)
```

### Light

A light version of the AppCompat theme

```swift
.theme(.appCompatLight)
```

#### darkActionBar

A light theme with a dark action bar for better contrast

```swift
.theme(.appCompatLightDarkActionBar)
```

#### noActionBar

A light theme without an action bar

```swift
.theme(.appCompatLightNoActionBar)
```

#### dialog

A light dialog theme based on AppCompat

```swift
.theme(.appCompatLightDialog)
```

**alert**

A light dialog alert theme based on AppCompat

```swift
.theme(.appCompatLightDialogAlert)
```

**minWidth**

A light dialog theme with minimum width based on AppCompat

```swift
.theme(.appCompatLightDialogMinWidth)
```

**whenLarge**

A light dialog theme optimized for large screens based on AppCompat

```swift
.theme(.appCompatLightDialogWhenLarge)
```

## Material3

### DayNight

Material Design 3 with automatic day/night switching

```swift
.theme(.material3DayNight)
```

#### noActionBar

DayNight Material 3 without action bar

```swift
.theme(.material3DayNightNoActionBar)
```

#### bottomSheetDialog

DayNight bottom sheet dialog with Material 3

```swift
.theme(.material3DayNightBottomSheetDialog)
```

#### sideSheetDialog

DayNight side sheet dialog with Material 3

```swift
.theme(.material3DayNightSideSheetDialog)
```

#### dialog

Standard DayNight dialog with Material 3 design

```swift
.theme(.material3DayNightDialog)
```

**alert**

DayNight alert dialog with Material 3

```swift
.theme(.material3DayNightDialogAlert)
```

**minWidth**

Compact DayNight dialog with Material 3

```swift
.theme(.material3DayNightDialogMinWidth)
```

**whenLarge**

Large DayNight dialog for big screens with Material 3

```swift
.theme(.material3DayNightDialogWhenLarge)
```

### Dark

Pure dark theme using Material Design 3 (Material You)

```swift
.theme(.material3Dark)
```

#### noActionBar

Dark Material 3 theme without action bar

```swift
.theme(.material3DarkNoActionBar)
```

#### bottomSheetDialog

Dark bottom sheet dialog with Material 3 styling

```swift
.theme(.material3DarkBottomSheetDialog)
```

#### sideSheetDialog

Dark side sheet dialog (emerging from side) with Material 3

```swift
.theme(.material3DarkSideSheetDialog)
```

#### dialog

Standard dark dialog with Material 3 design

```swift
.theme(.material3DarkDialog)
```

**alert**

Dark alert dialog with Material 3 styling

```swift
.theme(.material3DarkDialogAlert)
```

**minWidth**

Compact dark dialog with Material 3

```swift
.theme(.material3DarkDialogMinWidth)
```

**whenLarge**

Large dark dialog for big screens with Material 3

```swift
.theme(.material3DarkDialogWhenLarge)
```

### Light

Pure light theme using Material Design 3 (Material You)

```swift
.theme(.material3Light)
```

#### noActionBar

Light Material 3 theme without action bar

```swift
.theme(.material3LightNoActionBar)
```

#### bottomSheetDialog

Light bottom sheet dialog with Material 3

```swift
.theme(.material3LightBottomSheetDialog)
```

#### sideSheetDialog

Light side sheet dialog with Material 3

```swift
.theme(.material3LightSideSheetDialog)
```

#### dialog

Standard light dialog with Material 3 design

```swift
.theme(.material3LightDialog)
```

**alert**

Light alert dialog with Material 3 styling

```swift
.theme(.material3LightDialogAlert)
```

**minWidth**

Compact light dialog with Material 3

```swift
.theme(.material3LightDialogMinWidth)
```

**whenLarge**

Large light dialog for big screens with Material 3

```swift
.theme(.material3LightDialogWhenLarge)
```

### Dynamic - DayNight

DayNight theme with dynamic color theming

```swift
.theme(.material3DynamicColorsDayNight)
```

#### noActionBar

DayNight dynamic color theme without action bar

```swift
.theme(.material3DynamicColorsDayNightNoActionBar)
```

### Dynamic - Dark

Dark theme with dynamic color theming (Material You)

```swift
.theme(.material3DynamicColorsDark)
```

#### noActionBar

Dark dynamic color theme without action bar

```swift
.theme(.material3DynamicColorsDarkNoActionBar)
```

### Dynamic - Light

Light theme with dynamic color theming

```swift
.theme(.material3DynamicColorsLight)
```

#### noActionBar

Light dynamic color theme without action bar

```swift
.theme(.material3DynamicColorsLightNoActionBar)
```

## Material3Expressive

### DayNight

Expressive design language with automatic day/night switching

```swift
.theme(.material3ExpressiveDayNight)
```

#### noActionBar

Expressive DayNight theme without action bar

```swift
.theme(.material3ExpressiveDayNightNoActionBar)
```

#### dialog

Expressive DayNight dialog with dynamic design elements

**whenLarge**

Expressive DayNight dialog optimized for large screens

```swift
.theme(.material3ExpressiveDayNightDialogWhenLarge)
```

### Dark

Expressive design language - dark variant with more dynamic shapes and motion

```swift
.theme(.material3ExpressiveDark)
```

#### noActionBar

Expressive dark theme without action bar

```swift
.theme(.material3ExpressiveDarkNoActionBar)
```

#### dialog

Expressive dark dialog with dynamic design elements

```swift
.theme(.material3ExpressiveDarkDialog)
```

**alert**

Expressive dark alert dialog

```swift
.theme(.material3ExpressiveDarkDialogAlert)
```

**minWidth**

Expressive dark dialog with minimum width

```swift
.theme(.material3ExpressiveDarkDialogMinWidth)
```

**whenLarge**

Expressive dark dialog optimized for large screens

```swift
.theme(.material3ExpressiveDarkDialogWhenLarge)
```

### Light

Expressive design language - light variant

```swift
.theme(.material3ExpressiveLight)
```

#### noActionBar

Expressive light theme without action bar

```swift
.theme(.material3ExpressiveLightNoActionBar)
```

#### dialog

Expressive light dialog with dynamic design elements

```swift
.theme(.material3ExpressiveLightDialog)
```

**alert**

Expressive light alert dialog

```swift
.theme(.material3ExpressiveLightDialogAlert)
```

**minWidth**

Expressive light dialog with minimum width

```swift
.theme(.material3ExpressiveLightDialogMinWidth)
```

**whenLarge**

Expressive light dialog optimized for large screens

```swift
.theme(.material3ExpressiveLightDialogWhenLarge)
```

### Dynamic - DayNight

Expressive DayNight theme with dynamic color adaptation

```swift
.theme(.material3ExpressiveDynamicColorsDayNight)
```

#### noActionBar

Expressive DayNight dynamic color theme without action bar

```swift
.theme(.material3ExpressiveDynamicColorsDayNightNoActionBar)
```

### Dynamic - Dark

Expressive dark theme with dynamic color adaptation

```swift
.theme(.material3ExpressiveDynamicColorsDark)
```

#### noActionBar

Expressive dark dynamic color theme without action bar

```swift
.theme(.material3ExpressiveDynamicColorsDarkNoActionBar)
```

### Dynamic - Light

Expressive light theme with dynamic color adaptation

```swift
.theme(.material3ExpressiveDynamicColorsLight)
```

#### noActionBar

Expressive light dynamic color theme without action bar

```swift
.theme(.material3ExpressiveDynamicColorsLightNoActionBar)
```