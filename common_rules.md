## Common rules
<!-- synced block, do not edit; it will be overwritten -->

### Code style

- Comments are written in English.
- Indentation is 2 spaces in all file formats. Follow `.editorconfig`.
- Kotlin: format with `ktfmt` in the official Kotlin style. Do not use `!!` on nullable values.
- Swift: do not use `!` to force-unwrap optionals.
- Rust: format with `rustfmt` using `lib/.rustfmt.toml`.
- Hot paths must be performance-oriented. Everywhere else prefer code that is easy for a human to read.
- Do not add comments that only restate the code. Comment intent, constraints, and trade-offs.

### Experiments

Experimental code for testing hypotheses:
- Put large experiments into new files, not into existing production files.
- Gate each experiment with a toggle constant. All toggles live in one place per language:
  - Kotlin: `common/src/commonMain/kotlin/{{package_name}}/Experiments.kt` (replace {{package_name}} with actual package name, so `com.name.subname` becomes `com/name/subname`)
  - Swift: `ios/iosApp/Experiments.swift`
  - Rust: `lib/src/experiments.rs`
- Do not leave disabled experiments in production code paths without a toggle.

### Debugging and performance work

- Isolate the suspected code and add logs to collect data on behavior, performance, and correctness.
- Run builds and tests yourself (`cargo`, `./gradlew`) to see the effect of changes. Ask the user for logs only when you cannot get them yourself.
- For performance problems: measure the original code first, record the results, then apply changes and measure again. Compare against the baseline.
- Use only project directories, `/tmp`, and the Gradle and Cargo source directories for debugging.
- Save screenshots to `.tmp/screenshots/` in the root of the project directory, not in any other directories. Remove the screenshot when you viewed it and it becomes obsolete.
- Use `adb` if it's available for you for testing the app and making screenshots for debugging purposes. Ensure you make screenshots of the application you're developing, not anything else.
- Use `xvfb` when it's available for testing the app on Linux machine in order to prevent showing the app's window on user's screen and interrupt the workflow.

### Working agreements

- Ask the user when a task has more than one reasonable interpretation. Do not guess.
- Focus on completing the task rather than explaining why it cannot be done.
- Use search and subagents where they save context.
- Do not commit or create branches unless asked.
- Do not fix unrelated problems. Mention them instead.
- Try to be as concise as possible when writting code. Prefer small changes over refactoring. Smaller diffs are better than longer diffs.
- If the code requires refactoring, tell your objectives to the user and wait for a feedback before applying any changes.