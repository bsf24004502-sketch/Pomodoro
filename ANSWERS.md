# ANSWERS.md — Frontend Assessment: Pomodoro Timer

---

## 1. How to run

No installation required. Open `promodoro1.html` directly in any modern browser:

```bash
open promodoro1.html        # macOS
start promodoro1.html       # Windows
xdg-open promodoro1.html    # Linux
```

Alternatively, serve it locally to avoid browser audio restrictions:

```bash
python -m http.server 8080
# then visit http://localhost:8080/promodoro1.html
```

No deployed URL — the file runs entirely in-browser with no backend.

---

## 2. Stack & design choices

**Stack: Vanilla HTML/CSS/JavaScript — single file, zero dependencies.**

I chose this because the spec said "anything reasonable that runs in a browser," and a Pomodoro timer has no state complexity that needs a framework. A single `.html` file is the most portable deliverable possible — no `npm install`, no build step, no version mismatches. The assessor can open it in 2 seconds.

**Two specific design decisions:**

- **Timer takes 60% of the viewport height on mobile.** The `4.5rem` font size and center-aligned card are sized so the countdown is the first thing your eye goes to on a 360px-wide phone. Everything else — settings, history — lives below the fold. I picked this over a compact layout because the timer *feel* is the whole product.

- **Color flips between red (focus) and green (break) across the entire UI.** Rather than just changing an icon or label, I toggle a `brk` class on `<body>` that recolors the timer, the progress bar, the logo, and the main button in one shot. I picked a body-class approach over per-element logic because it made the "what mode am I in?" question answerable at a glance without reading any text — important for peripheral awareness while you're actually working.

---

## 3. Responsive & accessibility

**Responsive behavior:**

The layout uses a `max-width: 395px` centered column, so it degrades gracefully — on a 360px phone it fills the screen naturally; on a 1440px laptop it stays compact and centered rather than stretching into an unusable wide bar. The settings boxes use `flex: 1` so they share space evenly at any width.

**Accessibility consideration handled — keyboard navigation:**

All interactive elements are native `<button>` elements, which means they receive focus, respond to Enter/Space, and are announced correctly by screen readers without any extra ARIA work. The timer value is in a plain `<div>` that updates every second — a screen reader user relying on live regions would not get a good experience here, since I did not add `aria-live="polite"` to the timer display.

**One thing knowingly skipped:**

I did not add `aria-live` to the countdown. Announcing every second would be unusable noise; announcing only on session completion would require a separate visually-hidden live region. This is a real gap — a blind user would not know when a session ends without the audio cue, and the audio has no guaranteed autoplay on all browsers. Given more time, I would add a visually-hidden `aria-live="assertive"` element that announces only the session-end event ("Focus session complete. Break starting.").

---

## 4. AI usage

I used Claude (Anthropic) during development. Here are two specific cases:

**Case 1 — Progress bar direction**

I asked Claude to generate the initial progress bar logic. It gave me a bar that *grows* from 0% to 100% as time runs out. I changed it to *shrink* from 100% to 0% (`width: (timeLeft / total) * 100 + '%'`) because a shrinking bar reads more intuitively as "time running out" — you see what's left, not what's gone. The AI output was correct code, but the UX direction was wrong for a timer.

**Case 2 — localStorage date reset**

I asked Claude how to reset the session log daily without a backend. It suggested storing the current `toDateString()` alongside the log and comparing on load — exactly what the final code does. I kept this output verbatim because it was the right solution and added no complexity:

```javascript
var today = new Date().toDateString();
if (localStorage.getItem('pomo_date') !== today) {
  localStorage.setItem('pomo_date', today);
  localStorage.setItem('pomo_log', '[]');
}
```

---

## 5. Honest gap

The audio cue is unreliable. Most browsers block `audio.play()` unless the user has interacted with the page recently, and I only `catch` the error silently. In practice, if you set the timer and walk away, there's a real chance the bell never fires — which is the one moment it actually matters.

With another day, I would replace the `<audio>` element approach with the Web Audio API, synthesizing a short tone programmatically. The Web Audio API is less likely to be blocked after a prior user gesture, and it removes the dependency on an external MP3 URL (`mixkit.co`), which could go down or be blocked by a content policy.

A secondary fix would be adding a visual flash or a persistent banner ("Session complete!") as a fallback for when audio fails.
