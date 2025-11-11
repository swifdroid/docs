# Manifest

```json
📦 Project Root
├── 📄 Package.swift
└── 📂 Sources
    └── 📂 App
        └── 📄 App.swift  # Entry Point
```

As described in [→ entry point](entry-point.md) **`App.swift`** is the main place where you define your Android application structure, including the manifest:
```swift
@main
public final class App: DroidApp {
	@AppBuilder public override var body: Content {
        Manifest
			.usesPermission(.camera)
			.application {
				.label("My app")
				.activities(
					MainActivity.self
				)
			}
    }
}
```

Swift Stream IDE then executes special **`AppManifest`** proxy target to generate the corresponding [→ AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro) file based on your declarative manifest definition in **`App.swift`**.

Droid framework carefully provides all possible manifest options, so you can fully configure your app’s manifest right from Swift code.

## application

Defines your app's core settings, like its name, icon, backup options, activities, and more.

### activity

List all the activities of your application here:
```swift
.activities(
	MainActivity.self,
	SecondActivity.self,
	ThirdActivity.self,
	FourthActivity.self
)
```
Or list them one by one:
```swift
.activity(MainActivity.self)
.activity(SecondActivity.self)
.activity(ThirdActivity.self)
.activity(FourthActivity.self)
```

Setup of each activity is done in its own file, see [→ activities](activities.md)

### activity-alias

An activity alias provides an alternate entry point to an existing activity, allowing you to define additional intent filters or metadata for the same activity.

```swift
.activityAlias {
	.enabled(true)
	.exported(false)
	.icon("@mipmap/ic_activity_alias")
	.label("Activity Alias Example")
	.name("ActivityAliasName")
	.permission(.camera)
	.targetActivity(MainActivity.self)
	.intentFilter {
		.category(.launcher)
		.action(.mediaMounted)
		// and other params...
	}
	.metaData {
		.name("key").value("value").resource("")
	}
}
```
Details about each attribute in the official [→ documentation](https://developer.android.com/guide/topics/manifest/activity-alias-element)

### allowTaskReparenting

Whether activities that the application defines can move from the task that started them to the task they have an affinity for when that task is next brought to the front [→ learn more](https://developer.android.com/guide/topics/manifest/activity-element#reparent)

```swift
.allowTaskReparenting(true)
```

### allowBackup

Whether to let the application participate in the backup and restore infrastructure [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#backup)

```swift
.allowBackup(true)
```

### allowClearUserData

Whether to let the application reset user data [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#clearUserData)

```swift
.allowClearUserData(true)
```

### allowNativeHeapPointerTagging

Whether the app enables the Heap pointer tagging feature [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#nativeHeapPointerTagging)

```swift
.allowNativeHeapPointerTagging(true)
```

### appCategory

Declares the category of this app. Categories are used to cluster multiple apps together into meaningful groups, such as when summarizing battery, network, or disk usage. Only define this value for apps that fit well into one of the specific categories [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#appCategory)

```swift
.appCategory(.game)
```

### backupAgent

The name of the backup agent class for the application [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#backupAgent)

```swift
.backupAgent("com.example.MyBackupAgent")
```

### banner

A drawable resource to use as the application's banner [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#banner)

```swift
.banner("@drawable/app_banner")
```

### dataExtractionRules

Pointer to XML resource with the rules determining which files and directories can be copied from the device as part of backup or transfer operations. [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#dataExtractionRules)

```swift
.dataExtractionRules("@xml/data_extraction_rules")
```

### debuggable

Whether the application can be debugged, even when running on a device in user mode [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#debuggable)

```swift
.debuggable(true)
```

### description

User-readable text about the application, which is longer and more descriptive than the application label [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#description)

```swift
.description("This is my awesome Swift Android application!")
```

### enabled

Whether the Android system can instantiate components of the application [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#enabled)

```swift
.enabled(true)
```

### enableOnBackInvokedCallback

This flag lets you opt out of predictive system animations at the app level [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#enableOnBackInvokedCallback)

```swift
.enableOnBackInvokedCallback(true)
```

### extractNativeLibs

!!! info ""
	Starting with AGP 4.2.0 it is replaced by `useLegacyPackaging`

Indicates whether the package installer extracts native libraries from the APK to the file system. If set to "false", your native libraries are stored uncompressed in the APK. Although your APK might be larger, your application loads faster because the libraries load directly from the APK at runtime [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#extractNativeLibs)

```swift
.extractNativeLibs(true)
```

> The default value depends on `minSdkVersion` and the version of AGP you're using. In most cases, the default behavior is probably what you want, and you don't have to set this attribute explicitly.

### fullBackupContent

Points to an XML file that contains full backup rules for Auto Backup [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#fullBackupContent)

```swift
.fullBackupContent("@xml/full_backup_content")
```

### fullBackupOnly

 Indicates whether to use Auto Backup on devices where it is available [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#fullBackupOnly)

```swift
.fullBackupOnly(true)
```

### gwpAsanMode

Indicates whether to use GWP-ASan, a native memory allocator feature that helps find use-after-free and heap-buffer-overflow bugs [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#gwpAsanMode)

```swift
.gwpAsanMode(.never)
```

### hasCode

Whether the application contains any DEX code [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#hasCode)

```swift
.hasCode(true)
```

### hasFragileUserData

Whether to show the user a prompt to keep the app's data when the user uninstalls the app [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#hasFragileUserData)

```swift
.hasFragileUserData(true)
```

### hardwareAccelerated

Whether hardware-accelerated rendering is enabled for all activities and views in this application [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#hardwareAccelerated)

```swift
.hardwareAccelerated(true)
```

### icon

An icon for the application as whole and the default icon for each of the application's components [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#icon)

```swift
.icon("@mipmap/ic_launcher")
```

### isMonitoringTool

Indicates that this application is designed to monitor other individuals [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#isMonitoringTool) and [→ even more](https://support.google.com/googleplay/android-developer/answer/12955211?hl=en)

```swift
.isMonitoringTool(.childMonitoring)
```

### killAfterRestore

Whether the application terminates after its settings have been restored during a full-system restore operation [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#killAfterRestore)

```swift
.killAfterRestore(true)
```

### largeHeap

Whether the application's processes are created with a large Dalvik heap [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#largeHeap)

```swift
.largeHeap(true)
```

### label

The `label` attribute specifies the name of the application as it appears to users on their device, such as on the home screen or in the app settings. It helps users identify your app among others [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#label)

```swift
.label("My app")
```

### logo

Specifies a drawable resource used as your app’s branding image in certain system UI elements, such as the Action Bar or app title area. It’s optional and distinct from the main app icon. If not set, the icon or no image is shown instead [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#logo)

```swift
.logo("@drawable/logo")
```

### manageSpaceActivity

Specifies a custom Activity that the system can launch to let users manage your app’s storage (e.g., clear cached or downloaded data). You must implement this activity and declare it in your manifest [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#space)

```swift
.manageSpaceActivity("ManageSpaceActivity.self")
```
Assuming that you already have `ManageSpaceActivity` activity class.

!!! warning ""
	Modern Android versions often handle this automatically, so this attribute is optional and rarely needed

### name

The fully qualified name of an Application subclass implemented for the application [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#nm)

```swift
.name("com.example.MyApplication")
```

!!! danger ""
	This setting is already set under the hood to **`stream.swift.droid.appkit.DroidApp`**.  
	**Changing it may lead to unexpected behavior!**

### networkSecurityConfig

Points to an XML resource file that configures the network security settings for the application [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#networkSecurityConfig) and [→ even more](https://developer.android.com/training/articles/security-config)

```swift
.networkSecurityConfig("@xml/network_security_config")
```

### permission

Specifies a default permission that other apps must hold to interact with your app’s components (e.g., activities, services, receivers). It applies globally unless overridden by individual components’ own `permission`. Useful for restricting access to exported features of your app [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#prmsn) and check [→ security tips](https://developer.android.com/privacy-and-security/security-tips)

```swift
.permission("com.example.myapp.ACCESS_APP_FEATURES")
```

### persistent

Whether the application remains running at all times [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#persistent)

```swift
.persistent(true)
```

!!! warning ""
	Applications don't normally set this flag  
	Persistence mode is intended only for certain system applications

### process

Defines the name of the process in which the app or component should run. If set on the `application`, it applies to all components unless overridden individually. Process names starting with `:` create private secondary processes, fully qualified names allow shared processes across apps. Useful for isolating heavy or background tasks [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#proc)

```swift
.process(":remote")
```

> And if you have a service it may have e.g. `:background` process name.

### restoreAnyVersion

Indicates that the application is prepared to attempt a restore of any backed-up data set, even if the backup was stored by a newer version of the application than is currently installed on the device [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#restoreany)

```swift
.restoreAnyVersion(true)
```

### requestLegacyExternalStorage

Specifies whether the app should use the legacy external storage model (pre–Android 10). When set to `true`, it allows unrestricted file access on Android 10 (API 29) devices. Ignored on Android 11 (API 30) and higher, where scoped storage is always enforced [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#requestLegacyExternalStorage)

```swift
.requestLegacyExternalStorage(true)
```

!!! warning ""
	Intended only as a temporary migration aid

### requiredAccountType

Specifies the account type that your app requires, usually corresponding to an account managed by Android’s AccountManager (e.g., for sync or authentication) [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#requiredAccountType)

```swift
.requiredAccountType("com.example.account")
```

!!! warning ""
	Used mainly by system or enterprise apps that depend on a specific account type

### resizeableActivity

Specifies whether your app or activity supports multi-window mode (split-screen, freeform, or PiP). When true, the activity can be resized; when false, it runs fullscreen [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#resizeableActivity)

```swift
.resizeableActivity(true)
```

!!! warning ""
	Starting with Android 16 (API 36), this attribute is ignored on large-screen devices (≥ 600dp smallest width), as all apps are treated as resizable

### restrictedAccountType

Specifies the type of account that the app can create and manage within restricted user profiles (e.g., limited-access tablet profiles). Typically used with `AccountManager` and custom account types [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#restrictedAccountType)

```swift
.restrictedAccountType("com.example.restricted")
```

!!! warning ""
	Mostly relevant for enterprise or education apps

### supportsRtl

Declares whether your application is willing to support right-to-left (RTL) layouts [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#supportsrtl)

```swift
.supportsRtl(true)
```

### taskAffinity

Specifies the logical task group an activity belongs to. Activities with the same affinity share the same task (back stack). Changing this value allows activities to open in separate tasks — useful for document-style apps, deep links, or isolating flows [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#aff)

```swift
.taskAffinity("com.example.myapp.TASK_AFFINITY")
```

> Defaults to the app’s package name

### testOnly

Indicates whether this application is only for testing purposes [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#testOnly)

```swift
.testOnly(true)
```

### theme

A reference to a style resource defining a default theme for all activities in the application. Individual activities can override the default by [→ setting their own](activities.md#theme) theme [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#theme)

```swift
.theme(.material3DayNightNoActionBar)
```

### uiOptions

Specifies special UI behavior for the application or activity, mainly affecting the Action Bar. Common values include splitActionBarWhenNarrow, which moves overflow items to a bottom bar on narrow screens [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#uioptions)

```swift
.uiOptions(.splitActionBarWhenNarrow)
```

!!! warning ""
	Rarely needed in modern apps using Toolbar or Material Design components

### usesCleartextTraffic

Specifies whether the app allows cleartext (unencrypted) network traffic such as HTTP.  
Set to `true` only if your app must connect to legacy HTTP servers [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic)

```swift
.usesCleartextTraffic(false)
```

> The default value for apps that target API level 27 or lower is "true".  
Apps that target API level 28 or higher default to "false".

!!! note ""
	WebView honors this attribute for applications targeting API level 26 and higher

### vmSafeMode

Indicates whether the app should run in VM safe mode, disabling certain Dalvik/ART optimizations for stability [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#vmSafeMode)

```swift
.vmSafeMode(true)
```

!!! warning ""
	Intended for debugging or very old devices and ignored on modern Android versions

## backupInForeground

Whether backups can be performed while the application is in the foreground [→ learn more](https://developer.android.com/guide/topics/manifest/application-element#backupInForeground)

```swift
.backupInForeground(true)
```

## compatibleScreen

Specifies the screen sizes with which the application is compatible [→ learn more](https://developer.android.com/guide/topics/manifest/compatible-screens-element)

```swift
.compatibleScreens {
	.screen(.normal, .hdpi)
	.screen(.normal, .xhdpi)
	.screen(.large, .hdpi)
}
```

!!! danger ""
	Using this element can greatly reduce the potential user base for your application, by not allowing it to be installed on devices with screen configurations not listed here. Normally, you should not use this manifest element at all.

## installLocation

The preferred install location for the application [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#install)

```swift
.installLocation(.auto)
```
Accepts `internalOnly`, `preferExternal`, and `auto` values.

## instrumentation

Specifies an instrumentation class that runs against the application [→ learn more](https://developer.android.com/guide/topics/manifest/instrumentation-element)

```swift
.instrumentation {
	.functionalTest(true)
	.handleProfiling(true)
	.icon("@mipmap/some_icon")
	.label("Instrumentation Label")
	.name("InstrumentationName")
	.targetPackage("com.example.targetpackage")
	.targetProcesses("com.example.targetprocess")
}
```

## package

A full Java-language-style package name for the Android app [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#package)

```swift
.package("com.example.myapp")
```

## permission

Declares a security permission that the application must have in order to be granted access to a particular component or set of components [→ learn more](https://developer.android.com/guide/topics/manifest/permission-element)

```swift
.permission {
    .description("Allows access to the camera device")
    .icon("@drawable/ic_camera_permission")
    .label("Camera Access")
    .name(.camera)
    .permissionGroup(.camera)
    .protectionLevel(.dangerous)
}
```

## permissionGroup

Declares a group of related security permissions [→ learn more](https://developer.android.com/guide/topics/manifest/permission-group-element)

```swift
.permissionGroup {
    .description("Group of permissions related to camera access")
    .icon("@drawable/ic_camera_group")
    .label("Camera Permissions")
    .name(.camera)
}
```

## permissionTree

Declares a tree of related security permissions [→ learn more](https://developer.android.com/guide/topics/manifest/permission-tree-element)

```swift
.permissionTree {
    .icon("@drawable/ic_permission_tree")
    .label("Permission Tree Example")
    .name("com.example.permission.TREE")
}
```

## protectionLevel

Specifies the level of protection for the permission being declared [→ learn more](https://developer.android.com/guide/topics/manifest/permission-element#plevel)

```swift
.protectionLevel(.dangerous)
```
Accepts `normal`, `dangerous`, `signature`, and `signatureOrSystem` values

## sharedUserId

The name of a Linux user ID that will be shared with other applications [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#uid)

```swift
.sharedUserId("com.example.shareduserid")
```
!!! danger ""
	This constant is deprecated as of API level 29

## sharedUserLabel

The user-visible label for the shared user ID [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#uidlabel)

```swift
.sharedUserLabel("Shared User")
```

!!! danger ""
	This constant is deprecated as of API level 29

## usesPermission

Declares a permission that the application must have in order to access a particular feature or API [→ learn more](https://developer.android.com/guide/topics/manifest/uses-permission-element)

```swift
.usesPermission(.camera)
```

## usesFeature

Declares a hardware or software feature that the application requires or can use [→ learn more](https://developer.android.com/guide/topics/manifest/uses-feature-element)

```swift
.usesFeature(.hardwareCamera)
```

## versionCode

An integer value that represents the version of the application code, relative to other versions [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#vcode)

```swift
.versionCode(1)
```

## versionName

A string value that represents the release version of the application code, as it should be shown to users [→ learn more](https://developer.android.com/guide/topics/manifest/manifest-element#vname)

```swift
.versionName("1.0.0")
```