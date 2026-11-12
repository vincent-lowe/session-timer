# Prompt: Recreate the Salesforce Session Timer

Paste everything below (from "You are building…" to the end) to an AI coding
agent working in an **empty folder**. It reproduces the same outcome we built:
a single-file, branded countdown timer published to GitHub Pages at a
`/current/` sub-path.

> **Before you start:** two things require a human and cannot be automated by the
> agent — (1) signing `gh`/git into the correct **public github.com** account,
> and (2) supplying reference images of the mascots if you want them matched
> closely. The prompt calls these out at the right moments.

---

You are building a self-contained countdown timer as a single local HTML file,
then publishing it to GitHub Pages. Work in the current folder. Verify your work
as you go (on macOS you can render the page to an image with
`qlmanage -t -s 1400 -o <outdir> <file>` and inspect it; otherwise reason
carefully about the markup). Keep everything in ONE file with all CSS and JS
inline — no build step, no dependencies, no external files except the one web
font below. It must work when opened directly from disk (`file://`) and offline.

## 1. The timer — `timer.html`

Purpose: show remaining time to students while screen-sharing during online
sessions. Big, legible, calm.

Behaviour:
- A large circular **progress ring** that depletes as time counts down, with the
  remaining time shown as large tabular-numeral digits in the center
  (`MM:SS`, or `H:MM:SS` once over an hour).
- Countdown uses **wall-clock timestamps** (`endTime = Date.now() + remainingMs`,
  compared against `Date.now()` on each tick) so it stays accurate even if the
  tab is backgrounded. Tick every ~200 ms.
- **Controls:** a minutes number input (0–999); quick-set preset buttons for
  **5 / 10 / 15 / 25 / 45 / 60**; **Start/Pause** (label toggles
  Start → Pause → Resume), **Reset**, **+1 min** (extends even while running),
  and **Fullscreen**.
- **Editable session label** above the ring (`contenteditable`), persisted in
  `localStorage`, single-line (Enter blurs, no line breaks). Shows placeholder
  "Click to add a label" when empty.
- **Color state cues** on the body/ring as time runs low: normal → a warm
  "warn" state at ≤ 60 s → a red "danger" state at ≤ 30 s → a flashing
  "finished" state at 0.
- At zero, play a gentle 3-note chime generated with the **Web Audio API**
  (no audio files); wrap it in try/catch and fail silently. (Browsers need one
  user interaction before audio plays — that's expected.)
- Update `document.title` to the remaining time so it shows in the tab.
- **Keyboard shortcuts:** `Space` start/pause, `R` reset, `F` fullscreen,
  `↑`/`↓` adjust minutes. Ignore these while typing in the label or minutes
  field. Clicking the ring toggles the controls' visibility (clean presenting
  view). Show a one-line hint of the shortcuts at the bottom.
- Defensive JS: clamp minutes to 0–999; guard against negative time; use an IIFE
  in strict mode. No console errors.

## 2. Salesforce branding

- **Palette (CSS custom properties):** primary blue `#0176D3`, classic cloud
  blue `#00A1E0`, deep navy `#032D60`, light sky `#AACBFF` / `#D8EDFF`, near-white
  cloud `#EAF5FE`, trailhead green `#2E844A`, warm orange `#FE9339` (the "warn"
  accent), salesforce red `#EA001E` (the "danger"/finished accent). Background is
  a navy→blue gradient; the ring stroke is a blue gradient (`#00A1E0`→`#4FB3F0`).
- **Typography:** load **Poppins** from Google Fonts (`wght 400;500;600;700;800`,
  `display=swap`) as a stand-in for Salesforce Sans, with a system-font fallback
  stack so it still works offline. This is the only external resource.
- **Background scene (all original inline SVG, no external images):** a few soft
  **clouds** that slowly drift via CSS keyframes, plus the two mascots below.
- A small **brand header** ("Salesforce Session Timer") with a cloud mark at the
  top.

## 3. The mascots — Codey & Astro (inline SVG lookalikes)

Draw **original stylized SVG** figures (do NOT embed or download the official
copyrighted Salesforce artwork). Each sits on a **soft periwinkle (`#9FA8F0`)
rounded blob** backdrop that stays fully inside its own `viewBox` (no hard
rectangular clipping). Give each a gentle bobbing CSS animation.

> **Ask the human for reference images** of Codey and Astro and match these
> details closely. The key, easy-to-get-wrong facts:
>
> - **Codey** is a **brown bear** (not blue) wearing a **black "Trailblazer"
>   hoodie** (white italic script + a small blue cloud badge), with **one paw
>   raised in a wave** (show the paw pads), and brown legs/belly.
> - **Astro** has a **human face** (skin tone, black hair tuft) inside a **brown
>   raccoon "onesie" hood with ears**, wears the same **black Trailblazer
>   hoodie**, gives a **grey-gloved thumbs-up**, and has a **striped bushy
>   raccoon tail** curling out to one side.

**Placement:** position the mascots **high on the left and right sides**,
flanking the ring and clearly **above the controls** (anchor them near the top,
e.g. `top: 12vh`, not at the bottom). Scale them down and then hide them on
small screens so the controls never collide.

## 4. Verify

Confirm the HTML/SVG tags are balanced, there are no stray/invalid CSS rules,
and (if you can render it) that both mascots read correctly and sit above the
controls. Fix anything that looks off.

## 5. Publish to GitHub Pages at a `/current/` sub-path

The final public URL must be a **subordinate path**, leaving the repo root
untouched (do not create a root `index.html`):

```
https://<username>.github.io/<repo>/current/
```

Steps:
1. Add a `.gitignore` that excludes any reference images the human supplied
   (e.g. `cody.png`, `astro.jpeg`) and `.DS_Store` — they're unused (mascots are
   inline SVG) and copyrighted, so they must NOT be committed. Add a short
   `README.md`.
2. **Restructure for the `/current/` URL:** the timer file must live at
   `current/index.html` (a folder URL serves its `index.html`). Keep a tiny
   `timer.html` at the repo root that is **only a redirect** to `./current/`
   (`<meta http-equiv="refresh">` + `location.replace` + a canonical link), so
   any previously shared `timer.html` links keep working.
3. `git init -b main`, commit (author it as the human's work identity; end the
   commit message with any required co-author/attribution line).
4. **[Human step]** Ensure `gh` is authenticated to **public github.com** (not an
   internal/Enterprise host) under the intended account:
   `gh auth login --hostname github.com --git-protocol https --web`. The agent
   must verify with `gh auth status --hostname github.com` and
   `gh api user --hostname github.com -q .login` before proceeding, and must not
   assume github.com just because a git identity is set — check the actual host.
5. Create and push: `GH_HOST=github.com gh repo create <repo> --public --source . --remote origin --push`
   (if the repo already exists, just add the remote and `git push -u origin main`
   instead — and diff local vs `origin/main` to confirm they match).
6. Enable Pages from `main` / root:
   `GH_HOST=github.com gh api -X POST repos/<owner>/<repo>/pages -f "source[branch]=main" -f "source[path]=/"`
   (treat a 409 "already enabled" as success).
7. **Verify live:** poll the `/current/` URL until it returns 200 (Pages can take
   up to ~1 minute to build), confirm it serves the real timer (not a 404 or a
   stale version), and confirm the old `/timer.html` URL returns 200 and
   redirects to `./current/`.

## Notes / gotchas to respect

- Everything self-contained and offline-capable; only the Poppins font is remote,
  and it must fall back gracefully.
- The repo and Pages site will be **public** — fine for a timer, but say so.
- Reference images are inputs only: keep them out of git via `.gitignore`.
- Don't guess the GitHub host — a machine may be logged into an internal
  Enterprise GitHub whose Pages are not reachable externally. Confirm it's
  public github.com before publishing.
- The end-of-timer chime requires one click on the page first (browser autoplay
  policy); this is expected, not a bug.

## Definition of done

- `current/index.html` is the branded, single-file timer with working countdown,
  presets, label, color states, chime, keyboard shortcuts, and inline-SVG
  Codey (waving brown bear) + Astro (thumbs-up raccoon-hood explorer) high on the
  sides above the controls.
- Root `timer.html` redirects to `current/`.
- Reference images are git-ignored.
- The site is live and verified at
  `https://<username>.github.io/<repo>/current/` (200, correct content), with the
  old `timer.html` path redirecting to it.
