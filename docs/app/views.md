# Views

The **Droid** framework provides all the native Android views you need.

The full list of available views is in the left sidebar, categorized into sections like **Classic**, **Android X**, and **Material Design**.

!!! warning ""
    The application documentation is under active development. If you encounter any 404 pages or typos, please be patient – new content is being added every day.

For quick reference, here are some of the most commonly used views:  
[→ LinearLayout](classic/linear-layout.md)  
[→ RelativeLayout](classic/relative-layout.md)  
[→ TextView](classic/text-view.md)  
[→ Button](classic/button.md)  
[→ ImageView](classic/image-view.md)  
[→ EditText](classic/edit-text.md)  
[→ RecyclerView](x/recycler-view.md)  
[→ ScrollView](classic/scroll-view.md)

!!! important ""
    Use the search bar to quickly find the view you need

Views can be placed inside activities, fragments, or other views.

## Building UI

You either build UI declaratively like in SwiftUI, or imperatively by creating view instances and configuring them.

### Imperatively

It looks similar to how you would do it in Java, but using Swift syntax.

```swift
let linearLayout = LinearLayout()
linearLayout.orientation(.vertical)
let textView = TextView()
textView.text("Hello, World!")
textView.textSize(24)
textView.textColor(.black)
textView.padding(16)
textView.backgroundColor(.lightGray)
linearLayout.addSubview(textView)
contentView(linearLayout)
```
You may notice that property setters are without `set` prefix, this is intentional to make it more Swiftly. Getters are also without `get` prefix.

### Declaratively

Most natural way to build UI in Swift is using declarative syntax similar to SwiftUI.

```swift
LinearLayout {
    TextView("Hello, World!")
        .textSize(24)
        .textColor(.black)
        .padding(16)
        .backgroundColor(.lightGray)
}
.orientation(.vertical)
```

Allright, but how do we put this into an activity or fragment? There are [→ body](../activities/#body) and [→ buildUI](../activities/#buildui) methods:

```swift
override func buildUI() {
    super.buildUI()
    body {
        TextView("Hello, World!")
            .width(.matchParent)
            .height(.matchParent)
            .gravity(.center)
    }
}
```

## Context

You may notice that `View` initializers do not require a `Context` parameter. This is because **Droid** framework automatically provides the correct context behind the scenes. You can prepare the view anywhere, and it will work correctly when added to the view hierarchy.

## Custom Views

Any view inherits from the base `View` class, which provides common properties and methods for all views. Inheritance is exactly the same as in Android SDK.

When you create a custom view, you need a few steps: 

1. inherit from the appropriate base class
2. set the `className` property to specify the full Java class name
3. set the `layoutParamsClass` property to specify the default layout params class
4. set required gradle dependencies in `gradleDependencies` property
5. add initializers
6. add methods

Let's investigate each step.

### Inheritance and Dependencies

Your view may inherit from `View`, `ViewGroup`, or any other view class.  
This is how you declare a custom view that inherits from `ViewGroup`:
```swift
final class CustomView: ViewGroup {
    // ...
}
```
Then if your view is not just reimplementation of an existing one, you set the Java `className`:
```swift
override class var className: JClassName {
    "com/mycompany/customviews/CustomView"
}
```
Next, each view should provide its own `LayoutParams` class, otherwise it will use the default `ViewGroup.LayoutParams`, which may not be suitable and will cause runtime crash.
```swift
override class var layoutParamsClass: LayoutParams.Class {
    "com/mycompany/customviews/CustomView$LayoutParams"
}
```
Finally, if your view depends on any external Java libraries, you need to specify them in `gradleDependencies` property, so Swift Stream IDE knows which dependencies to include in the generated Gradle project.
```swift
override class var gradleDependencies: [AppGradleDependency] {[
    #"implementation "com.mycompany:customviews:1.0.0""#
]}
```
I also suggest creating a convenient static property for your custom dependency to avoid version mismatches if you use it in multiple places:
```swift
extension AppGradleDependency {
    static let customViews: Self = #"implementation "com.mycompany:customviews:1.0.0""#
}
```
and then use it in your view like this:
```swift
override class var gradleDependencies: [AppGradleDependency] {[
    .customViews
]}
```

Allright, this way, **Droid** framework knows which Java class to instantiate, which layout params to use, and which Gradle dependencies to include in your project. Easy and convenient.

### Initializer

Next, we add the required initializer:
```swift
@discardableResult
override init (id: Int32? = nil) {
    super.init(id: id)
}
```
And if your view inherits from `ViewGroup` you have to add another initializer with `@BodyBuilder`:
```swift
@discardableResult
override init (
    id: Int32? = nil,
    @BodyBuilder content: BodyBuilder.SingleView
) {
    super.init(id: id, content: content)
}
```
This means that this view accepts child views like this:
```swift
CustomView {
    TextView("Child 1")
    TextView("Child 2")
}
```

### View Properties

All properties of the view set not immediately, but later once the view is attached to the view hierarchy.

**That's why you can declare views anywhere, even outside activities or fragments.**

But to achieve this, you need to wrap all property setters into special structs that conform to `ViewPropertyToApply` protocol.

Let's say your Java view has a property called `baselineAligned`, which is a boolean. First you create a struct for it:
```swift
struct BaselineAlignedViewProperty: ViewPropertyToApply {
    let key: ViewPropertyKey = "setBaselineAligned"
    let value: Bool
    func applyToInstance(_ env: JEnv?, _ instance: View.ViewInstance) {
        // if it is Java field
        instance.booleanField(env, name: key.rawValue, value: value)
        // if it is Java method
        instance.callVoidMethod(env, name: key.rawValue, args: value)
    }
}
```
Then you create a convenient method in the view extension:
```swift
extension CustomView {
    @discardableResult
    public func baselineAligned(_ value: Bool = true) -> Self {
        BaselineAlignedViewProperty(value: value).applyOrAppend(nil, self)
    }
}
```
This method creates the property struct and applies it to the view instance immediately if it is already attached or appends it to be applied later. The rest is handled by the framework.

So now it can be used like this:
```swift
CustomView().baselineAligned()
```

!!! note ""
    Learn more about JNI usage in [→ JNIKit Fields](/jni-kit/fields/#setting-field-value) and [→ JNIKit Methods](/jni-kit/methods/#calling-method-that-pass-an-object-and-returns-nothing)

#### ID

Views have unique integer IDs assigned automatically.

Access the ID simply via `id` property:
```swift
let view = CustomView()
let viewId = view.id
```

You can also set your own ID via initializer:
```swift
CustomView(id: 12345)
```

### View Methods

If your view has some methods that you want to expose, you can add them directly in the view extension.

```swift
extension CustomView {
    public func doSomething(param: Int) {
        instance?.callVoidMethod(name: "doSomething", args: param)
    }
}
```
This method will be called immediately on the view instance if it is already attached. Otherwise, you need to use [→ View Properties](#view-properties) technique to queue the method call for later.

### Layout Params Properties

Layout Params are set similarly to view properties, but they conform to `LayoutParamToApply` protocol.

Let's say your Java view has a layout param called `weight`, which is a float. First you create a struct for it:
```swift
struct WeightLayoutParam: LayoutParamToApply {
    let key: LayoutParamKey = "weight"
    let value: Float
    func apply(_ env: JEnv?, _ context: View.ViewInstance, _ lp: LayoutParams) {
        // if it is Java field
        lp.setField(env, name: key.rawValue, arg: value)
        // if it is Java method
        lp.callVoidMethod(env, name: key.rawValue, args: value)
    }
}
```
Then you create a convenient method in the view extension:
```swift
extension CustomView {
    @discardableResult
    public func weight(_ value: Float) -> Self {
        WeightLayoutParam(value: value).applyOrAppend(self)
    }
}
```
This method creates the layout param struct and applies it to the view's layout params immediately if the view is already attached or appends it to be applied later.

### Layout Params Methods

If your layout params has some methods that you want to expose, you can add them directly in the view extension.

```swift
extension CustomView {
    public func changeSomething() {
        instance?.layoutParams()?.callVoidMethod(name: "changeSomething")
    }
}
```

## Methods

### addSubview

You can add one subview:
```swift
let subView = TextView("Child 1")
addSubview(subView)
```
or multiple child views:
```swift
let subView1 = TextView("Child 1")
let subView2 = TextView("Child 2")
addSubviews(subView1, subView2) // or with array
```

### removeSubview

You can remove a specific child view like this:
```swift
removeSubview(subView)
```

### removeFromParent

You can remove the view from its parent view like this:
```swift
subView.removeFromParent()
```

### onAddToParent

You can listen for when the view is added to a parent view to perform additional setup.
```swift
.onAddToParent {
    Log.i("CustomView added to parent")
}
```

### onRemovedFromParent

You can listen for when the view is removed from its parent to perform cleanup actions.

```swift
.onRemovedFromParent {
    Log.i("CustomView removed from parent")
}
```

### willAddSubview

You can override this method to intercept when a subview is about to be added.

```swift
final class CustomView: ViewGroup {
    override func willAddSubview(_ subview: View) {
        super.willAddSubview(subview)
        Log.i("Subview \(subview) will be added")
    }
}
```

### didAddSubview

You can override this method to intercept when a subview has been added.

```swift
final class CustomView: ViewGroup {
    override func didAddSubview(_ subview: View) {
        super.didAddSubview(subview)
        Log.i("Subview \(subview) was added")
    }
}
```

### willRemoveSubview

You can override this method to intercept when a subview is about to be removed.

```swift
final class CustomView: ViewGroup {
    override func willRemoveSubview(_ subview: View) {
        super.willRemoveSubview(subview)
        Log.i("Subview \(subview) will be removed")
    }
}
```

### didRemoveSubview

You can override this method to intercept when a subview has been removed.

```swift
final class CustomView: ViewGroup {
    override func didRemoveSubview(_ subview: View) {
        super.didRemoveSubview(subview)
        Log.i("Subview \(subview) was removed")
    }
}
```

### willMoveToParent

You can override this method to intercept when the view is about to be moved to a parent view.

```swift
final class CustomView: View {
    override func willMoveToParent() {
        super.willMoveToParent()
        Log.i("CustomView will move to parent")
    }
}
```
Parent is still nil at this point.

### didMoveToParent

You can override this method to intercept when the view has been moved to a parent view.

```swift
final class CustomView: View {
    override func didMoveToParent() {
        super.didMoveToParent()
        Log.i("CustomView did move to parent")
    }
}
```
If the view is added as a child view, then you can access parent from the `parent` property.

If the view is a content view of an activity/fragment, then `parent` will stay nil.

### willMoveFromParent

You can override this method to intercept when the view is about to be removed from its parent view.

```swift
final class CustomView: View {
    override func willMoveFromParent() {
        super.willMoveFromParent()
        Log.i("CustomView will be removed its from parent")
    }
}
```

### didMoveFromParent

You can override this method to intercept when the view has been removed from its parent view.

```swift
final class CustomView: View {
    override func didMoveFromParent() {
        super.didMoveFromParent()
        Log.i("CustomView has been removed from its parent")
    }
}
```

### accessibilityDataSensitive

Sets whether this view contains sensitive information for accessibility services.

```swift
.accessibilityDataSensitive(.auto)
```

### accessibilityHeading

Sets whether this view is a heading for a section of content for accessibility purposes.

```swift
.accessibilityHeading(true)
```

### accessibilityLiveRegion

Sets the live region mode for this view

```swift
.accessibilityLiveRegion(.assertive)
```

### accessibilityPaneTitle

Sets the accessibility pane title for this view.

```swift
.accessibilityPaneTitle("Main Content")
```

### accessibilityTraversalAfter

Sets the id of a view that screen readers are requested to visit before this view.

```swift
.accessibilityTraversalAfter(anotherViewId)
```

### accessibilityTraversalBefore

Sets the id of a view that screen readers are requested to visit after this view.

```swift
.accessibilityTraversalBefore(anotherViewId)
```

### activated

Sets the activated state of this view.

```swift
.activated() // sets to true
.activated(false) // sets to false
```

### allowClickWhenDisabled

Sets whether this view should allow click events when it is disabled.

```swift
.allowClickWhenDisabled() // sets to true
.allowClickWhenDisabled(false) // sets to false
```

### allowedHandwritingDelegatePackage

Specifies that this view may act as a handwriting initiation delegator for a delegate editor view from the specified package.

```swift
.allowedHandwritingDelegatePackage("com.example.handwriting")
```

### alpha

Sets the alpha transparency of this view.

```swift
.alpha(0.5) // sets to 50% transparency
```

### autoHandwritingEnabled

Set whether this view enables automatic handwriting initiation.

```swift
.autoHandwritingEnabled() // sets to true
.autoHandwritingEnabled(false) // sets to false
```

### backgroundColor

Sets the background color of this view.

```swift
.backgroundColor(.lightGray)
```

Refer to [→ colors](colors.md) section for more details on colors.

### backgroundResource

Sets the background to a given resource ID.

```swift
.backgroundResource(R.drawable.my_background)
```

Refer to [→ R](r.md) section for more details on resource IDs.

### bottom

Sets the bottom position of this view relative to its parent.

```swift
.bottom(100) // in dp units
.bottom(100, .px) // in pixels
```

Refer to [→ units](units.md) section for more details on dimension units.

### cameraDistance

Sets the distance along the Z axis (orthogonal to the X/Y plane on which views are drawn) from the camera to this view.

```swift
.cameraDistance(1000) // in dp units
.cameraDistance(1000, .px) // in pixels
```

Refer to [→ units](units.md) section for more details on dimension units.

### clickable

Enables or disables click events for this view.

```swift
.clickable() // sets to true
.clickable(false) // sets to false
```

### clipToOutline

Sets whether the View's Outline should be used to clip the contents of the View.

```swift
.clipToOutline() // sets to true
.clipToOutline(false) // sets to false
```

### contentDescription

Sets the content description of this view for accessibility purposes.

```swift
.contentDescription("This is a button")
```

### contentSensitivity

Sets content sensitivity mode to determine whether this view displays sensitive content (e.g. username, password etc.).

```swift
.contentSensitivity(.sensitive)
.contentSensitivity(.notSensitive)
.contentSensitivity(.auto) // uses heuristics
```

### contextClickable

Enables or disables context clicking for this view.

```swift
.contextClickable() // sets to true
.contextClickable(false) // sets to false
```

### defaultFocusHighlightEnabled

Sets whether this View should use a default focus highlight when it gets focused but doesn't have `R.attr.state_focused` defined in its background.

```swift
.defaultFocusHighlightEnabled() // sets to true
.defaultFocusHighlightEnabled(false) // sets to false
```

### drawingCacheBackgroundColor

Sets the drawing cache background color for this view.

```swift
.drawingCacheBackgroundColor(.white)
```

Refer to [→ colors](colors.md) section for more details on colors.

### drawingCacheEnabled

Enables or disables the drawing cache for this view.

```swift
.drawingCacheEnabled() // sets to true
.drawingCacheEnabled(false) // sets to false
```

!!! warning ""
    This method was deprecated in API level 28.

### drawingCacheQuality

Sets the drawing cache quality for this view.

```swift
.drawingCacheQuality(.low)
.drawingCacheQuality(.high)
.drawingCacheQuality(.auto)
```

!!! warning ""
    This method was deprecated in API level 28.

### duplicateParentStateEnabled

Enables or disables the duplication of the parent's state into this view.

```swift
.duplicateParentStateEnabled() // sets to true
.duplicateParentStateEnabled(false) // sets to false
```

### elevation

Sets the base elevation of this view relative to its parent.

```swift
.elevation(8) // in dp units
.elevation(8, .px) // in pixels
```

Refer to [→ units](units.md) section for more details on dimension units.

### enabled

Set the enabled state of this view.

```swift
.enabled() // sets to true
.enabled(false) // sets to false
```

### fadingEdgeLength

Set the size of the faded edge used to indicate that more content in this view is available.

```swift
.fadingEdgeLength(16) // in dp units
.fadingEdgeLength(16, .px) // in pixels
```

Refer to [→ units](units.md) section for more details on dimension units.

### filterTouchesWhenObscured

Sets whether the framework should discard touches when the view's window is obscured by another visible window at the touched location.

```swift
.filterTouchesWhenObscured() // sets to true
.filterTouchesWhenObscured(false) // sets to false
```

### fitsSystemWindows

Sets whether or not this view should account for system screen decorations such as the status bar and inset its content; that is, controlling whether the default implementation of `fitSystemWindows(Rect)` will be executed.

```swift
.fitsSystemWindows() // sets to true
.fitsSystemWindows(false) // sets to false
```

### focusable

Sets whether this view can receive focus.

```swift
.focusable() // sets to true
.focusable(false) // sets to false
```

### focusableInTouchMode

Sets whether this view can receive focus while in touch mode.

```swift
.focusableInTouchMode() // sets to true
.focusableInTouchMode(false) // sets to false
```

### focusedByDefault

Sets whether this View should receive focus when the focus is restored for the view hierarchy containing this view.

```swift
.focusedByDefault() // sets to true
.focusedByDefault(false) // sets to false
```

### forceDarkAllowed

Sets whether this view allows the system to automatically apply a dark theme to it when the system is in dark mode.

```swift
.forceDarkAllowed() // sets to true
.forceDarkAllowed(false) // sets to false
```

### foregroundGravity

Sets the gravity used to position the foreground drawable within this view.

```swift
.foregroundGravity(.center)
.foregroundGravity(.top | .start)
```

### frameContentVelocity

Sets the velocity at which content is moved within this view.

```swift
.frameContentVelocity(1.0) // normal speed
.frameContentVelocity(0.5) // half speed
.frameContentVelocity(2.0) // double speed
```

### handwritingBoundsOffsets

Set the amount of offset applied to this view's stylus handwriting bounds.

```swift
.handwritingBoundsOffsets(left: 10, top: 10, right: 5, bottom: 5) // in dp units
.handwritingBoundsOffsets(left: 10, top: 10, right: 5, bottom: 5, unit: .px) // in pixels
```

### handwritingDelegateFlags

Sets flags configuring the handwriting delegation behavior for this delegate editor view.

```swift
.handwritingDelegateFlags(.default)
.handwritingDelegateFlags(.homeDelegatorAllowed)
```

### hapticFeedbackEnabled

Set whether this view should have haptic feedback for events such as long presses.

```swift
.hapticFeedbackEnabled() // sets to true
.hapticFeedbackEnabled(false) // sets to false
```

### hasTransientState

Set whether this view is currently tracking transient state that the framework should attempt to preserve when possible.

```swift
.hasTransientState() // sets to true
.hasTransientState(false) // sets to false
```

### horizontalFadingEdgeEnabled

Define whether the horizontal edges should be faded when this view is scrolled horizontally.

```swift
.horizontalFadingEdgeEnabled() // sets to true
.horizontalFadingEdgeEnabled(false) // sets to false
```

### horizontalScrollBarEnabled

Define whether the horizontal scrollbar should be drawn or not.

```swift
.horizontalScrollBarEnabled() // sets to true
.horizontalScrollBarEnabled(false) // sets to false
```

### hovered

Sets the hovered state of this view.

```swift
.hovered() // sets to true
.hovered(false) // sets to false
```

### importantForAccessibility

Sets how to determine whether this view is important for accessibility which is if it fires accessibility events and if it is reported to accessibility services that query the screen.

```swift
.importantForAccessibility(.yes)
.importantForAccessibility(.no)
.importantForAccessibility(.noHideDescendants)
.importantForAccessibility(.auto)
```

### importantForAutofill

Sets how this view is important for autofill.

```swift
.importantForAutofill(.auto)
.importantForAutofill(.captureNo)
.importantForAutofill(.captureNoExcludeDescendants)
.importantForAutofill(.captureYes)
.importantForAutofill(.captureYesExcludeDescendants)
```

### importantForContentCapture

Sets the mode for determining whether this view is considered important for content capture.

```swift
.importantForContentCapture(.auto)
.importantForContentCapture(.captureNo)
.importantForContentCapture(.captureNoExcludeDescendants)
.importantForContentCapture(.captureYes)
.importantForContentCapture(.captureYesExcludeDescendants)
```

### isCredential

Sets whether this view is a credential for Credential Manager purposes.

```swift
.isCredential() // sets to true
.isCredential(false) // sets to false
```

### isHandwritingDelegate

Sets whether this view is a handwriting delegate editor view.

```swift
.isHandwritingDelegate() // sets to true
.isHandwritingDelegate(false) // sets to false
```

### keepScreenOn

Sets whether to keep the screen on while this view is visible to the user.

```swift
.keepScreenOn() // sets to true
.keepScreenOn(false) // sets to false
```

### keyboardNavigationCluster

Set whether this view is a root of a keyboard navigation cluster.

```swift
.keyboardNavigationCluster() // sets to true
.keyboardNavigationCluster(false) // sets to false
```

### labelFor

Sets the id of a view for which this view serves as a label for accessibility purposes.

```swift
.labelFor(anotherViewId)
```

### layoutDirection

Sets the layout direction for this view.

```swift
.layoutDirection(.ltr)
.layoutDirection(.rtl)
.layoutDirection(.locale)
```

### left

Sets the left position of this view relative to its parent.

```swift
.left(50) // in dp units
.left(50, .px) // in pixels
```

### leftTopRightBottom

Assign a size and position to this view.

```swift
.leftTopRightBottom(left: 50, top: 100, right: 250, bottom: 400) // in dp units
.leftTopRightBottom(left: 50, top: 100, right: 250, bottom: 400, unit: .px) // in pixels
```

!!! warning ""
    Low-level layout method that directly sets the view's bounding box coordinates. **Use with caution.**

### longClickable

Sets whether this view can receive long click events.

```swift
.longClickable() // sets to true
.longClickable(false) // sets to false
```

### minimumHeight

Sets the minimum height of this view.

```swift
.minimumHeight(48) // in dp units
.minimumHeight(48, .px) // in pixels
```

### minimumWidth

Sets the minimum width of this view.

```swift
.minimumWidth(48) // in dp units
.minimumWidth(48, .px) // in pixels
```

### nestedScrollingEnabled

Sets whether nested scrolling is enabled for this view.

```swift
.nestedScrollingEnabled() // sets to true
.nestedScrollingEnabled(false) // sets to false
```

### nextClusterForwardId

Sets the id of the view to use as the root of the next keyboard navigation cluster.

```swift
.nextClusterForwardId(anotherViewId)
```

### nextFocus

Sets the id of the view to use when the next focus is requested in the specified direction.

```swift
.nextFocusUpId(anotherViewId)
.nextFocusDownId(anotherViewId)
.nextFocusForwardId(anotherViewId)
.nextFocusLeftId(anotherViewId)
.nextFocusRightId(anotherViewId)
```

### onApplyWindowInsets

Set an OnApplyWindowInsetsListener to take over the policy for applying window insets to this view.

```swift
.onApplyWindowInsets { event in
    event.isSameView // check if insets are for the same view
    event.triggerView // get the view triggering the event
}
```

### onCapturedPointer

Set a listener to receive callbacks when the pointer capture state of a view changes.

```swift
.onCapturedPointer { event in
    event.motionEvent // all the info about the pointer
}
```

Read about [→ motion event](https://developer.android.com/reference/android/view/MotionEvent)

### onClick

Register a callback to be invoked when this view is clicked.

```swift
.onClick { event in
    event.isSameView // check if click is for the same view
    event.triggerView // get the view triggering the event
    Log.i("View clicked")
}
```

### onContextClick

Register a callback to be invoked when this view is context clicked.

If the view is not context clickable, it becomes context clickable.

```swift
.onContextClick { event in
    event.isSameView // check if context click is for the same view
    event.triggerView // get the view triggering the event
    Log.i("View context clicked")
}
```

### onDrag

Register a callback to be invoked when a drag event is dispatched to this view.

```swift
.onDrag { event in
    event.isSameView // check if drag event is for the same view
    event.triggerView // get the view triggering the event
    event.dragEvent // all the info about the drag event
    Log.i("View drag event")
}
```
Read about [→ drag event](https://developer.android.com/reference/android/view/DragEvent)

### onFocusChange

Register a callback to be invoked when the focus state of this view changes.

```swift
.onFocusChange { event in
    event.isSameView // check if focus change is for the same view
    event.triggerView // get the view triggering the event
    event.hasFocus // bool to check if view has focus now
    Log.i("View focus changed")
}
```

### onGenericMotion

Register a callback to be invoked when a generic motion event is dispatched to this view.

```swift
.onGenericMotion { event in
    event.isSameView // check if generic motion event is for the same view
    event.triggerView // get the view triggering the event
    event.motionEvent // all the info about the motion event
    Log.i("View generic motion event")
}
```
Read about [→ motion event](https://developer.android.com/reference/android/view/MotionEvent)

### onHover

Register a callback to be invoked when a hover event is dispatched to this view.

```swift
.onHover { event in
    event.isSameView // check if hover event is for the same view
    event.triggerView // get the view triggering the event
    event.motionEvent // all the info about the motion event
    Log.i("View hover event")
}
```
Read about [→ motion event](https://developer.android.com/reference/android/view/MotionEvent)

### onKey

Register a callback to be invoked when a hardware key event is dispatched to this view.

Key presses in software input methods will generally not trigger the methods of this listener.

```swift
.onKey { event in
    event.isSameView // check if key event is for the same view
    event.triggerView // get the view triggering the event
    event.keyEvent // all the info about the key event
    Log.i("View key event")
}
```
Read about [→ key event](https://developer.android.com/reference/android/view/KeyEvent)

### onLongClick

Register a callback to be invoked when this view is clicked and held.

Return `true` if you catch the event.

Returns `true` by default.

```swift
.onLongClick { event in
    event.isSameView // check if long click event is for the same view
    event.triggerView // get the view triggering the event
    Log.i("View long clicked")
    return true // event caught
}
// or
.onLongClick {
    Log.i("View long clicked")
}
```

### onReceiveContent

Sets the listener to be used to handle insertion of content into this view.

```swift
.onReceiveContent { event in
    event.isSameView // check if receive content event is for the same view
    event.triggerView // get the view triggering the event
    event.payload // all the content info
    Log.i("View receive content event")
}
```
Read about [→ content info](https://developer.android.com/reference/android/view/ContentInfo)

### onScrollChange

Register a callback to be invoked when the scroll X or Y position of this view changes.

```swift
.onScrollChange { event in
    event.isSameView // check if scroll change event is for the same view
    event.triggerView // get the view triggering the event
    event.scrollX // new scroll X position
    event.scrollY // new scroll Y position
    event.oldScrollX // old scroll X position
    event.oldScrollY // old scroll Y position
    Log.i("View scroll changed")
}
```

### onSystemUIVisibilityChange

Set a listener to receive callbacks when the visibility of the system bar changes.

```swift
.onSystemUIVisibilityChange { visibility in
    visibility // new visibility flags
    Log.i("View system UI visibility changed")
}
```

!!! warning ""
    This method was deprecated in API level 30.

### onTouch

Register a callback to be invoked when a touch event is dispatched to this view.

```swift
.onTouch { event in
    event.isSameView // check if touch event is for the same view
    event.triggerView // get the view triggering the event
    event.motionEvent // all the info about the motion event
    Log.i("View touch event")
}
```
Read about [→ motion event](https://developer.android.com/reference/android/view/MotionEvent)

### outlineAmbientShadowColor

Sets the color of the ambient shadow that is drawn when the view has a positive Z or elevation value.

```swift
.outlineAmbientShadowColor(.black)
```
Refer to [→ colors](colors.md) section for more details on colors.

### outlineSpotShadowColor

Sets the color of the spot shadow that is drawn when the view has a positive Z or elevation value.

```swift
.outlineSpotShadowColor(.darkGray)
```
Refer to [→ colors](colors.md) section for more details on colors.

### overScrollMode

Sets the over-scroll mode for this view.

```swift
.overScrollMode(.always)
.overScrollMode(.ifContentScrolls)
.overScrollMode(.never)
```

### padding

Sets the padding for this view.

```swift
// sets all sides
.padding(16)      // in dp
.padding(16, .px) // in pixels
// sets horizontal and vertical sides individually
.padding(h: 16, v: 8)      // in dp
.padding(h: 16, v: 8, .px) // in pixels
// sets each side individually
.padding(left: 8, top: 16, right: 8, bottom: 16)      // in dp
.padding(left: 8, top: 16, right: 8, bottom: 16, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### paddingRelative

Sets the relative padding for this view.

```swift
// sets all sides
.paddingRelative(16)      // in dp
.paddingRelative(16, .px) // in pixels
// sets horizontal and vertical sides individually
.paddingRelative(h: 16, v: 8)      // in dp
.paddingRelative(h: 16, v: 8, .px) // in pixels
// sets each side individually
.paddingRelative(start: 8, top: 16, end: 8, bottom: 16)      // in dp
.paddingRelative(start: 8, top: 16, end: 8, bottom: 16, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### pivotX

Sets the X coordinate of the pivot point around which transformations are applied to this view.

```swift
.pivotX(50) // in dp units
.pivotX(50, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### pivotY

Sets the Y coordinate of the pivot point around which transformations are applied to this view.

```swift
.pivotY(50) // in dp units
.pivotY(50, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### preferKeepClear

Set a preference to keep the bounds of this view clear from floating windows above this view's window.

This informs the system that the view is considered a vital area for the user and that ideally it should not be covered. Setting this is only appropriate for UI where the user would likely take action to uncover it.

The system will try to respect this preference, but when not possible will ignore it.

```swift
.preferKeepClear() // sets to true
.preferKeepClear(false) // sets to false
```

### pressed

Sets the pressed state of this view.

```swift
.pressed() // sets to true
.pressed(false) // sets to false
```

### requestedFrameRate

You can set the preferred frame rate for a `View` using a positive number or by specifying the preferred frame rate category using constants.

```swift
.requestedFrameRate(60) // sets preferred frame rate to 60 FPS
.requestedFrameRate(.noPreference) // system chooses the best frame rate
.requestedFrameRate(.normal) // sets default frame rate
.requestedFrameRate(.low) // sets to low power mode
.requestedFrameRate(.high) // sets to high performance mode
```

### revealOnFocusHint

Sets this view's preference for reveal behavior when it gains focus.

```swift
.revealOnFocusHint() // sets to true
.revealOnFocusHint(false) // sets to false
```

### right

Sets the right position of this view relative to its parent.

```swift
.right(250) // in dp units
.right(250, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### rotation

Sets the rotation of this view in degrees.

```swift
.rotation(45) // rotates 45 degrees
```

### rotationX

Sets the rotation of this view around the X axis in degrees.

```swift
.rotationX(30) // rotates 30 degrees
```

### rotationY

Sets the rotation of this view around the Y axis in degrees.

```swift
.rotationY(30) // rotates 30 degrees
```

### saveEnabled

Controls whether the saving of this view's state is enabled (that is, whether its onSaveInstanceState() method will be called).

```swift
.saveEnabled() // sets to true
.saveEnabled(false) // sets to false
```

### saveFromParentEnabled

Controls whether the entire hierarchy under this view will save its state when a state saving traversal occurs from its parent.

```swift
.saveFromParentEnabled() // sets to true
.saveFromParentEnabled(false) // sets to false
```

### scaleX

Sets the horizontal scaling factor of this view.

```swift
.scaleX(1.5) // scales to 150%
```

### scaleY

Sets the vertical scaling factor of this view.

```swift
.scaleY(1.5) // scales to 150%
```

### screenReaderFocusable

Sets whether this View should be a focusable element for screen readers and include non-focusable Views from its subtree when providing feedback.

```swift
.screenReaderFocusable() // sets to true
.screenReaderFocusable(false) // sets to false
```

### scrollBarDefaultDelayBeforeFade

Sets the default delay before the scrollbars fade.

```swift
.scrollBarDefaultDelayBeforeFade(1.0) // in seconds
```

### scrollBarFadeDuration

Sets the duration of the fade for scrollbars.

```swift
.scrollBarFadeDuration(0.5) // in seconds
```

### scrollBarSize

Sets the width of the scrollbars.

```swift
.scrollBarSize(4) // in dp units
.scrollBarSize(4, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### scrollBarStyle

Sets the style of the scrollbars.

```swift
.scrollBarStyle(.insideOverlay)
.scrollBarStyle(.insideInset)
.scrollBarStyle(.outsideOverlay)
.scrollBarStyle(.outsideInset)
```

### scrollCaptureHint

Sets whether this view is a hint to the system that it would like to capture scroll events for scroll capture.

```swift
.scrollCaptureHint(.auto)
.scrollCaptureHint(.exclude)
.scrollCaptureHint(.excludeDescendants)
.scrollCaptureHint(.include)
```

### scrollContainer

Change whether this view is one of the set of scrollable containers in its window.

```swift
.scrollContainer() // sets to true
.scrollContainer(false) // sets to false
```

### scrollIndicators

Sets the state of all scroll indicators.

```swift
.scrollIndicators(.top)
```

### scrollX

Sets the scrolled left position of this view.

```swift
.scrollX(100) // in dp units
.scrollX(100, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### scrollY

Sets the scrolled top position of this view.

```swift
.scrollY(100) // in dp units
.scrollY(100, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### scrollbarFadingEnabled

Sets whether the scrollbars will fade when the view is not scrolling.

```swift
.scrollbarFadingEnabled() // sets to true
.scrollbarFadingEnabled(false) // sets to false
```

### selected

Sets the selected state of this view.

```swift
.selected() // sets to true
.selected(false) // sets to false
```

### soundEffectsEnabled

Set whether this view should have sound effects enabled for events such as clicking and touching.

```swift
.soundEffectsEnabled() // sets to true
.soundEffectsEnabled(false) // sets to false
```

### stateDescription

Sets the state description of this view for accessibility purposes.

```swift
.stateDescription("Expanded")
```

### systemUiVisibility

Sets the system UI visibility flags for this view.

```swift
.systemUiVisibility(.lowProfile | .fullscreen)
```

!!! warning ""
    This method was deprecated in API level 30.

### textAlignment

Sets the text alignment for this view.

```swift
.textAlignment(.textStart)
.textAlignment(.center)
.textAlignment(.viewEnd)
// and so on...
```

### textDirection

Sets the text direction for this view.

```swift
.textDirection(.ltr)
.textDirection(.rtl)
.textDirection(.locale)
// and so on...
```

### tooltipText

Sets the tooltip text for this view.

```swift
.tooltipText("This is a tooltip")
```

### top

Sets the top position of this view relative to its parent.

```swift
.top(100) // in dp units
.top(100, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### transitionAlpha

Sets the transition alpha value for this view.

```swift
.transitionAlpha(0.5) // sets to 50% transparency
```

!!! warning ""
    This property is intended only for use by the Fade transition, which animates it to produce a visual translucency that does not side-effect (or get affected by) the real alpha property.

### transitionName

Sets the transition name for this view.

```swift
.transitionName("myTransitionView")
```

### transitionVisibility

Changes the visibility of this View without triggering any other changes.

```swift
.transitionVisibility(.visible)
.transitionVisibility(.invisible)
.transitionVisibility(.gone)
```

### translationX

Sets the horizontal translation of this view.

```swift
.translationX(50) // in dp units
.translationX(50, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### translationY

Sets the vertical translation of this view.

```swift
.translationY(50) // in dp units
.translationY(50, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### translationZ

Sets the translation of this view along the Z axis.

```swift
.translationZ(8) // in dp units
.translationZ(8, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### verticalScrollBarEnabled

Define whether the vertical scrollbar should be drawn or not.

```swift
.verticalScrollBarEnabled() // sets to true
.verticalScrollBarEnabled(false) // sets to false
```

### verticalScrollbarPosition

Sets the position of the vertical scrollbar.

```swift
.verticalScrollbarPosition(.default) // determined by the system
.verticalScrollbarPosition(.left)    // along the left edge
.verticalScrollbarPosition(.right)   // along the right edge
```

### visibility

Sets the visibility of this view.

```swift
.visibility(.visible)
.visibility(.invisible)
.visibility(.gone)
```

### willNotCacheDrawing

Sets whether this view will not cache its drawing.

```swift
.willNotCacheDrawing() // sets to true
.willNotCacheDrawing(false) // sets to false
```

!!! warning ""
    This method was deprecated in API level 28.

### willNotDraw

Sets whether this view will not draw.

```swift
.willNotDraw() // sets to true
.willNotDraw(false) // sets to false
```

### x

Sets the visual x position of this view.

```swift
.x(100) // in dp units
.x(100, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### y

Sets the visual y position of this view.

```swift
.y(100) // in dp units
.y(100, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.

### z

Sets the visual z position of this view.

```swift
.z(8) // in dp units
.z(8, .px) // in pixels
```
Refer to [→ units](units.md) section for more details on dimension units.
