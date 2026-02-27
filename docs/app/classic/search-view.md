# SearchView

`SearchView` is Android's dedicated search widget. The current Droid wrapper inherits `LinearLayout` and exposes query text, hints, iconified state, submit behavior, suggestion integration, searchable metadata, and listener bridges.

## Use it when…

Use `SearchView` when search is a first-class interaction and you want Android's dedicated expand/collapse, query, submit, and suggestion behavior rather than a generic text field.

## How it behaves

Query text, query hint, IME/input options, max width, iconified state, submit-button state, query refinement, listeners, searchable info, and suggestions adapter are queued Droid view properties.

Getter and lifecycle methods operate on the live Android instance.

## Examples

### Build an expanded search field

```swift
SearchView()
    .queryHint("Search")
    .iconifiedByDefault(false)
    .iconified(false)
    .submitButtonEnabled(true)
```

### Preload a query

```swift
SearchView()
    .query("swift", submit: false)
```

Pass `submit: true` only when setting the query should also submit it through Android.

## Initializers

```swift
SearchView()
SearchView(id: 1001)
```

## Methods

### Query text and hint

`query(_:, submit:)` queues the text and optional submit flag. `queryHint(_:)` queues the hint.

`query()` and `queryHint()` read the current live values.

### Expand and collapse state

`iconifiedByDefault(_:)` configures the resting state. `iconified(_:)` expands or collapses the search view.

`iconified()`, `isIconified()`, `iconifiedByDefault()`, and `isIconifiedByDefault()` read live state.

### Submit and refinement

`submitButtonEnabled(_:)` controls whether Android may show the submit button for a non-empty query.

`queryRefinementEnabled(_:)` controls query-refinement affordances for suggestions.

### IME and input configuration

`imeOptions(_:)`, `inputType(_:)`, `maxWidth(_:)`, and `appSearchData(_:)` are queued configuration properties. Their getter counterparts read current live state where available.

### Suggestions and listeners

`suggestionsAdapter(_:)`, `searchableInfo(_:)`, `onQueryTextListener(_:)`, `onSuggestionListener(_:)`, `onCloseListener(_:)`, `onSearchClickListener(_:)`, and `onQueryTextFocusChangeListener(_:)` accept JNI-compatible objects.

Droid exposes these bridges but does not construct those Android adapter/listener/searchable objects for you.

### Action-view and lifecycle helpers

`onActionViewExpanded()`, `onActionViewCollapsed()`, focus/state methods, measurement/layout hooks, and key handling are available for lower-level integrations.
