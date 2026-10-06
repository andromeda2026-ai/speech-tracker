# 10,000-Hour Speech & Vocabulary Tracker
Owner: Sanasam. Maintainer: Speech Tracker Dev bot (id d0dfa41a-db21-4c2d-8dc9-4033567196b8). Built 2026-10-05.

Goal: improve vocabulary, diction, and speech to get promoted. Practice at least 30 min a day (goal), 1 hour when possible (stretch). Family comes first, so 30 min counts as a full win.

## Files
- index.html: the whole app (HTML, CSS, JS), no external libraries
- manifest.webmanifest + icon.svg: "Add to Home Screen" install
- sw.js: service worker, caches everything so it runs offline. Bump CACHE version on every change.

## How it works
- Countdown starts at 10,000 h (startHours). Remaining = 10,000 − (sum of logged minutes ÷ 60).
- +30 min and +1 hour buttons log today. "Other amount" logs custom minutes, an earlier date, and a note.
- Today card: under 30 min shows minutes left to the goal, 30+ is "Goal met", 60+ is "Stretch hit".
- Streak counts consecutive days with 30+ min. Today counts once it's met; otherwise the streak runs through yesterday. Best streak, minutes this week, a 28-day heatmap, and a projected finish at your average pace.
- History list, where ✕ removes a mistaken entry and adds its time back to the countdown.
- Data: localStorage key `tenK_speech_v1` = {version, startHours, sessions:[{id,date,minutes,note,at}]}. It stays on the phone only, with JSON export/import for backup.

## Rules for changes
Never wipe user data. Migrate any schema change from version 1. Keep it offline-first, with no accounts and no server.

## v2 (2026-10-05 11 PM ET): guided sessions (user asked by voice)
- Sessions aren't fixed at 30 min. Start a guided session at 30 min, 1 hour, or 2 hours depending on the day. Quick +30 / +1h logging still works for practice done outside the app.
- Each session walks through timed steps with prompts from prompts.js:
  - 30 min: warm-up 5, diction 5, vocabulary 7 (3 words), speak on the spot 8, review 5
  - 1 hour: warm-up 8, diction 10, vocab 12 (5 words), read aloud 10, speak 12, review 8
  - 2 hours: warm-up 10, diction 15, vocab 20 (8 words), read aloud 20, speak 25, storytelling 15, review 15
- Vocabulary rotates through a 60-word professional list (S.wordCursor), so no repeats until the list is done. Speaking topics are promotion and workplace scenarios, recorded with the phone's voice recorder.
- The timer is timestamp-based, so it survives a screen lock or app switch. The in-progress session is saved in localStorage key `tenK_active_v1`. Pause/resume works, and the phone vibrates at the end of each step. Finish & log records the minutes actually done, even if stopped early.
- End-of-session self-rating: clarity, pace, confidence (1–5), filler-word count, and one thing to fix. The Improvement card compares the first 5 vs latest 5 guided sessions and counts vocabulary words practiced.
- Daily push: the top banner shows minutes left to 30, gets firmer after 7 PM with nothing logged, and turns green at 30 and blue at 60.
- Schema version 2: sessions may add {mode:'guided', planned, ratings, fillers, words}. v1 data loads fine.
- Known limit: it can't send phone notifications while closed, since it has no server. Reminders need a phone alarm, or a Grok Bot routine can ping by chat.
