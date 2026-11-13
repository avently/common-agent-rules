# Common and per language rules for agents

You can use the rules to simplify development of projects. Copy them manually or use the following script (works on Linux/Mac, run from your project dir, can be run repeatedly):

```bash
BASE="https://raw.githubusercontent.com/avently/common-agent-rules/refs/heads/main/"
# separate supported languages you need rules for by space
LANGS="Kotlin Rust"
touch AGENTS.md

# Downloading the files
curl -fsSL "$BASE/common_rules.md" -o /tmp/common_rules.md
curl -fsSL "$BASE/language_rules.md"  -o /tmp/language_rules.md

# Taking only needed languages from the whole list
awk -v langs="$LANGS" '
BEGIN { n = split(langs, a, " "); for (i = 1; i <= n; i++) want[a[i]] = 1 }
$0 == "## Common rules per language" { next }
$0 ~ /^### / {
    lang = substr($0, 5)
    active = (lang in want)
    if (active) {
        if (!printed) { print "## Common rules per language\n<!-- synced block, do not edit; it will be overwritten -->\n"; printed = 1 }
        print
    }
    next
}
active { print }
' /tmp/language_rules.md > /tmp/lang_block.md

# If there is no match found, exiting to prevent any unneeded changes
if [ -s /tmp/lang_block.md ]; then

  # Making backup for a diff
  cp AGENTS.md /tmp/AGENTS.md.old

  # Replacing common rules
  awk '
BEGIN { while ((getline l < "/tmp/common_rules.md") > 0) b = b l "\n" }
$0 ~ /^## Common rules *$/ { printf "%s\n", b; skip = 1; found = 1; next }
skip && /^## / { skip = 0 }
!skip { print }
END { if (!found) { printf "%s\n", b } }
' AGENTS.md > /tmp/AGENTS.md.new && mv /tmp/AGENTS.md.new AGENTS.md

  # Adding languages block
  awk '
BEGIN { while ((getline l < "/tmp/lang_block.md") > 0) b = b l "\n" }
$0 ~ /^## Common rules per language *$/ { printf "%s\n", b; skip = 1; found = 1; next }
skip && /^## / { skip = 0 }
!skip { print }
END { if (!found) { printf "%s\n", b } }
' AGENTS.md > /tmp/AGENTS.md.new && mv /tmp/AGENTS.md.new AGENTS.md

  # Check the diff
  git diff /tmp/AGENTS.md.old AGENTS.md
else
  echo "No such languages found: $LANGS" >&2
fi
```


Or run locally:

```bash
# Specify a directory where you store common-agent-rules
BASE="../common-agent-rules/"
# separate supported languages you need rules for by space
LANGS="Kotlin Rust"
touch AGENTS.md

# Downloading the files
cp "$BASE/common_rules.md" /tmp/common_rules.md
cp "$BASE/language_rules.md" /tmp/language_rules.md

# Taking only needed languages from the whole list
awk -v langs="$LANGS" '
BEGIN { n = split(langs, a, " "); for (i = 1; i <= n; i++) want[a[i]] = 1 }
$0 == "## Common rules per language" { next }
$0 ~ /^### / {
    lang = substr($0, 5)
    active = (lang in want)
    if (active) {
        if (!printed) { print "## Common rules per language\n<!-- synced block, do not edit; it will be overwritten -->\n"; printed = 1 }
        print
    }
    next
}
active { print }
' /tmp/language_rules.md > /tmp/lang_block.md

# If there is no match found, exiting to prevent any unneeded changes
if [ -s /tmp/lang_block.md ]; then

  # Making backup for a diff
  cp AGENTS.md /tmp/AGENTS.md.old

  # Replacing common rules
  awk '
BEGIN { while ((getline l < "/tmp/common_rules.md") > 0) b = b l "\n" }
$0 ~ /^## Common rules *$/ { printf "%s\n", b; skip = 1; found = 1; next }
skip && /^## / { skip = 0 }
!skip { print }
END { if (!found) { printf "%s\n", b } }
' AGENTS.md > /tmp/AGENTS.md.new && mv /tmp/AGENTS.md.new AGENTS.md

  # Adding languages block
  awk '
BEGIN { while ((getline l < "/tmp/lang_block.md") > 0) b = b l "\n" }
$0 ~ /^## Common rules per language *$/ { printf "%s\n", b; skip = 1; found = 1; next }
skip && /^## / { skip = 0 }
!skip { print }
END { if (!found) { printf "%s\n", b } }
' AGENTS.md > /tmp/AGENTS.md.new && mv /tmp/AGENTS.md.new AGENTS.md

  # Check the diff
  git diff /tmp/AGENTS.md.old AGENTS.md
else
  echo "No such languages found: $LANGS" >&2
fi
```