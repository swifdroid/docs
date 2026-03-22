# QuickContactBadge

`QuickContactBadge` is Droid's wrapper for Android's contact badge. It inherits `ImageView`, adds queued quick-contact configuration, and exposes live contact-assignment actions.

## Use it when…

Use `QuickContactBadge` when an Android contact avatar should be associated with an email address, phone number, or contact URI.

For a plain avatar without quick-contact behavior, use `ImageView`.

## How it behaves

Queued Droid view properties:

- `mode(_:)`
- `overlay(_:)`
- `prioritizedMimeType(_:)`
- `excludeMimes(_:)`

Contact assignment and default-image actions operate on the live Android instance.

Image assignment and tint behavior are inherited from `ImageView`.

## Example

### Configure badge presentation

```swift
QuickContactBadge()
    .imageResource(1001)
    .prioritizedMimeType("vnd.android.cursor.item/phone_v2")
    .excludeMimes(["vnd.android.cursor.item/email_v2"])
```

### Assign a contact on a live badge

```swift
func assignContact(_ badge: QuickContactBadge) {
    badge.assignContactFromEmail("person@example.com", true)
}
```

The assignment method requires a live Android instance.

## Initializers

```swift
QuickContactBadge()
QuickContactBadge(id: 1001)
```

## Contact assignment

`assignContactFromEmail(_:_:)` assigns an email address with a lazy-lookup flag.

An overload also accepts optional `Bundle` extras.

`assignContactFromPhone(_:_:)` provides the corresponding phone-number forms.

`assignContactUri(_:)` accepts an optional JNI-compatible Android `Uri` object.

All assignment methods are live calls.

## Quick-contact configuration

`mode(_:)`, `overlay(_:)`, `prioritizedMimeType(_:)`, and `excludeMimes(_:)` are queued.

`setImageToDefault()` is a live action that asks Android to restore the badge's default avatar image.

## Inherited image behavior

Because `QuickContactBadge` inherits `ImageView`, it also receives queued drawable/resource/URI/icon assignment, tinting, bounds adjustment, and max-size configuration.

## Lower-level helpers

The wrapper forwards drawable hotspot/state changes, attach-to-window, click, and drawing callbacks for low-level integrations.
