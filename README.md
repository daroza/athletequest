# AthleteQuest AI

**Your Personal AI Athlete Twin** — a virtual sports coach that helps young athletes train smarter, stay motivated, and reach their goals.

Built by **Dean Daroza** (Grade 7, St. Francis Xavier School).

---

## About

AthleteQuest AI is an AI-powered coaching app for young athletes. You set a goal, log your training, and talk to an AI coach that knows your whole history — by typing or by voice. The app builds a personalized **Athlete Twin** from your results, training, sleep, and goals, then forecasts your future performance and tells you exactly what to work on.

Best of all, it runs **100% on your device** — no accounts, no servers, no cloud, and no data collection. Everything stays private on your own phone.

## Features

- **AI Coach** — ask questions in plain language (type or speak) and get advice computed from your own data, not generic tips.
- **Athlete Twin** — a living profile of your sport, goals, personal records, training, and recovery.
- **Performance forecasting** — a model trained on your own logs predicts your next result and your odds of hitting your goal, and re-trains every time you add a result.
- **Skill Radar** — compares you to an ideal athlete your age and shows your two biggest gaps plus drills to close them.
- **Gamification** — XP, levels, streaks, badges, a coin shop, and an avatar that celebrates your workouts in your sport.
- **Daily quests** — personalized challenges to keep you progressing.
- **Parent dashboard** — parents can see activity, health flags, and progress at a glance.
- **Safe by design** — if you report an injury, the app stops training advice and refers you to a parent and physician; it avoids unsafe diet/overtraining advice.
- Works across **9 sports**.

## How the AI works (all on-device)

- **Rule-based coaching engine** over the Athlete Twin — deterministic and kid-safe.
- **Linear-regression forecasting** trained locally on your performance logs.
- **On-device speech-to-text** (Whisper tiny via transformers.js) for voice chat — audio never leaves the device.

No cloud AI is used. An optional on-device LLM chat (e.g. Google Gemma) is planned as a future, still-fully-offline upgrade.

## Tech stack

- Single self-contained **HTML / CSS / vanilla JavaScript** file — no framework, no build step.
- Installable **Progressive Web App** that works fully offline.
- **Browser `localStorage`** for all data — no database, no backend, no accounts, no analytics.

## Getting started

Because the whole app is one HTML file, you can run it with no setup:

1. Open `AthleteQuest.html` directly in any modern browser, **or**
2. Serve the folder locally and open it on your phone to install it as a PWA:

   ```bash
   # from the project folder
   python -m http.server 8000
   # then visit http://localhost:8000/AthleteQuest.html
   ```

See `userguide.html` (or `userguide.md`) for a full walkthrough.

## Project files

- `AthleteQuest.html` — the complete app.
- `pwa-assets/` — icons and assets for installing as a Progressive Web App.
- `userguide.html` / `userguide.md` — user guide.
- `AthleteQuest_AI_*` / `AthleteQuest_Spec_*` — product requirements and design specs.
- `AthleteQuest_Local_LLM_Requirements.md` — design notes for the future on-device LLM.

## License

Released under the **MIT License** — free and open source. See [LICENSE](LICENSE).

© 2026 Dean Daroza
