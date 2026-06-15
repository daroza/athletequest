# AthleteQuest AI — User Guide

**Live app:** https://delightful-pond-0409a311e.7.azurestaticapps.net

AthleteQuest AI is a virtual sports coach for young athletes (ages 5–18). It combines a personal AI coach, performance tracking with forecasting, age-group benchmark bands, and a game layer (XP, levels, coins, badges, quests) to help kids stay motivated and reach their sports goals. Everything runs in the browser and all data stays on the device — no account, no sign-up, nothing sent to a server. It even works offline (airplane mode).

---

## 1. Getting Started

### Try the demo first
On the welcome screen, tap **👀 Try demo athlete (Dean, swimmer)** to explore a fully loaded profile — a 12-year-old swimmer chasing a 59.5 qualifying time in 100m Freestyle, complete with results, forecasts, and quests.

### Create your own athlete
Tap **Start my adventure! ✨** and walk through the 7-step setup:

1. **Name & age** — what the coach calls you.
2. **Sport** — swimming, track, running/XC, soccer, basketball, baseball/softball, tennis, gymnastics, volleyball, hockey, or "My Sport" for anything else.
3. **About me** — height/weight (optional), position or main event, your level (Rookie → Elite), **how many years you've played**, and a quick **injury check**. If something hurts, the app asks you to talk with a parent and your **family physician** before training or competing — and it leaves a note on the Parent dashboard.
4. **Big goal** — pick goal chips (get faster, make the team, etc.). Optional turbo-mode: enter a target time/score and a date, and the app will forecast your chances of hitting it.
5. **Avatar** — build your athlete: skin, hair, jersey color, and number.
6. **AI coach** — pick a style (🤖 🐬 🦅 🦁 🧙 ⭐), name your coach, and choose a personality:
   - 🔥 **Hype** — pumps you up
   - 😎 **Chill** — calm and kind
   - 🦉 **Wise** — deep thinker
   - 🤪 **Goofy** — makes you laugh
7. The app builds your **Athlete Twin** and you're in!

### Install it like an app
In your phone browser's menu, choose **"Add to Home screen."** AthleteQuest then opens full-screen like a native app and runs offline.

### Need help?
Tap the **❓ Guide** button in the top header any time to open this user guide. Grown-ups can also reach it from the Parent Dashboard.

---

## 2. Multiple Athletes 👥

One device can hold a whole family of athletes — siblings, or one kid in two sports.

- Tap your **avatar in the top-left of the header** (or **👥 Switch / add athlete** in the Parent Dashboard) to open **Athletes on this device**.
- **Switch** to any athlete with ↪️ — the right data, coach, and progress load instantly.
- **🆕 Add an athlete** runs setup for a new profile *without* erasing anyone — the current athlete is kept safe.
- **🗑 Delete** removes a single athlete (other athletes stay). Tip: save a backup first.

Each athlete's data is stored separately on the device. Use the backup tools (section 6) to move an athlete to another phone.

---

## 3. The Five Tabs

### 🏠 Home
Your daily dashboard: a tip from your coach, your avatar, the **Athlete Twin** summary (consistency score, weekly training minutes, performance trend), today's quests, quick-log buttons (Log Practice, Add Result, **💓 Heart Check**, Log Sleep), the **Skill Radar**, your skill tree, and badge collection.

**💓 Heart Check:** a kid-friendly pulse tool. It teaches how to find a pulse (two fingers on the wrist or neck — never the thumb), runs a built-in 15-second counter, and converts beats to BPM. Do it **before** practice (rested) and **again after** — the difference shows how hard the heart worked, and a falling before-practice number over weeks is fitness you can see. Averages appear on the Parent dashboard.

**Safety check:** every practice log asks "any pain or injuries today?" Picking 🤕 stops the log, explains Rule #1 (never hurt yourself — proper technique first), advises telling a parent and consulting the **family physician** before training or competing again, and flags it for parents.

**Workout reward:** finishing a practice log plays a cute animation of your own avatar doing your sport — swimming through waves, dribbling, juggling a soccer ball, cartwheeling, and more.

**Skill Radar:** a spider chart comparing your six skill levels (Speed, Strength, Endurance, Technique, Recovery, Mental) against the ideal profile for your sport. Your two biggest gaps are called out with a specific drill to close each one.

### 🎯 Quests
- **Daily quests** (reset at midnight): 3 per day, e.g. log a practice, drink water, stretch, ask the coach a question. Some complete automatically when you do the activity.
- **Weekly challenges:** complete 3 practices and 6 daily quests per week for bonus rewards.
- **Special Mission:** a 3-step road to your big goal — log 2 results, complete 6 practices, set a new personal best. Finishing earns 150 XP, 60 coins, and the 🏆 Goal Crusher badge.

### 💬 Coach
Chat with your AI coach. It knows your real data — results, training load, sleep, goal progress — and replies in your chosen personality. It covers a wide range of questions:

- **Training:** "What should I do today?" → a workout plan based on your recent energy and training; "How do I get faster?" → power moves built from your trend.
- **Goals & forecasts:** "How close am I to my goal?" / "Predict my future time" → forecast with a probability estimate.
- **Why am I slowing down?** → checks your sleep, training volume, and how practices felt.
- **Body & habits:** nutrition, hydration, sleep & recovery, warm-up/cool-down, getting stronger, and "How do I check my heartbeat?"
- **Mind & emotions:** game-day nerves, building confidence, bouncing back from a loss, celebrating a win, focus, breathing to calm down, and teammate/coach situations.
- **Gear:** "What do I need?" → your sport's equipment list with price ranges.
- **Hard stuff:** if a kid mentions being bullied or picked on, the coach gently urges them to tell a trusted adult.

**Talk instead of typing:** tap the 🎤 button. The first use downloads a small speech model (~40MB, one time); after that the mic works on-device. Tap 🎤 to start, ⏹ to stop.

**Coach voice:** tap 🔊 in the coach header to turn the spoken replies on/off. The voice defaults to a male voice — see section 5 to change it.

**Safety guardrails:** the coach always redirects injury or pain questions to parents, coaches, and doctors; it never gives dieting, supplement, or overtraining advice to kids; and anything it can't help with is handed back to a grown-up.

### 📈 Stats
- **Add results** (race times, meet scores, skill tests). Times accept `65.2` or `1:05.2` formats. You can also **bulk-import** several results at once.
- After **3 results** in one event, the **forecast engine** turns on: a chart with your trend line, predictions for 1 and 2 months out, a confidence rating (⭐–⭐⭐⭐), and — if you set a target — your estimated **goal chance %** and a 🎯 goal line on the chart.
- **Benchmark bands** 📊: for supported timed events the forecast also shows your level — **Developing → Competitive → Advanced** — and how far you are from the next band (e.g. "1.2s from Competitive"). Covered events: swimming (50m & 100m Freestyle), track (100m Sprint, 1600m), and running (1 Mile). These are an **approximate all-around guide (~ages 11–13), not official standards** — your coach or parent can always set a custom target instead.
- Personal bests trigger a 🚀 PR celebration (with a **📸 Share this win** card — see section 4) and are tracked per event.
- Recent activity list lets you review or delete entries.

The forecast uses linear regression over your logged results — the more you log, the smarter it gets.

### 🎒 Locker
Spend coins earned from quests:
- **👕 Jerseys** — colors plus special gradients (Ocean Wave, Galaxy, Champion Gold)
- **👟 Shoes**, **🎩 Extras** (shades, goggles, medal, cape, crown), **✨ Effects** (sparkles, fire feet, lightning, rainbow)
- **🛒 Gear** — a *real-world* gear guide for your sport: what you actually need, rough US price ranges, and fit tips. Tailored to how long you've played, with a reminder to always shop with a parent.
- **🤖 Coach tab** — rebuild your coach: name, personality, skin, hair, outfit, headwear, gear (whistle, clipboard, megaphone…), and **voice** (see below)

---

## 4. Share Your Wins 📸

When you set a **personal best** or **level up**, the celebration screen has a **📸 Share this win** button. It builds a polished image card — your avatar, the headline stat, and the date — and sends it through your phone's share sheet (to a parent or coach), or saves it to your photos.

For privacy, cards only ever show your **first name and the stat** — no last name, no location.

---

## 5. Voice Settings

**Coach speaks:** Locker → 🤖 Coach tab → **Voice**.
- **Auto (male)** is the default — the app picks the best male English voice on your device.
- Or choose any voice from the dropdown; tap **▶️ Test** to hear a sample. The choice is saved.
- Voices come from your phone/computer, so the list differs by device. On Android you can add voices in Settings → Text-to-speech.

**You speak:** the 🎤 mic button on the Coach chat bar (needs microphone permission; first use downloads the speech model).

---

## 6. Parent Dashboard 👪

Tap the **👪** button in the header. A quick multiplication question keeps kids out.

Inside you'll find:
- **Athlete summary** — level, streak, quests, years of experience, and average pre/post-practice heart rate
- **🩺 Health note** — appears if your child reported an injury (at setup or before a practice), with a reminder to consult your family physician; clear it once resolved
- **📅 Consistency calendar** — which of the last 28 days had training
- **📊 Weekly training volume** — minutes per week, last 4 weeks
- **🎯 Goal progress** — current best, predicted result, and estimated chance
- **😴 Recovery** — average sleep from check-ins
- **⏰ Practice reminders** *(new)* — set a daily reminder time and tap **📅 Add to my calendar**. This writes a recurring daily event (with an alarm) to your phone's own Calendar, so it reminds you **even when the app is closed and even in airplane mode** — no accounts or push servers. A **🔔 In-app nudges** toggle adds a friendly "keep your streak alive" prompt when you open the app with a streak at risk.
- **🔒 Privacy card** — confirms all data stays on the device (nothing uploaded, no accounts)
- **🛡️ Responsible AI note** — what the coach will and won't say to kids
- **Data controls:**
  - **📤 Save backup file** — downloads a JSON backup of the current athlete
  - **✉️ Email / share** — share that backup via the device share sheet
  - **📥 Restore backup** — load a backup file; it's added as an athlete and the others are kept
  - **👥 Switch / add athlete** — open the multi-athlete sheet (section 2)
  - **📦 Backup all athletes** *(new)* — save one file containing every athlete on the device
  - **📋 Copy / paste backup** *(new)* — move an athlete to another device with **no internet**: copy the backup text here, paste it into AthleteQuest on the other phone
  - **🗑️ Delete this athlete** — remove just the current athlete (others are kept)
- **Sound on/off** toggle for game sound effects
- **User Guide** link — opens this guide

---

## 7. The Game Layer

| Element | How it works |
|---|---|
| **XP & Levels** | Earn XP for logging practices (+25), results (+20), PRs (+40), quests, and check-ins. Level-ups award bonus coins — and a shareable win card. |
| **🪙 Coins** | From quests, level-ups, and skill-ups. Spend in the Locker. |
| **🔥 Streak** | Open the app and be active on consecutive days. Don't let the flame die! |
| **🏅 Badges** | 13 to collect — streaks, PRs, workout milestones, chat milestones, and more. |
| **🌳 Skills** | Each workout type grows a matching skill (e.g. speed work → ⚡ Speed). Every 100 points = 1 level + 15 coins. |

---

## 8. Data & Privacy

- All data — profiles, results, workouts, heart checks, and chats — lives in the browser's local storage on the device. Nothing is uploaded.
- Each athlete is stored separately; deleting one leaves the others untouched.
- Forms validate against impossible values (ages, dates in the future, extreme times, heart rates, sleep hours, etc.), so typos can't corrupt the data.
- Different devices (or browsers) have separate data — use backup/restore, "backup all," or copy/paste transfer to move athletes between devices.
- Clearing browser data erases the app's data, so save a backup occasionally.
- The only internet use: loading the app, fonts, and the one-time voice-model download. Everything else — including coach replies and reminders — runs on-device and works offline.

---

## 9. Tips & Troubleshooting

- **Forecast says it needs more results** — log at least 3 results in the same event.
- **No benchmark band showing** — bands only appear for the supported timed events listed in section 3; for anything else, set a custom target instead.
- **Reminder didn't fire** — make sure you tapped **📅 Add to my calendar** and allowed it to be added; the reminder lives in your phone's Calendar app, not inside AthleteQuest.
- **Mic button shows 📡 error** — the one-time model download needs internet; try again online.
- **No sound/voice** — check the 🔊 toggle in the coach header, the sound toggle in the Parent dashboard, and the phone's mute switch.
- **Voice sounds female on Auto** — the device may have no male English voice installed; pick one manually or add one in the device's text-to-speech settings.
- **Moving to a new phone** — Parents → Save backup file (or 📦 Backup all athletes, or 📋 Copy / paste backup) → send it to the new device → Restore / paste it.
- **App looks zoomed/cramped** — it's designed portrait-first for phones; on desktop it centers in a phone-width column.

Have fun, train smart, and let Coach do the cheering! 🏆
