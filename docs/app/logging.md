# Logging

## Simply Print

You can simply use `print` statements and they will appear in **LogCat**.

```swift
print("Hello from Swift!")
```
In **LogCat** it will look like this:
```
I/Swift: Hello from Swift!
```
So all `print` statements are logged with the **`Swift`** tag and the **`INFO`** log level.

!!! success ""
    These `print` calls are the simplest form of logging which works without any additional setup

## Fine Grained Logging

You have an option to use more advanced logging to see different log levels in **LogCat**.

### Setup

Set it up in [→ entry point](entry-point.md) of your application like this:

```swift
@main
public final class App: DroidApp {
	@AppBuilder public override var body: Content {
        Lifecycle.didFinishLaunching{
			App.setLogLevel(.debug)
			App.setInnerLogLevel(.debug)
			Log.i("🚀 didFinishLaunching")
		}
        // ...
    }
}
```

This sets the global log level to **DEBUG** so all log messages with level **DEBUG** and higher will be shown in **LogCat**.
```swift
App.setLogLevel(.debug)
```
It is for your own log messages.

```swift
App.setInnerLogLevel(.debug)
```
It is for internal log messages from the Droid framework. Enable it for debugging purposes. Disabled by default.

### Log Levels

You can use the following log levels:

- `trace`: Very detailed logging, typically only enabled for development.
- `debug`: Detailed logging for debugging purposes.
- `info`: General information about the application's operation.
- `notice`: Important information that is not an error.
- `warning`: Indications of potential issues or important situations.
- `error`: Errors that occur during the application's operation.
- `critical`: Severe errors that require immediate attention.

### Usage

You can use the following methods to log messages at different levels:

```swift
Log.t("Trace message")    // V/App: Trace message
Log.d("Debug message")    // D/App: Debug message
Log.i("Info message")     // I/App: Info message
Log.n("Notice message")   // I/App: Notice message
Log.w("Warning message")  // W/App: Warning message
Log.e("Error message")    // E/App: Error message
Log.c("Critical message") // F/App: Critical message
```

If you set the log level to **DEBUG**, you will see all messages from **DEBUG** to **CRITICAL**.  
So only the **TRACE** messages will be ignored.

!!! success ""
    These log calls can be used anywhere in your Swift code and even can be enabled/disabled at runtime

## JNI Logs

Underlying JNIKit also has its own logging mechanism.

You can enable it in sidebar panel of Swift Stream IDE by enabling **`JNI Logs`** option.

!!! warning ""
    Be careful, it will produce a lot of noise in LogCat, but it can be useful for crash investigations
