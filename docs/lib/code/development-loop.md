# Development Loop

In order to test your coode in the real Android project please consider [→ importing](../distribution/library-import.md) this Library project as a module into your Android app project.

**The Loop**

1. Make changes to your Swift code
2. Hit `Swift Project` -> `Build`
3. Hit `Java Library Project -> Assemble`
4. Run your Android app project
5. Repeat

**With Standalone AAR**

1. Make changes to your Swift code
2. Hit `Swift Project` -> `Build`
3. Hit `Java Library Project -> Assemble`
4. Copy the new `.aar` file from the `outputs/aar` folder into your Android project's `app/libs` folder, replacing the old one.
5. Run your Android app project
6. Repeat