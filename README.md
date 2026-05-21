# Pomodoro Timer

A clean, single-page Pomodoro timer built with vanilla HTML, CSS, and JavaScript. No frameworks, no build step — just open the file and it works.

## Features

- Configurable focus and break durations (1–60 min focus, 1–30 min break)
- Auto-transitions between focus and break sessions
- Audible alert when a session ends
- Session history log persisted via `localStorage` (resets daily)
- Live countdown in the browser tab title
- Responsive layout — works on mobile and desktop

## How to Run

### Option 1 — Open directly in browser (recommended)

```bash
# No installation needed
open promodoro1.html        # macOS
start promodoro1.html       # Windows
xdg-open promodoro1.html    # Linux
```

Or simply drag `promodoro1.html` into any browser window.

### Option 2 — Serve locally (avoids audio autoplay restrictions on some browsers)

```bash
# Python 3
python -m http.server 8080

# Then open:
# http://localhost:8080/promodoro1.html
```

### Option 3 — Live Server (VS Code)

Install the [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer), right-click `promodoro1.html`, and select **Open with Live Server**.

## Deployed URL

> _Not deployed. To deploy, drop `promodoro1.html` into [Netlify Drop](https://app.netlify.com/drop) and share the generated URL — no account required._

## Project Structure

```
promodoro1.html   ← entire app (HTML + CSS + JS in one file)
README.md
ANSWERS.md
```

## Stack

Vanilla HTML/CSS/JavaScript — no dependencies, no build tools, no package manager.
