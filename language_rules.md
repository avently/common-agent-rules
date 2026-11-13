## Common rules per language

### Kotlin

- Files with `expect`/`actual` declarations: the `commonMain` file gets a descriptive name based on its content, for example `Expectations.kt`.
- Platform implementations get the platform name in lowercase as a suffix: `Expectations.android.kt`, `Expectations.jvm.kt`, `Expectations.ios.kt`.
- Functions are named after what they do. Split functions longer than 200 lines into named logical blocks.

### Rust

- Avoid using nightly features. If this is impossible for the task, ask the user for a permission.
