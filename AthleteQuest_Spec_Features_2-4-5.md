# AthleteQuest AI — Build Spec: Reminders, Profiles & Backup, Benchmarks & Share Cards

**Status:** Draft for review · June 2026
**In scope:** Feature 2 (Practice reminders & streak notifications), Feature 4 (Multiple athlete profiles + cross-device backup), Feature 5 (Age-group benchmarks + shareable achievement cards).
**Deferred:** Feature 3 (wearable/health auto-import) and Feature 1 (on-device LLM coach) — not in this phase.

---

## Shared principles (apply to all three)

- **100% on-device — works in airplane mode.** No backend, no API calls, no network dependency of any kind. All UI/logic lands in `AthleteQuest.html`; the only sibling files are the existing PWA assets (`manifest.webmanifest`, `sw.js`, icons), all cached by the service worker for offline use. **This rules out Web Push** (which requires a server) and any cloud sync.
- **Privacy & kid-safety first.** No personal data leaves the device unless the user explicitly shares it. Anything that turns on notifications, sharing, or any network call sits behind the **Parents** tab (math-gated) by default.
- **Progressive enhancement.** Features degrade gracefully when a browser/OS doesn't support them — never a broken screen.
- **localStorage today.** Current state lives under the key `aq_state`; `save()`/`boot()` manage it. Profiles (Feature 4) generalize this.

---

## Feature 2 — Practice reminders & streak notifications

### Goal
Bring kids back daily so the streak/quest/XP loop actually works. Today nothing pulls a user back into the app.

### Constraint (airplane-mode requirement)
Reminders are most useful when they fire **while the app is closed** — but doing that with guaranteed timing normally needs Web Push, which requires a server. Since the app must run **100% offline with no server**, we do **not** use Web Push. Instead we layer three fully on-device mechanisms:

**1. Calendar reminder (.ics) — primary; works everywhere incl. iPhone; offline.**
A "Remind me to train" action generates a **recurring calendar event** ("Log AthleteQuest practice," daily at the chosen time, with an alarm) as an `.ics` file and hands it to the device via download or `navigator.share`. The phone's own Calendar/Reminders app then notifies at that time. No server, works in airplane mode, works on iOS — the most reliable closed-app reminder we can ship offline.

**2. In-app nudges — baseline; all platforms.**
When the app is opened, evaluate the streak/quests and show a "streak at risk" banner ("🔥 Log today to keep your 6-day streak!"), and fire an immediate local `Notification` if permission is already granted. Re-engages anyone who opens the app.

**3. Local scheduled notification — bonus where supported.**
On browsers exposing the **Notification Triggers API** (some Chromium/Android installed PWAs), schedule a local notification through the service worker that fires offline with no server. Pure progressive enhancement — never relied on, and absent on iOS.

### What we are explicitly NOT promising
Precise, guaranteed background push at an exact time on every device. On iOS the dependable offline path is the **calendar event** (mechanism 1). The UI sets that expectation honestly.

### UX flow
1. Parents tab → **Reminders** card → toggle on → pick a time (default 5:00 PM).
2. Primary button **"📅 Add reminder to my calendar"** (offline, all devices). Secondary **"🔔 Notify me in the app"** (notification permission requested after a win, not on load).
3. A kid-facing "Want a daily nudge? ⏰" prompt can appear after a level-up.

### Data model
`state.settings.reminders = { enabled:false, time:"17:00", lastPrompted:null }`

### .ics generation (100% client-side)
Build a `VEVENT` with `RRULE:FREQ=DAILY`, `DTSTART` at the chosen local time, and a `VALARM` trigger; output as a `text/calendar` Blob for download or `navigator.share`. No libraries, no network.

### Privacy & safety
Fully on-device; nothing leaves the phone. Master toggle in the (math-gated) Parents tab; opt-in and revocable.

### Effort
**~1–1.5 days**, client only.

### Open decisions
1. Calendar reminder as the **primary** mechanism — confirmed sensible given offline + iOS? (Recommended.)
2. Default time (5 PM) and copy tone.

### Acceptance criteria
- A parent can set a time and add a recurring **calendar** reminder the device honors **offline**.
- Opening the app with a streak at risk shows a clear in-app nudge.
- Where the browser supports local scheduled notifications, one fires offline; everywhere else it degrades to calendar + in-app with **no errors and zero network calls**.

---

## Feature 4 — Multiple athlete profiles + cross-device backup

### Goal
Support families with several kids on one device, and make moving/recovering data painless (today data is trapped in one browser's `localStorage`; only a manual JSON file exists).

### Part A — Multiple profiles (no backend; the main win)
**Storage redesign**
```
localStorage:
  aq_profiles = [ {id, name, sport, emoji, avatar, level, lastUsed} , ... ]   // lightweight index
  aq_state__<id> = <full state JSON>                                          // one per profile
  aq_active = <id>                                                            // currently open profile
```
- `boot()` reads `aq_active`, loads `aq_state__<id>`.
- `save()` writes to `aq_state__<active>` and refreshes that profile's index entry.
- **Migration:** on first launch of this version, if legacy `aq_state` exists, create a profile from it, set it active, keep a backup of the old key for one version.

**UX**
- **Profile switcher**: tap the athlete avatar in the header → sheet listing profiles (avatar + name + sport + level) → tap to switch, or **"+ Add athlete"** (runs onboarding, creates a new profile — this replaces today's destructive "New session" behavior), or **manage** (rename / delete one).
- Splash also offers "Continue as <name>" / "Switch athlete".
- Keep **Delete my data** as per-profile delete **and** a separate "Delete everything on this device".

**Effort:** **~1.5–2.5 days** — touches `boot()`, `save()`, splash, `newSession`/`freshOB`, plus the switcher UI and migration. Self-contained and testable.

### Part B — Easy backup & restore (no backend)
Build on the existing `exportData()` / `shareData()` / `importData()`:
- **Export all profiles** (not just the active one) to one file.
- **QR transfer for a single profile** when small enough: render the (compressed) profile JSON to a QR; scan on the second device to import. Falls back to file/share when too large (lots of results) — be honest about the size limit.
- **"Copy backup" / "Paste backup"** text option as a universal fallback.

**Effort:** **~1 day.**

### What we are NOT doing here
- **Automatic cloud sync across devices** would require a server, which the offline requirement rules out. Backup/restore + QR + clipboard is the on-device substitute for moving data between devices.

### Privacy & safety
- Everything stays local. Delete is math-gated. QR/file/clipboard transfers are user-initiated and contain only that athlete's own data.

### Open decisions
1. ✅ **Confirmed:** the destructive "New session" becomes the non-destructive **"Add athlete"** (the current athlete is kept as its own profile).
2. Max number of profiles to show (soft cap, e.g., 6)?
3. Include QR transfer now, or just file + clipboard?

### Acceptance criteria
- Can create, switch between, rename, and delete multiple athletes; the right data loads for each.
- Existing single-athlete users are migrated automatically with no data loss.
- A profile can be exported and re-imported on another device and lands intact.

---

## Feature 5 — Age-group benchmarks + shareable achievement cards

### Part A — Age-group benchmarks
**Goal:** Add motivating *context* to the forecast — "you're 1.2s from the next standard" instead of only comparing to the kid's own goal.

**Approach (start small and honest)**
- Add `BENCHMARKS[sport][event] = [{ level, byAge: { 12: value, 13: value, ... } }]` for the events where credible standards are well established (e.g., **swimming** motivational time standards; **track** age-group marks). Levels like "Developing → Competitive → Advanced → Elite" (kept generic to avoid implying official certification).
- On the **Stats forecast card**: show the athlete's current band ("≈ Competitive") and the gap to the next level; optionally draw a faint **benchmark line** on `chartSVG` (we already render a goal line — same mechanism).
- Always allow a **parent/coach-set custom standard/target** so unsupported sports still benefit.

**Accuracy caveat (important):** Authoritative, current standards are sport-governing-body data and can carry licensing/accuracy concerns. Recommend: ship a **clearly-labeled "approximate guide"** for a few sports first, sourced and reviewed, plus the custom-target option — rather than claiming official standards for all 10 sports. I can research and assemble a sourced set as a follow-up before building this part.

**Effort:** rendering **~1 day**; **data sourcing/review is the real cost** and should be a separate, checked task.

### Part B — Shareable achievement cards
**Goal:** Let kids share a PR / level-up / badge as a polished image (to a parent or coach) — delight + a little organic reach.

**Approach**
- A `shareCard(kind, payload)` builds an SVG card (brand gradient, the athlete's avatar via `avatarSVG`, the headline stat, date, AthleteQuest mark), rasterizes via canvas → PNG.
- Share via the existing `navigator.share({files})` path (already used for backups); fall back to download.
- Trigger points: the **New PR** modal and **level-up** modal get a "📸 Share this win" button; badges get a share action.

**Effort:** **~1 day.**

### Privacy & safety
- Cards include first name + the stat only — **no location, no precise dates beyond the event date, no last name**. Sharing is always user-initiated through the OS share sheet (parent controls the destination). Keep a Parents toggle to disable sharing entirely.

### Open decisions
1. Which sports get benchmark data first? (Recommend swimming + track, where standards are clearest.)
2. Level naming ("Developing/Competitive/Advanced/Elite" vs sport-specific labels)?
3. ✅ **Confirmed:** I'll research + assemble a sourced benchmark dataset (swimming + track first) before building Part A.

### Acceptance criteria
- For a supported sport/event/age, the forecast shows the current band and the gap to the next level (or a custom target works for any sport).
- A PR or level-up can be shared/saved as a clean image containing only non-sensitive info.

---

## Recommended build order & rough effort

| Order | Item | Effort | Notes |
|------:|------|:------:|-------|
| 1 | **5B — Share cards** | ~1 day | Pure client, high delight. Fastest win, no dependencies. |
| 2 | **4A — Multiple profiles** | ~1.5–2.5 days | Biggest everyday-use improvement; "Add athlete" is non-destructive. |
| 3 | **4B — Backup / QR / clipboard** | ~1 day | Rides on 4A; the offline way to move data between devices. |
| 4 | **2 — Reminders (calendar + in-app)** | ~1–1.5 days | 100% offline; calendar `.ics` is the primary mechanism. |
| 5 | **5A — Benchmarks** | ~1 day build + research | I assemble a sourced dataset (swimming + track) first. |

**Net:** every item is now fully on-device / no-backend — nothing depends on a network. **5B** has zero dependencies and is the fastest win; **5A** comes last because it waits on the benchmark dataset I'll research.

### Decisions — status
1. ✅ **Reminders:** fully offline (no backend). Calendar `.ics` is the primary mechanism, plus in-app nudges. *(Still open: confirm 5 PM default + copy tone.)*
2. ✅ **Profiles:** "New session" becomes the non-destructive **"Add athlete"**.
3. ✅ **Benchmarks:** I'll research + assemble a sourced dataset (swimming + track first) before building 5A.
