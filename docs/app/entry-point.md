# Entry Point

The entry point of your Swift Android application is defined in the `App.swift` file located in the `Sources/App` directory:

```json
📦 Project Root
├── 📄 Package.swift
└── 📂 Sources
    └── 📂 App
        └── 📄 App.swift
```

This file imports the **Droid framework**, defines **@main** class of your app, and configures lifecycle, gradle dependencies, and manifest.

```swift
import Droid

@main
public final class App: DroidApp {
	@AppBuilder public override var body: Content {
        Lifecycle.didFinishLaunching{
			App.setLogLevel(.debug)
			App.setInnerLogLevel(.debug)
			Log.i("🚀 didFinishLaunching")
		}
		Manifest
			.usesPermissions(.camera)
			.usesFeatures(.hardwareCamera)
			.application {
				.allowBackup()
				.icon("@mipmap/ic_launcher")
				.roundIcon("@mipmap/ic_launcher_round")
				.label("My app")
				.activities(
					MainActivity.self,
					SecondActivity.self,
					ThirdActivity.self,
					FourthActivity.self
				)
			}
		GradleDependencies
			.implementation("com.some.company:library:1.0.0")
			.debugImplementation("com.some.company:library:1.0.0-nightly")
			.testImplementation("com.some.company:test-library:1.0.0")
    }
}
```

## Lifecycle

This allows catching different lifecycle events of your application.

Only `didFinishLaunching` is currently supported.

## Manifest

It represents the [→ AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro) in a convenient declarative way. 

Swift Stream IDE then generates the corresponding XML file during the build process.

Head to [→ manifest](manifest.md) section for more details.

## Gradle Dependencies

You can declare additional Gradle dependencies for your app directly as shown above.

But most of them are **automatically managed** based on the components you use in your Swift code. **For example**, once you use Android X components, the corresponding dependencies are added automatically, same for the Material, Flexbox, etc.