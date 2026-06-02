# AppCompatButton

`AppCompatButton` is an AppCompat button with controls for EmojiCompat fallback in its text. Use these controls when the button’s text may contain emoji that EmojiCompat should attempt to supply.

It inherits the text and view behavior of `Button`. The methods below let you enable or disable EmojiCompat fallback on an existing button and read back its enabled state.

## Use it when…

Use `AppCompatButton` when you need to choose whether an existing AppCompat button attempts EmojiCompat fallback for missing emoji in its label.

## How it behaves

Calling `emojiCompatEnabled()` enables attempts to use EmojiCompat for missing emoji. Passing `false` prevents that fallback. The setting is applied to the underlying button or queued until its instance is available.

`isEmojiCompatEnabled()` returns the enabled state reported by the underlying button. It returns `false` if the Java instance or call result is unavailable. This Boolean is API readback, not proof that an emoji was rendered successfully.

## Examples

### Enable fallback for missing emoji

This enables EmojiCompat fallback on an existing button. With correct device configuration, EmojiCompat can provide fallback for missing emoji.

```swift
@MainActor
func enableEmojiFallback(on button: AppCompatButton) {
    button.emojiCompatEnabled()
}
```

### Disable fallback and read the setting

This disables EmojiCompat fallback and returns the API’s enabled-state readback. The returned value does not verify the button’s visible text.

```swift
@MainActor
func disableEmojiFallback(on button: AppCompatButton) -> Bool {
    button.emojiCompatEnabled(false)
    return button.isEmojiCompatEnabled()
}
```

## Methods

### `emojiCompatEnabled`

Enable or disable EmojiCompat fallback. This method is inherited from `View` and returns the button for chaining; you can also ignore the return value. Omitting the argument passes `true`, which does not establish the initial state of a newly constructed button.

```swift
@MainActor
func disableEmojiFallback(on button: AppCompatButton) {
    button.emojiCompatEnabled(false)
}
```

### `isEmojiCompatEnabled`

Read whether the underlying button reports EmojiCompat as enabled. An unavailable Java instance or call result produces `false`.

```swift
@MainActor
func emojiFallbackIsEnabled(on button: AppCompatButton) -> Bool {
    button.isEmojiCompatEnabled()
}
```

## Notes

EmojiCompat must be correctly configured on the device for fallback to have a visible effect. This configuration is available by default when a correct downloadable-fonts provider is installed. If you initialize EmojiCompat manually after constructing the button, you may call `emojiCompatEnabled()` again to enable fallback.
