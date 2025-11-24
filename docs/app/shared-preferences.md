# Shared Preferences
 
 Shared Preferences is a key-value storage mechanism that allows you to store and retrieve small amounts of primitive data types in your Android application. It is commonly used for storing user preferences, settings, and other application-specific data.

## Preferences Class

You can access it via context's `sharedPreferences` method:
 
```swift
let prefs: SharedPreferences? = self.context.sharedPreferences("my_prefs")
```
on `Activity` or `Service` you can use it directly:
```swift
let prefs: SharedPreferences? = self.sharedPreferences("my_prefs")
```
This method takes the name of the preferences file as a parameter and returns an instance of `SharedPreferences`.

## Storing Data

To store data in Shared Preferences, you need to use the `edit` method to obtain an instance of `SharedPreferences.Editor`. You can then use the editor to put key-value pairs and commit the changes.

```swift
if let editor = prefs?.edit() {
    editor.putString("username", "john_doe")
    editor.putInt("age", 30)
    editor.putBoolean("isLoggedIn", true)
    editor.apply() // or editor.commit()
}
```

## Retrieving Data

To retrieve data from Shared Preferences, you can use the various `get` methods provided by the `SharedPreferences` class. You need to provide the key and a default value in case the key does not exist.

```swift
let username: String? = prefs?.getString("username", defaultValue: "guest")
let age: Int32 = prefs?.getInt("age", defaultValue: 0)
let isLoggedIn: Bool = prefs?.getBoolean("isLoggedIn", defaultValue: false)
```

## Removing Data

To remove a specific key-value pair from Shared Preferences, you can use the `remove` method of the `SharedPreferences.Editor`.

```swift
if let editor = prefs?.edit() {
    editor.remove("username")
    editor.apply() // or editor.commit()
}
```

## Clearing All Data

To clear all data from Shared Preferences, you can use the `clear` method of the `SharedPreferences.Editor`.

```swift
if let editor = prefs?.edit() {
    editor.clear()
    editor.apply() // or editor.commit()
}
```

This will remove all key-value pairs from the specified Shared Preferences file.

## Commit vs Apply

When saving changes to Shared Preferences, you have two options: `commit` and `apply`.

- `commit`: This method writes the changes synchronously and returns a boolean indicating whether the operation was successful. It is recommended to use this method when you need to ensure that the changes are saved immediately.

- `apply`: This method writes the changes asynchronously and does not return a value. It is generally faster than `commit` and is recommended for most use cases where immediate confirmation of the save operation is not required.