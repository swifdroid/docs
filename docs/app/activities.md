# Activities

**Activity** in Android represents a single screen. Unlike iOS, where the whole app lives on a single screen, Android apps are made up of multiple activities that users can navigate between. Each activity is independent and has its own lifecycle.

!!! info ""
    However activities is not the only way to create screens in Android. You can also use [→ fragments](x/fragments.md), which are modular sections of an activity. Fragments allow for more flexible UI designs.

## Starting an Activity

In classic Android development, you are starting an Activity via intents. With Droid framework you can do it in a more Swiftly way.

Simply initialize the activity class you want to start and call `startActivity` method with it:

```swift
let secondActivity = SecondActivity()
startActivity(secondActivity)
```

This way gives you an ablity to pass parameters to the activity via its initializer rather than using intent extras.

You also can start multiple activities at once by passing an array of activities to `startActivity` method:

```swift
let activity1 = FirstActivity()
let activity2 = SecondActivity()
startActivity(activity1, activity2)
```

And you can start activities for result using `startActivityForResult` method:

```swift
let thirdActivity = ThirdActivity()
startActivityForResult(thirdActivity, requestCode: 1001)
```

The way with intents is also supported, mostly for an ability to start activities from outside of the Droid framework.

```swift
let intent = Intent(...)
startActivity(intent)
```

## Launcher Activity

Some activities can be designated as launcher activities. These are the entry points of your app, the first screen users see when they launch your app from the home screen or app drawer.

Normally, the main launcher activity is defined in your app's manifest file with a specific intent filter. But with Droid framework you have to define it simply in your activity class:

```swift
final class MainActivity: AppCompatActivity {
    override class var intentFilter: IntentFilter? { .mainLauncher }
    /// ...
}
```
This will automatically set up the necessary intent filter in the generated Android manifest file.

## Creating an Activity

To create a new activity you need to do only two things.

### Activity class

Create a new class that extends `Activity` (or its subclass like `AppCompatActivity`, depending on your theme):

```swift
import Droid

final class YourActivity: AppCompatActivity {
    override var body: any Body {
        TextView("Hello world!")
            .width(.matchParent)
            .height(.matchParent)
            .gravity(.center)
    }
}
```

### Manifest Declaration

Declare the activity in your `App.swift` file in the `Manifest`:

```swift
@main
public final class App: DroidApp {
    @AppBuilder public override var body: Content {
        /// ...
        Manifest
            /// ...
            .activities(
                /// ...
				YourActivity.self,
                /// ...
            )
    }
}
```

Corresponding Kotlin code for the activity will be generated automatically during the build process.

!!! success ""
    That's it! You have created a new activity for your Android app.

## Lifecycle

Activities have a well-defined lifecycle managed by the Android operating system.

!!! warning ""
    It's crucial to call **`super`** for each lifecycle method you override to ensure the proper functioning of the activity.

### onCreate

Called when the activity is first created.

Here it receives an `ActivityContext` which can later be accessed via the `self.context` property anytime.

```swift
override func onCreate(_ context: ActivityContext) {
    super.onCreate(context)
    Log.i("onCreate")
}
```

### body

This is where you build your UI declaratively using SwiftUI-like syntax.

Is a convenient minimalistic way to define the UI declaratively.

It is called after `onCreate` and before `buildUI`.

```swift
override var body: any Body {
    TextView("Hello world!")
        .width(.matchParent)
        .height(.matchParent)
        .gravity(.center)
}
```

It can be used in mix with `buildUI` method, or alone.

### buildUI

**Alternative to `body`**, could be more useful if you need to declare some variables or calculate something before building the UI.

It is called after `onCreate` and `body`.

```swift
override func buildUI() {
    super.buildUI()
    let customText = "Hello from buildUI!"
    body {
        TextView(customText)
            .width(.matchParent)
            .height(.matchParent)
            .gravity(.center)
    }
}
```

### onStart

Called when the activity is becoming visible to the user.

Happens after `onCreate()` (for a new instance) or `onRestart()` (for a restarted one).

```swift
override func onStart() {
    super.onStart()
    Log.i("onStart")
}
```

### onRestart

Called after the activity has been stopped and right before it starts again.

This method is typically followed by calls to `onStart()` and then `onResume()`.
Override `onRestart()` when you need to restore resources or state that were released or adjusted in `onStop()`.

```swift
override func onRestart() {
    super.onRestart()
    Log.i("onRestart")
}
```

### onResume

Called when the activity is about to resume interaction with the user.

At this point, the activity is in the foreground and ready for user input.

Resume any tasks that were paused (e.g. restarting animations or refreshing data).

```swift
override func onResume() {
    super.onResume()
    Log.i("onResume")
}
```

### onPause

Called when the activity is about to enter a paused state.

This means another activity is coming into the foreground, but this one is still partially visible. 

Use this to pause animations, video playback, or other ongoing tasks.

```swift
override func onPause() {
    super.onPause()
    Log.i("onPause")
}
```

### onStop

Called when the activity is no longer visible to the user.

This happens when a new activity covers it or the app goes to the background.

Use this to release resources that don’t need to be kept while the activity is not visible.

```swift
override func onStop() {
    super.onStop()
    Log.i("onStop")
}
```

### onDestroy

Called before the activity is completely destroyed.

This may occur when:
- The activity is finishing (`finish()` was called), or
- The system needs to reclaim memory.

Use this to perform final cleanup, like releasing resources or saving persistent data.

```swift
override func onDestroy() {
    super.onDestroy()
    Log.i("onDestroy")
}
```

### onStateNotSaved

Called when the system is about to save the activity's state, but before that state has actually been committed.

Typically invoked before `onStop()`. 

Override this to do lightweight cleanup tasks that should not be persisted in the saved instance state.

```swift
override func onStateNotSaved() {
    super.onStateNotSaved()
    Log.i("onStateNotSaved")
}
```

### onAttachedToWindow

Called when the activity's window has been attached to the window manager.

This is the point where your activity can safely interact with the actual window.

Useful for performing final UI setup that depends on the window being ready.

```swift
override func onAttachedToWindow() {
    super.onAttachedToWindow()
    Log.i("onAttachedToWindow")
}
```

### onBackPressed

Called when the user presses the **Back** button.

By default, this finishes the current activity.

Override this to implement custom back navigation logic (e.g., showing a confirmation dialog before exit).

```swift
override func onBackPressed() {
    Log.i("onBackPressed")
    finish() // finish activity manually
}
```

### onActivityResult

Called when an activity you launched with `startActivityForResult()` finishes.

Parameters:

- **requestCode**: The integer request code originally supplied
- **resultCode**: The integer result code returned by the child activity
- **intent**: Optional data returned from the child activity
- **componentCaller**: The caller component that initiated the request

Override this to handle results from sub-activities (e.g., picking an image or capturing video).

```swift
override func onActivityResult(
    requestCode: Int,
    resultCode: Int,
    intent: Intent?,
    componentCaller: ComponentCaller?
) {
    super.onActivityResult(
        requestCode: requestCode,
        resultCode: resultCode,
        intent: intent,
        componentCaller: componentCaller
    )
    Log.i("onActivityResult request: \(requestCode), result: \(resultCode)")
}
```

### onMultiWindowModeChanged

Called when the activity's multi-window mode changes.

If `isInMultiWindowMode` is `true` then activity is in multi-window mode now.

Override this to adjust your UI or behavior based on whether the activity is in multi-window mode.

```swift
override func onMultiWindowModeChanged(isInMultiWindowMode: Bool) {
    super.onMultiWindowModeChanged(isInMultiWindowMode: isInMultiWindowMode)
    Log.i("onMultiWindowModeChanged: \(isInMultiWindowMode)")
}
```

### onRequestPermissionsResult

Called when the user responds to a permission request.

Parameters:

- **requestCode**: The integer request code originally supplied to `requestPermissions()`
- **results**: An array of `ActivityPermissionResult` indicating the permissions requested and whether they were granted
- **deviceId**: The ID of the device for which the permissions were requested

Override this to handle the user's response to permission requests.

```swift
override func onRequestPermissionsResult(
    requestCode: Int,
    results: [ActivityPermissionResult],
    deviceId: Int
) {
    super.onRequestPermissionsResult(
        requestCode: requestCode,
        results: results,
        deviceId: deviceId
    )
    for result in results {
        Log.i("Permission: \(result.permission), granted: \(result.granted)")
    }
}
```

Learn how to easily request [→ permissions](permissions.md)

## Finishing Activity

### finish

Call this when your activity is done and should be closed.

```swift
finish()
```

### finishAffinity

Call this to finish the current activity and all activities immediately below it in the current task that have the same affinity.

```swift
finishAffinity()
```

### finishAfterTransition

Call this to finish the activity after completing any ongoing transitions.

```swift
finishAfterTransition()
```

### finishActivity

Call this to finish another activity that you had previously started with `startActivityForResult()`.

```swift
finishActivity(requestCode: 1001)
```

## Configuration

### className

Class name of the activity which can be overriden for the new activity types. For example, classic activity class is `android/app/Activity`, `AppCompatActivity` is `androidx/appcompat/app/AppCompatActivity`, but you may want to create an activity based on some custom class and then you have to override this property.

```swift
open class MyCustomActivity: AppCompatActivity {
    open class override var className: JClassName { "com/domain/Activity" }
}
```

### gradle dependencies

When you are wrapping some custom activity class from a library, you may need to add corresponding gradle dependencies to your project. You can do it by overriding this property:

```swift
open class MyCustomActivity: AppCompatActivity {
    open class override var gradleDependencies: [GradleDependency] {
        [ .implementation("com.domain:library:1.0.0") ]
    }
}
```

### java imports

If you are using some custom activity class, you may need to add corresponding Java imports to the generated Kotlin code. You can do it by overriding this property:

```swift
open class MyCustomActivity: AppCompatActivity {
    open class override var javaImports: [String] {
        [ "com.domain.library.*" ]
    }
}
```

### parent class

The default is `DroidActivity()`.

When you are creating an activity it is based on some activity class, you may need to override the parent class declaration in the generated Kotlin code:

```swift
open class MyCustomActivity: AppCompatActivity {
    open class override var parentClass: String { "DroidActivity()" }
}
```

### allowEmbedded

Indicate that the activity can be launched as the embedded child of another activity [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#embedded)

```swift
final class YourActivity: AppCompatActivity {
    override class var allowEmbedded: Bool { true }
    /// ...
}
```

### allowTaskReparenting

/// Whether or not the activity can move from the task that started it to the task it has an affinity for when that task is next brought to the front – **"true"** if it can move, and **"false"** if it must remain with the task where it started [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#reparent)

```swift
final class YourActivity: AppCompatActivity {
    override class var allowTaskReparenting: Bool { true }
    /// ...
}
```

### alwaysRetainTaskState

/// Whether or not the state of the task that the activity is in will always be maintained by the system – **"true"** if it will be, and **"false"** if the system is allowed to reset the task to its initial state in certain situations [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#always)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var alwaysRetainTaskState: Bool { true }
    /// ...
}
```

### autoRemoveFromRecents

Whether or not tasks launched by activities with this attribute remains in the overview screen until the last activity in the task is completed. If true, the task is automatically removed from the overview screen [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#autoremrecents)

```swift
final class YourActivity: AppCompatActivity {
    override class var autoRemoveFromRecents: Bool { true }
    /// ...
}
```

### banner

A drawable resource providing an extended graphical banner for its associated item [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#banner)

```swift
final class YourActivity: AppCompatActivity {
    override class var banner: String? { "@drawable/banner" }
    /// ...
}
``` 

### clearTaskOnLaunch

Whether or not all activities will be removed from the task, except for the root activity, whenever it is re-launched from the home screen – **"true"** if the task is always stripped down to its root activity, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#clear)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var clearTaskOnLaunch: Bool { true }
    /// ...
}
```

### colorMode

Requests the activity to be displayed in wide color gamut mode on compatible devices [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#colormode)

```swift
final class YourActivity: AppCompatActivity {
    override class var colorMode: ActivityColorMode { .hdr }
    /// ...
}   
```

### configChanges

Lists configuration changes that the activity will handle itself.  
For the rest activity will be simply restarted [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#config)

```swift
final class YourActivity: AppCompatActivity {
    override class var configChanges: [ActivityConfigChange] {
        [ .keyboard, .orientation, .screenSize, .screenLayout ]
    }
    /// ...
}
```

### directBootAware

Whether or not the activity is direct-boot aware; that is, whether or not it can run before the user unlocks the device [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#directBootAware)

```swift
final class YourActivity: AppCompatActivity {
    override class var directBootAware: Bool { true }
    /// ...
}
```

### documentLaunchMode

Specifies how a new instance of an activity should be added to a task each time it is launched [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#dlmode)

```swift
final class YourActivity: AppCompatActivity {
    override class var documentLaunchMode: ActivityDocumentLaunchMode {
        .intoExisting
    }
    /// ...
}
```

### enabled

Whether or not the activity can be instantiated by the system – **"true"** if it can be, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#enabled)

The default value is "true".

```swift
final class YourActivity: AppCompatActivity {
    override class var enabled: Bool { false }
    /// ...
}
```

### excludeFromRecents

Whether or not the task initiated by this activity should be excluded from the list of recently used applications, the overview screen [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#exclude)

```swift
final class YourActivity: AppCompatActivity {
    override class var excludeFromRecents: Bool { true }
    /// ...
}
```

### exported

Whether it can be launched by components of other applications [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#exported)

If **"true"**, the activity is accessible to any app, and is launchable by its exact class name.

If **"false"**, the activity can be launched only by components of the same application, applications with the same user ID, or privileged system components.

**"false"** is the default value when there are no intent filters.

```swift
final class YourActivity: AppCompatActivity {
    override class var exported: Bool { true }
    /// ...
}
```

### finishOnTaskLaunch

Whether or not an existing instance of the activity should be shut down (finished), except for the root activity, whenever the user again launches its task (chooses the task on the home screen) – **"true"** if it should be shut down, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#finish)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var finishOnTaskLaunch: Bool { true }
    /// ...
}
```

### hardwareAccelerated

Whether or not hardware-accelerated rendering should be enabled for this Activity – **"true"** if it should be enabled, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#hwaccel)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var hardwareAccelerated: Bool { true }
    /// ...
}
```

### icon

An icon representing the activity [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#icon)

```swift
final class YourActivity: AppCompatActivity {
    override class var icon: String? { "@mipmap/ic_launcher" }
    /// ...
}
```

### roundIcon

A default round icon for all application components [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#icon)

```swift
final class YourActivity: AppCompatActivity {
    override class var roundIcon: String? { "@mipmap/ic_launcher_round" }
    /// ...
}
```

### immersive

Sets the immersive mode setting for the current activity [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#immersive)

```swift
final class YourActivity: AppCompatActivity {
    override class var immersive: Bool { true }
    /// ...
}
```

### label

A user-readable label for the activity [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#label)

```swift
final class YourActivity: AppCompatActivity {
    override class var label: String? { "My Activity" }
    /// ...
}
```

### launchMode

An instruction on how the activity should be launched [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#lmode)

```swift
final class YourActivity: AppCompatActivity {
    override class var launchMode: LaunchMode { .singleTop }
    /// ...
}
```

### lockTaskMode

Specifies how this activity should behave when the device is in lock task mode [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#ltmode)

```swift
final class YourActivity: AppCompatActivity {
    override class var lockTaskMode: LockTaskMode { .ifWhitelisted }
    /// ...
}
```

### maxRecents

he maximum number of tasks rooted at this activity in the overview screen [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#maxrecents)

```swift
final class YourActivity: AppCompatActivity {
    override class var maxRecents: Int { 3 }
    /// ...
}
```

### maxAspectRatio

The maximum aspect ratio the activity supports [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#maxaspectratio)

```swift
final class YourActivity: AppCompatActivity {
    override class var maxAspectRatio: Float { 2.0 }
    /// ...
}
```

### multiprocess

Whether an instance of the activity can be launched into the process of the component that started it – **"true"** if it can be, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#multi)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var multiprocess: Bool { true }
    /// ...
}
```

### noHistory

Whether or not the activity should be removed from the activity stack and finished (its finish() method called) when the user navigates away from it and it's no longer visible on screen – **"true"** if it should be finished, and **"false"** if not [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#nohist)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var noHistory: Bool { true }
    /// ...
}
```

### parentActivityName

The class name of the logical parent of the activity [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#parent)

```swift
final class YourActivity: AppCompatActivity {
    override class var parentActivityName: AnyActivity.Type? {
        ParentActivity.self
    }
    /// ...
}
```

### persistableMode

Defines how an instance of an activity is preserved within a containing task across device restarts [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#persistableMode)

```swift
final class YourActivity: AppCompatActivity {
    override class var persistableMode: PersistableMode { .acrossReboots }
    /// ...
}
```

### permission

The name of a permission that clients must have to launch the activity or otherwise get it to respond to an intent [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#prmsn)

```swift
final class YourActivity: AppCompatActivity {
    override class var permission: ManifestPermission? { .sendSms }
    /// ...
}
```

### process

The name of the process in which the activity should run [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#proc)

```swift
final class YourActivity: AppCompatActivity {
    override class var process: String? { ":remote" }
    /// ...
}
```

### relinquishTaskIdentity

Whether or not the activity relinquishes its task identifiers to an activity above it in the task stack [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#relinquish)

```swift
final class YourActivity: AppCompatActivity {
    override class var relinquishTaskIdentity: Bool { true }
    /// ...
}
```

### resizeableActivity

Specifies whether the app supports multi-window mode [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#resizeableActivity)

```swift
final class YourActivity: AppCompatActivity {
    override class var resizeableActivity: Bool { true }
    /// ...
}
```

### screenOrientation

The orientation of the activity's display on the device [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#screen)

```swift
final class YourActivity: AppCompatActivity {
    override class var screenOrientation: ScreenOrientation {
        .portrait
    }
    /// ...
}
```

!!! warning ""
    The system ignores this attribute if the activity is running in multi-window mode

### showForAllUsers

Whether or not the activity is shown when the device's current user is different than the user who launched the activity. You can set this attribute to a literal value – **"true"** or **"false"** – or you can set the attribute to a resource or theme attribute that contains a boolean value [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#showForAllUsers)

```swift
final class YourActivity: AppCompatActivity {
    override class var showForAllUsers: Bool { true }
    /// ...
}
```

### stateNotNeeded

Whether or not the activity can be killed and successfully restarted without having saved its state — "true" if it can be restarted without reference to its previous state, and "false" if its previous state is required [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#state)

The default value is "false".

```swift
final class YourActivity: AppCompatActivity {
    override class var stateNotNeeded: Bool { true }
    /// ...
}
```

### supportsPictureInPicture

Specifies whether the activity supports Picture-in-Picture display [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#supportsPIP)

```swift
final class YourActivity: AppCompatActivity {
    override class var supportsPictureInPicture: Bool { true }
    /// ...
}
```

### taskAffinity

The task that the activity has an affinity for [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#aff)

```swift
final class YourActivity: AppCompatActivity {
    override class var taskAffinity: String? { "com.example.task" }
    /// ...
}
```

### theme

The theme name applied to the activity as soon as it is created, before any views are instantiated. If not specified, the application-level theme defined in your app's manifest is used. If no application-level theme is set, the default system theme will be applied [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#theme)

```swift
final class YourActivity: AppCompatActivity {
    override class var theme: Style? { .material3DayNightNoActionBar }
    /// ...
}
```

### uiOptions

Extra options for an activity's UI [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#uioptions)

```swift
final class YourActivity: AppCompatActivity {
    override class var uiOptions: ApplicationUIOptions {
        .splitActionBarWhenNarrow
    }
    /// ...
}
```

### windowSoftInputMode

How the main window of the activity interacts with the window containing the on-screen soft keyboard [→ docs](https://developer.android.com/guide/topics/manifest/activity-element#wsoft)

```swift
final class YourActivity: AppCompatActivity {
    override class var windowSoftInputMode: [WindowSoftInputMode] {
        [.adjustResize, .stateHidden]
    }
    /// ...
}
```

### intentFilter

Specifies the types of intents that an activity can respond to [→ docs](https://developer.android.com/guide/topics/manifest/intent-filter-element)

```swift
final class MainActivity: AppCompatActivity {
    override class var intentFilters: [IntentFilter] {
        [.action(.main).category(.launcher)]
    }
    /// ...
}
```

### metaData

A name-value pair for an item of additional, arbitrary data that can be supplied to the parent component [→ docs](https://developer.android.com/guide/topics/manifest/meta-data-element)

```swift
final class YourActivity: AppCompatActivity {
    override class var metaData: [MetaData] {
        [.name("key").value("value")]
    }
    /// ...
}
```           