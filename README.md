# veb

Headless browser automation CLI for agents and humans.

## Installation

```bash
pnpm install
npx playwright install chromium
pnpm build
```

## Quick Start

```bash
veb open example.com
veb click "#submit"
veb fill "#email" "test@example.com"
veb get text "h1"
veb screenshot page.png
veb close
```

## Commands

### Core Commands

```bash
veb open <url>              # Navigate to URL
veb click <sel>             # Click element
veb type <sel> <text>       # Type into element
veb fill <sel> <text>       # Clear and fill
veb press <key>             # Press key (Enter, Tab, Control+a)
veb keydown <key>           # Hold key down
veb keyup <key>             # Release key
veb insert <text>           # Insert text (no key events)
veb hover <sel>             # Hover element
veb select <sel> <val>      # Select dropdown option
veb multiselect <sel> <v1> <v2>  # Multi-select
veb check <sel>             # Check checkbox
veb uncheck <sel>           # Uncheck checkbox
veb scroll <dir> [px]       # Scroll (up/down/left/right)
veb scrollinto <sel>        # Scroll element into view
veb drag <src> <tgt>        # Drag and drop
veb upload <sel> <files>    # Upload files
veb download [path]         # Wait for download
veb screenshot [path]       # Take screenshot (--full for full page)
veb pdf <path>              # Save as PDF
veb snapshot                # Accessibility tree (best for AI)
veb eval <js>               # Run JavaScript
veb close                   # Close browser
```

### Get Info

```bash
veb get text <sel>          # Get text content
veb get html <sel>          # Get innerHTML
veb get value <sel>         # Get input value
veb get attr <sel> <attr>   # Get attribute
veb get title               # Get page title
veb get url                 # Get current URL
veb get count <sel>         # Count matching elements
veb get box <sel>           # Get bounding box
```

### Check State

```bash
veb is visible <sel>        # Check if visible
veb is enabled <sel>        # Check if enabled
veb is checked <sel>        # Check if checked
```

### Find Elements (Semantic Locators)

```bash
veb find role <role> <action> [value]       # By ARIA role
veb find text <text> <action>               # By text content
veb find label <label> <action> [value]     # By label
veb find placeholder <ph> <action> [value]  # By placeholder
veb find alt <text> <action>                # By alt text
veb find title <text> <action>              # By title attr
veb find testid <id> <action> [value]       # By data-testid
veb find first <sel> <action> [value]       # First match
veb find last <sel> <action> [value]        # Last match
veb find nth <n> <sel> <action> [value]     # Nth match
```

**Actions:** `click`, `fill`, `check`, `hover`, `text`

**Examples:**
```bash
veb find role button click --name "Submit"
veb find text "Sign In" click
veb find label "Email" fill "test@test.com"
veb find first ".item" click
veb find nth 2 "a" text
```

### Wait

```bash
veb wait <selector>         # Wait for element
veb wait <ms>               # Wait for time
veb wait --text "Welcome"   # Wait for text
veb wait --url "**/dash"    # Wait for URL pattern
veb wait --load networkidle # Wait for load state
veb wait --fn "window.ready === true"  # Wait for JS condition
```

**Load states:** `load`, `domcontentloaded`, `networkidle`

### Mouse Control

```bash
veb mouse move <x> <y>      # Move mouse
veb mouse down [button]     # Press button (left/right/middle)
veb mouse up [button]       # Release button
veb mouse wheel <dy> [dx]   # Scroll wheel
```

### Browser Settings

```bash
veb set viewport <w> <h>    # Set viewport size
veb set device <name>       # Emulate device ("iPhone 14")
veb set geo <lat> <lng>     # Set geolocation
veb set offline [on|off]    # Toggle offline mode
veb set headers <json>      # Extra HTTP headers
veb set credentials <u> <p> # HTTP basic auth
veb set media [dark|light|print]  # Emulate media
```

### Cookies & Storage

```bash
veb cookies                 # Get all cookies
veb cookies set <json>      # Set cookies
veb cookies clear           # Clear cookies

veb storage local           # Get all localStorage
veb storage local <key>     # Get specific key
veb storage local set <k> <v>  # Set value
veb storage local clear     # Clear all

veb storage session         # Same for sessionStorage
```

### Network

```bash
veb network route <url>              # Intercept requests
veb network route <url> --abort      # Block requests
veb network route <url> --body <json>  # Mock response
veb network unroute [url]            # Remove routes
veb network requests                 # View tracked requests
veb network requests --filter api    # Filter requests
veb response <url>                   # Get response body (waits for matching request)
```

### Tabs & Windows

```bash
veb tab                     # List tabs
veb tab new                 # New tab
veb tab <n>                 # Switch to tab n
veb tab close [n]           # Close tab
veb window new              # New window
```

### Frames

```bash
veb frame <sel>             # Switch to iframe
veb frame main              # Back to main frame
```

### Dialogs

```bash
veb dialog accept [text]    # Accept (with optional prompt text)
veb dialog dismiss          # Dismiss
```

### Debug

```bash
veb trace start             # Start recording trace
veb trace stop <path>       # Stop and save trace
veb console                 # View console messages
veb console --clear         # Clear console
veb errors                  # View page errors
veb highlight <sel>         # Highlight element
veb state save <path>       # Save auth state
veb state load <path>       # Load auth state
veb initscript <js>         # Run JS on every page load
```

### Navigation

```bash
veb back                    # Go back
veb forward                 # Go forward
veb reload                  # Reload page
```

### Sessions

```bash
veb session                 # Show current session
veb session list            # List active sessions
```

## Options

| Option | Description |
|--------|-------------|
| `--session <name>` | Use isolated session (or `VEB_SESSION` env) |
| `--json` | JSON output (for agents) |
| `--full, -f` | Full page screenshot |
| `--name, -n` | Locator name filter |
| `--exact` | Exact text match |
| `--debug` | Debug output |

## Sessions

Run multiple isolated browser instances:

```bash
# Different sessions
veb --session agent1 open site-a.com
veb --session agent2 open site-b.com

# Or via environment
VEB_SESSION=agent1 veb click "#btn"

# List all
veb session list
```

## Selectors

```bash
# CSS
veb click "#id"
veb click ".class"
veb click "div > button"

# Text
veb click "text=Submit"

# XPath
veb click "xpath=//button"

# Semantic (recommended)
veb find role button click --name "Submit"
veb find label "Email" fill "test@test.com"
```

## Agent Mode

Use `--json` for machine-readable output:

```bash
veb snapshot --json
veb get text "h1" --json
veb is visible ".modal" --json
```

## License

MIT
