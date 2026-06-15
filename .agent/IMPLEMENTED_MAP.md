# Implemented Map

Durable, verified page-to-source navigation map for authored `docs/docs/app/**` content.

## Role and maintenance

- This map is evidence/navigation, not framework authority; inspect current `droid/Sources/Droid` before changing claims or examples.
- Include a page only after its substantive content has been checked against the listed implementation.
- Update mappings when source ownership moves or a page's covered surface changes.
- Do not encode completion history here; compact outcomes belong in `DONE.md`.
- `.agent/architecture/DOCUMENTATION_SYSTEM.md` owns coverage, source-verification, navigation, and tracking semantics.

## Verified mappings

- `docs/docs/app/index.md` -> `droid/Sources/Droid/Droid.swift`, `droid/Sources/Droid/DroidApp.swift`
- `docs/docs/app/getting-started.md` -> `droid/Sources/Droid/DroidApp.swift`, `droid/Sources/Droid/AppBuilder.swift`
- `docs/docs/app/structure.md` -> `droid/Sources/Droid/DroidApp+GenerateMain.swift`, `droid/Sources/Droid/DroidApp+GenerateActivity.swift`, `droid/Sources/Droid/DroidApp+GenerateFragment.swift`
- `docs/docs/app/entry-point.md` -> `droid/Sources/Droid/DroidApp.swift`, `droid/Sources/Droid/AppBuilder.swift`, `droid/Sources/Droid/AppLifecycle.swift`, `droid/Sources/Droid/Manifest/AppManifest.swift`, `droid/Sources/Droid/AppGradleDependencies.swift`
- `docs/docs/app/logging.md` -> `droid/Sources/Droid/Droid.swift`, `droid/Sources/Droid/DroidApp.swift`
- `docs/docs/app/manifest.md` -> `droid/Sources/Droid/Manifest/AppManifest.swift`, `droid/Sources/Droid/Manifest/DroidApp+Application.swift`, `droid/Sources/Droid/Manifest/DroidApp+Activity.swift`, `droid/Sources/Droid/Manifest/DroidApp+IntentFilter.swift`
- `docs/docs/app/activities.md` -> `droid/Sources/Droid/Activities/Activity.swift`, `droid/Sources/Droid/Activities/AnyActivity.swift`, `droid/Sources/Droid/Activities/AppCompatActivity.swift`
- `docs/docs/app/views.md` -> `droid/Sources/Droid/Packages/android/view/View.swift`, `droid/Sources/Droid/Packages/android/view/ViewGroup.swift`, `droid/Sources/Droid/BodyBuilder.swift`
- `docs/docs/app/assets.md` -> `droid/Sources/Droid/Resources.swift`, `droid/Sources/Droid/R.swift`
- `docs/docs/app/r.md` -> `droid/Sources/Droid/R.swift`
- `docs/docs/app/colors.md` -> `droid/Sources/Droid/Packages/android/graphics/GraphicsColor.swift`
- `docs/docs/app/themes.md` -> `droid/Sources/Droid/RStyle.swift`
- `docs/docs/app/for-each.md` -> `droid/Sources/Droid/ForEach.swift`
- `docs/docs/app/state.md` -> `droid/Sources/Droid/State.swift`
- `docs/docs/app/units.md` -> `droid/Sources/Droid/TypedValue.swift`, `droid/Sources/Droid/Enums/DimensionUnit.swift`
- `docs/docs/app/permissions.md` -> `droid/Sources/Droid/Manifest/ManifestPermission.swift`, `droid/Sources/Droid/Manifest/DroidApp+UsesPermission.swift`
- `docs/docs/app/shared-preferences.md` -> `droid/Sources/Droid/SharedPreferences.swift`
- `docs/docs/app/x/recycler-view.md` -> `droid/Sources/Droid/Packages/androidx/recyclerview/widget/RecyclerView.swift`
- `docs/docs/app/classic/absolute-layout.md` -> `droid/Sources/Droid/Packages/android/widget/AbsoluteLayout.swift`
- `docs/docs/app/classic/button.md` -> `droid/Sources/Droid/Packages/android/widget/Button.swift`
- `docs/docs/app/classic/edit-text.md` -> `droid/Sources/Droid/Packages/android/widget/EditText.swift`
- `docs/docs/app/classic/frame-layout.md` -> `droid/Sources/Droid/Packages/android/widget/FrameLayout.swift`
- `docs/docs/app/classic/linear-layout.md` -> `droid/Sources/Droid/Packages/android/widget/LinearLayout.swift`
- `docs/docs/app/classic/relative-layout.md` -> `droid/Sources/Droid/Packages/android/widget/RelativeLayout.swift`
- `docs/docs/app/classic/text-view.md` -> `droid/Sources/Droid/Packages/android/widget/TextView.swift`
- `docs/docs/app/classic/space.md` -> `droid/Sources/Droid/Packages/android/widget/Space.swift`
- `docs/docs/app/classic/h-space.md` -> `droid/Sources/Droid/Packages/android/widget/HSpace.swift`
- `docs/docs/app/classic/v-space.md` -> `droid/Sources/Droid/Packages/android/widget/VSpace.swift`
- `docs/docs/app/classic/h-stack.md` -> `droid/Sources/Droid/Packages/android/widget/HStack.swift`
- `docs/docs/app/classic/v-stack.md` -> `droid/Sources/Droid/Packages/android/widget/VStack.swift`
- `docs/docs/app/classic/adapter-view.md` -> `droid/Sources/Droid/Packages/android/widget/AdapterView.swift`, `droid/Sources/Droid/Packages/android/widget/Adapter.swift`
- `docs/docs/app/classic/action-provider.md` -> `droid/Sources/Droid/Packages/android/view/ActionProvider.swift`, `droid/Sources/Droid/Packages/android/view/Menu.swift`
- `docs/docs/app/classic/progress-bar.md` -> `droid/Sources/Droid/Packages/android/widget/ProgressBar.swift`
- `docs/docs/app/classic/abs-seek-bar.md` -> `droid/Sources/Droid/Packages/android/widget/AbsSeekBar.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ProgressBar.swift`
- `docs/docs/app/classic/seek-bar.md` -> `droid/Sources/Droid/Packages/android/widget/SeekBar.swift`, inherited `droid/Sources/Droid/Packages/android/widget/AbsSeekBar.swift`, `droid/Sources/Droid/Packages/android/widget/ProgressBar.swift`
- `docs/docs/app/classic/rating-bar.md` -> `droid/Sources/Droid/Packages/android/widget/RatingBar.swift`, inherited `droid/Sources/Droid/Packages/android/widget/AbsSeekBar.swift`, `droid/Sources/Droid/Packages/android/widget/ProgressBar.swift`
- `docs/docs/app/classic/compound-button.md` -> `droid/Sources/Droid/Packages/android/widget/CompoundButton.swift`
- `docs/docs/app/classic/check-box.md` -> `droid/Sources/Droid/Packages/android/widget/CheckBox.swift`, inherited `droid/Sources/Droid/Packages/android/widget/CompoundButton.swift`
- `docs/docs/app/classic/radio-button.md` -> `droid/Sources/Droid/Packages/android/widget/RadioButton.swift`, inherited `droid/Sources/Droid/Packages/android/widget/CompoundButton.swift`
- `docs/docs/app/classic/switch.md` -> `droid/Sources/Droid/Packages/android/widget/Switch.swift`, inherited `droid/Sources/Droid/Packages/android/widget/CompoundButton.swift`
- `docs/docs/app/classic/toggle-button.md` -> `droid/Sources/Droid/Packages/android/widget/ToggleButton.swift`
- `docs/docs/app/classic/auto-complete-text-view.md` -> `droid/Sources/Droid/Packages/android/widget/AutoCompleteTextView.swift`, inherited `droid/Sources/Droid/Packages/android/widget/EditText.swift`
- `docs/docs/app/classic/multi-auto-complete-text-view.md` -> `droid/Sources/Droid/Packages/android/widget/MultiAutoCompleteTextView.swift`
- `docs/docs/app/classic/search-view.md` -> `droid/Sources/Droid/Packages/android/widget/SearchView.swift`
- `docs/docs/app/classic/share-action-provider.md` -> `droid/Sources/Droid/Packages/android/view/ActionProvider.swift`
- `docs/docs/app/classic/spinner.md` -> `droid/Sources/Droid/Packages/android/widget/Spinner.swift`
- `docs/docs/app/classic/number-picker.md` -> `droid/Sources/Droid/Packages/android/widget/NumberPicker.swift`
- `docs/docs/app/classic/chronometer.md` -> `droid/Sources/Droid/Packages/android/widget/Chronometer.swift`, inherited `droid/Sources/Droid/Packages/android/widget/TextView.swift`
- `docs/docs/app/classic/text-clock.md` -> `droid/Sources/Droid/Packages/android/widget/TextClock.swift`, inherited `droid/Sources/Droid/Packages/android/widget/TextView.swift`
- `docs/docs/app/classic/date-picker.md` -> `droid/Sources/Droid/Packages/android/widget/DatePicker.swift`, inherited `droid/Sources/Droid/Packages/android/widget/FrameLayout.swift`
- `docs/docs/app/classic/time-picker.md` -> `droid/Sources/Droid/Packages/android/widget/TimePicker.swift`
- `docs/docs/app/classic/view-animator.md` -> `droid/Sources/Droid/Packages/android/widget/ViewAnimator.swift`, inherited `droid/Sources/Droid/Packages/android/widget/FrameLayout.swift`
- `docs/docs/app/classic/view-flipper.md` -> `droid/Sources/Droid/Packages/android/widget/ViewFlipper.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ViewAnimator.swift`
- `docs/docs/app/classic/view-switcher.md` -> `droid/Sources/Droid/Packages/android/widget/ViewSwitcher.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ViewAnimator.swift`
- `docs/docs/app/classic/text-switcher.md` -> `droid/Sources/Droid/Packages/android/widget/TextSwitcher.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ViewSwitcher.swift`, `droid/Sources/Droid/Packages/android/widget/ViewAnimator.swift`
- `docs/docs/app/classic/image-switcher.md` -> `droid/Sources/Droid/Packages/android/widget/ImageSwitcher.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ViewSwitcher.swift`, `droid/Sources/Droid/Packages/android/widget/ViewAnimator.swift`
- `docs/docs/app/classic/list-popup-window.md` -> `droid/Sources/Droid/Packages/android/widget/ListPopupWindow.swift`
- `docs/docs/app/classic/list-view.md` -> `droid/Sources/Droid/Packages/android/widget/ListView.swift`
- `docs/docs/app/classic/grid-view.md` -> `droid/Sources/Droid/Packages/android/widget/GridView.swift`
- `docs/docs/app/classic/expandable-list-view.md` -> `droid/Sources/Droid/Packages/android/widget/ExpandableListView.swift`
- `docs/docs/app/classic/two-line-list-item.md` -> `droid/Sources/Droid/Packages/android/widget/TwoLineListItem.swift`, inherited `droid/Sources/Droid/Packages/android/widget/RelativeLayout.swift`
- `docs/docs/app/classic/checked-text-view.md` -> `droid/Sources/Droid/Packages/android/widget/CheckedTextView.swift`, inherited `droid/Sources/Droid/Packages/android/widget/TextView.swift`
- `docs/docs/app/classic/action-menu-view.md` -> `droid/Sources/Droid/Packages/android/widget/ActionMenuView.swift`, inherited `droid/Sources/Droid/Packages/android/widget/LinearLayout.swift`
- `docs/docs/app/classic/image-view.md` -> `droid/Sources/Droid/Packages/android/widget/ImageView.swift`
- `docs/docs/app/classic/image-button.md` -> `droid/Sources/Droid/Packages/android/widget/ImageButton.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ImageView.swift`
- `docs/docs/app/classic/video-view.md` -> `droid/Sources/Droid/Packages/android/widget/VideoView.swift`
- `docs/docs/app/classic/media-controller.md` -> `droid/Sources/Droid/Packages/android/widget/MediaController.swift`, inherited `droid/Sources/Droid/Packages/android/widget/FrameLayout.swift`
- `docs/docs/app/classic/quick-contact-badge.md` -> `droid/Sources/Droid/Packages/android/widget/QuickContactBadge.swift`, inherited `droid/Sources/Droid/Packages/android/widget/ImageView.swift`
- `docs/docs/app/classic/zoom-button.md` -> `droid/Sources/Droid/Packages/android/widget/ZoomButton.swift`
- `docs/docs/app/classic/zoom-controls.md` -> `droid/Sources/Droid/Packages/android/widget/ZoomControls.swift`
- `docs/docs/app/classic/toolbar.md` -> `droid/Sources/Droid/Packages/android/widget/Toolbar.swift`
- `docs/docs/app/classic/gallery.md` -> `droid/Sources/Droid/Packages/android/widget/Gallery.swift`
- `docs/docs/app/classic/menu.md` -> `droid/Sources/Droid/Packages/android/view/Menu.swift`, `droid/Sources/Droid/Activities/Activity.swift`
- `docs/docs/app/classic/stack-view.md` -> `droid/Sources/Droid/Packages/android/widget/StackView.swift`
- `docs/docs/app/classic/table-layout.md` -> `droid/Sources/Droid/Packages/android/widget/TableLayout.swift`
- `docs/docs/app/classic/table-row.md` -> `droid/Sources/Droid/Packages/android/widget/TableRow.swift`, `droid/Sources/Droid/Packages/android/view/View+LayoutParams/View+TableLayout.swift`
- `docs/docs/app/classic/surface-view.md` -> `droid/Sources/Droid/Packages/android/view/SurfaceView.swift`
- `docs/docs/app/classic/texture-view.md` -> `droid/Sources/Droid/Packages/android/view/TextureView.swift`
- `docs/docs/app/classic/toast.md` -> `droid/Sources/Droid/Packages/android/widget/Toast.swift`
- `docs/docs/app/classic/calendar-view.md` -> `droid/Sources/Droid/Packages/android/widget/CalendarView.swift`
- `docs/docs/app/classic/horizontal-scroll-view.md` -> `droid/Sources/Droid/Packages/android/widget/HorizontalScrollView.swift`
- `docs/docs/app/classic/scroll-view.md` -> `droid/Sources/Droid/Packages/android/widget/ScrollView.swift`
- `docs/docs/app/classic/radio-group.md` -> `droid/Sources/Droid/Packages/android/widget/RadioGroup.swift`
- `docs/docs/app/classic/sliding-drawer.md` -> `droid/Sources/Droid/Packages/android/widget/SlidingDrawer.swift`
- `docs/docs/app/classic/grid-layout.md` -> `droid/Sources/Droid/Packages/android/widget/GridLayout.swift`, `droid/Sources/Droid/Packages/android/view/View+LayoutParams/View+GridLayout.swift`
- `docs/docs/app/classic/magnifier.md` -> `droid/Sources/Droid/Packages/android/widget/Magnifier.swift`
- `docs/docs/app/classic/popup-menu.md` -> `droid/Sources/Droid/Packages/android/widget/PopupMenu.swift`
- `docs/docs/app/classic/popup-window.md` -> `droid/Sources/Droid/Packages/android/widget/PopupWindow.swift`
- `docs/docs/app/classic/view-stub.md` -> `droid/Sources/Droid/Packages/android/view/ViewStub.swift`

- `docs/docs/app/x/drawer-layout.md` -> `droid/Sources/Droid/Packages/androidx/drawerlayout/widget/DrawerLayout.swift`
- `docs/docs/app/x/sliding-pane-layout.md` -> `droid/Sources/Droid/Packages/androidx/constraintlayout/slidingpanelayout/widget/SlidingPaneLayout.swift`
- `docs/docs/app/x/fragment-activity.md` -> `droid/Sources/Droid/Packages/androidx/fragment/app/FragmentActivity.swift`, `droid/Sources/Droid/Packages/androidx/fragment/app/FragmentManager.swift`
- `docs/docs/app/x/dialog-fragment.md` -> `droid/Sources/Droid/Packages/androidx/fragment/app/DialogFragment.swift`, `droid/Sources/Droid/Packages/androidx/fragment/app/Fragment.swift`
- `docs/docs/app/x/fragment-container-view.md` -> `droid/Sources/Droid/Packages/androidx/fragment/app/FragmentContainerView.swift`, `droid/Sources/Droid/Packages/androidx/fragment/app/FragmentManager.swift`, `droid/Sources/Droid/Packages/androidx/fragment/app/FragmentTransaction.swift`

- `docs/docs/app/x/navigation/app-bar-configuration.md` -> `droid/Sources/Droid/Packages/androidx/navigation/ui/AppBarConfiguration.swift`
- `docs/docs/app/x/navigation/navigation-ui.md` -> `droid/Sources/Droid/Packages/androidx/navigation/ui/NavigationUI.swift`, `droid/Sources/Droid/Activities/AppCompatActivity.swift`, `droid/Sources/Droid/Packages/comgoogleandroid/material/navigation/NavigationView.swift`, `droid/Sources/Droid/Packages/comgoogleandroid/material/navigation/NavigationBarView.swift`

- `docs/docs/app/x/fragments.md` -> `droid/Sources/Droid/Packages/androidx/fragment/app/Fragment.swift`
- `docs/docs/app/x/component-activity.md` -> `droid/Sources/Droid/Packages/androidx/activity/ComponentActivity.swift`
- `docs/docs/app/x/navigation/nav-controller.md` -> `droid/Sources/Droid/Packages/androidx/navigation/NavController.swift`
- `docs/docs/app/x/compat/button.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatButton.swift`
- `docs/docs/app/x/compat/toolbar.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetToolbar.swift`
- `docs/docs/app/x/coordinator-layout.md` -> `droid/Sources/Droid/Packages/androidx/coordinatorlayout/widget/CoordinatorLayout.swift`
- `docs/docs/app/x/constraint-layout.md` -> `droid/Sources/Droid/Packages/androidx/constraintlayout/widget/ConstraintLayout.swift`
- `docs/docs/app/x/card-view.md` -> `droid/Sources/Droid/Packages/androidx/cardview/CardViewPackage.swift`, `droid/Sources/Droid/Packages/androidx/cardview/widget/CardViewWidgetPackage.swift`, `droid/Sources/Droid/Packages/androidx/cardview/widget/CardView.swift`
- `docs/docs/app/x/view-pager.md` -> `droid/Sources/Droid/Packages/androidx/viewpager/ViewPagerPackage.swift`, `droid/Sources/Droid/Packages/androidx/viewpager/widget/ViewPagerWidgetPackage.swift`, `droid/Sources/Droid/Packages/androidx/viewpager/widget/ViewPager.swift`, `droid/Sources/Droid/Packages/androidx/viewpager2/ViewPager2Package.swift`, `droid/Sources/Droid/Packages/androidx/viewpager2/widget/ViewPager2WidgetPackage.swift`, `droid/Sources/Droid/Packages/androidx/viewpager2/widget/ViewPager2.swift`
- `docs/docs/app/x/compat/autocomplete-text-field.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatAutoCompleteTextView.swift`
- `docs/docs/app/x/compat/checkbox.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatCheckBox.swift`
- `docs/docs/app/x/compat/checked-text-view.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatCheckedTextView.swift`
- `docs/docs/app/x/compat/edit-text.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatEditText.swift`
- `docs/docs/app/x/compat/image-button.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatImageButton.swift`
- `docs/docs/app/x/compat/image-view.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatImageView.swift`
- `docs/docs/app/x/compat/multi-autocomplete-text-view.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatMultiAutoCompleteTextView.swift`
- `docs/docs/app/x/compat/radio-button.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatRadioButton.swift`
- `docs/docs/app/x/compat/rating-bar.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatRatingBar.swift`
- `docs/docs/app/x/compat/search-view.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetSearchView.swift`
- `docs/docs/app/x/compat/seek-bar.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatSeekBar.swift`
- `docs/docs/app/x/compat/spinner.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatSpinner.swift`
- `docs/docs/app/x/compat/switch.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetSwitchCompat.swift`
- `docs/docs/app/x/compat/text-view.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatTextView.swift`
- `docs/docs/app/x/compat/toggle-button.md` -> `droid/Sources/Droid/Packages/androidx/appcompat/widget/AppCompatWidgetAppCompatToggleButton.swift`

Newly authored pages enter this map once verified; `DONE.md` may retain a compact outcome for the completed coverage wave.

## Material pages (pin ab7a20d)

Pin-backed:
- `docs/docs/app/material/badges.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/badge/BadgeDrawable.swift`, `droid/Sources/Droid/Packages/comgoogleandroid/material/badge/BadgeUtils.swift`
- `docs/docs/app/material/nav-bar.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/navigation/NavigationBarView.swift`, `droid/Sources/Droid/Packages/comgoogleandroid/material/bottomnavigation/BottomNavigationView.swift`
- `docs/docs/app/material/nav-drawer.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/navigation/NavigationView.swift`
- `docs/docs/app/material/buttons/fabs.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/floatingactionbutton/FloatingActionButton.swift`
- `docs/docs/app/material/snackbar.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/Snackbar.swift`
- `docs/docs/app/material/app-bars.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/appbar/AppBarLayout.swift`
- `docs/docs/app/material/buttons/button.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/button/MaterialButton.swift`
- `docs/docs/app/material/toolbars.md` -> `droid/Sources/Droid/Packages/comgoogleandroid/material/appbar/MaterialToolbar.swift`

Pin-absent thin pages (namespace-only / no live wrapper type at ab7a20d):
- `docs/docs/app/material/chips.md`, `cards.md`, `tabs.md`, `text-fields.md`, `sliders.md`, `progress-indicators.md`, `checkbox.md`, `switch.md`, `radio-button.md`, `nav-rail.md`
- `docs/docs/app/material/bottom-sheets.md`, `side-sheets.md`, `dialogs.md`, `menus.md`, `search.md`, `lists.md`, `divider.md`, `tooltips.md`, `carousel.md`, `date-pickers.md`, `time-pickers.md`, `loading-indicator.md`
- `docs/docs/app/material/buttons/extended-fabs.md`, `fab-menu.md`, `groups.md`, `icon-button.md`, `segmented-button.md`, `split-button.md`
