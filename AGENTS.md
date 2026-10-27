# AGENTS.md — Learning Ways

Instructions for any coding agent (Claude, Copilot, Aider, …) working in this repo.
Read `DESIGN.md` first for the why. This file is the how.

## Context

- Built at **Claude Impact Lab**, Mesh Oslo, 21 Oct 2026 (Day of AI, Oslo Innovation Week).
  Day: 10:20 build round 1 · 11:45 lunch · 12:15 build round 2 · **13:00 feature freeze** · 13:15–14:00 demos (5 min per team).
- Team: a philosopher, an economist/actor, builders, parents. Some never write code. Explain changes in plain language.
- Area: *My kids* → learning and thinking. Person: **Ida, 13, 8th grade (fictional)**.
- Event brief: https://www.gritai.studio/meetups/claude-impact-lab/brief

## Files

| File | What |
|---|---|
| `index.html` | The whole app. One self-contained file: HTML + CSS + JS. No build, no server, no dependencies except Google Fonts. |
| `DESIGN.md` | Problem, principles, what's built, open questions. Keep it current. |
| `AGENTS.md` | This file. |

The original decision board is historical. Record new Learning Ways decisions in this repository.

## Hard rules

1. **One file.** Everything stays in `index.html`. It must open by double-click in any browser with nothing installed.
2. **Made-up data only.** Ida, class 8B, classmates, week history are fictional. Never add real names or personal data.
3. **Makker never gives the answer.** Hints go question → tip → example. Any AI prompt must forbid revealing the final answer or doing the last step.
4. **UI copy is Norwegian bokmål (default) and English**, toggled with NO/EN in the header. All copy lives in `STR` (UI) and the `nb`/`en` blocks of `TASKS`, `PAST`, `BADGES`; add every new string in both languages. Written for a 13-year-old: friendly, not babyish. Code comments may be Norwegian or English.
5. **Two brains, both must work.**
   - *Published artifact*: real Claude via `claude.use("sample")` (`modelTier: "quick"`, `cache: false`). Shows "Ekte KI".
   - *Local file*: scripted replies using keyword lists per task. Shows "Øvingsmodus".
   Every AI feature needs a scripted fallback.
6. **Voice** uses the browser Web Speech API only (no keys, no installs):
   - Speech-to-text: OFF by default (privacy); opt-in toggle under "Lyd" / "Sound". Press-and-hold the mic, release to send. `SpeechRecognition`, language selectable (`nb-NO` / `no-NO` / `nn-NO`, or `en-GB` / `en-US` in English mode). Works in Google Chrome/Edge on a normal web page or local file; blocked inside the artifact frame — degrade to typing with a clear message.
   - Text-to-speech: `speechSynthesis` with an on-device (`localService`) voice matching the current language only. If none is installed, stay silent and say so once (never read Norwegian with an English voice, or the reverse).
7. **Points reward thinking, not speed.** See the TP table in `DESIGN.md`. Wrong answers never cost points.
8. **Ask before** installing anything, adding API keys, or using accounts.
9. **Theme + phone.** Colors are CSS tokens with light/dark variants; layout must work at ~390px with no horizontal scroll. Respect `prefers-reduced-motion`.

## Working loop

- One change at a time; say what changed in plain words.
- After each working version, suggest 2–3 next steps (small and big), each naming what it would show or test.
- From 13:00: no new features — fix, polish, help with the demo script.

## Test before saying "done"

Headless Chromium is available (Playwright; do not run `playwright install`):

```python
from playwright.sync_api import sync_playwright
with sync_playwright() as p:
    b = p.chromium.launch(); pg = b.new_page()
    errs = []; pg.on("pageerror", lambda e: errs.append(str(e)))
    pg.goto("file:///path/to/index.html")
    # stuck → answer hint → correct answer → teach Makker → next … → parent tab
    assert not errs
```

Check: full flow for all 3 tasks, parent tab, dark mode, 390px width, `scrollWidth` ≤ viewport.

## Repository and releases

- Work in the Learning Ways checkout connected to `github.com/justaride/learning-ways`, independently of the original project.
- Do not publish to the original artifact, copy to the original team laptop, or push to the original repository.
- Superpowers is optional and disabled by default; use it only when explicitly requested for the current task.
- Do not push or deploy without authorization.
