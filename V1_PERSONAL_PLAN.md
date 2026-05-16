# Singomantic — V1 Personal Practice App Plan

## Overview

A solo singing practice app built incrementally. Focus on getting core audio working before adding any polish or social features.

### Core Features
- Upload sheet music / notes
- Play reference notes or accompaniment
- Record singing
- Detect pitch accuracy
- Compare pitch by measure
- Show feedback visually
- Save user recordings / history

### Strategy
1. Build a very small prototype first
2. Get pitch detection working
3. Add music-note alignment later
4. Add polish only after core audio works

---

## Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Mobile | Flutter | One codebase for iOS + Android, good audio libs |
| AI Coding | Claude + Cursor / VS Code | Best for iterative small tasks |
| Version Control | GitHub | Feature branches, frequent commits |
| Local Storage | SQLite or local files | No backend needed in V1 |

---

## GitHub Branch Structure

```
main
feature/audio-recording
feature/pitch-detection
```

---

## Phase-by-Phase Plan

### Phase 1 — Learn Basics (1–2 weeks)
**Goal:** Understand enough to work with Claude effectively.

Learn:
- What Flutter is
- Basic Dart language
- GitHub basics
- Running app on phone emulator

Install:
- Flutter SDK
- Android Studio
- Xcode (Mac, for iPhone)
- VS Code or Cursor

---

### Phase 2 — Minimal Recording App (1–2 weeks)
**Goal:** Simple recording app, no music analysis yet.

Features:
- [ ] Press Record
- [ ] Save audio locally
- [ ] Playback recording

Flutter packages:
- `record`
- `audioplayers`

Example Claude prompts:
- "Create Flutter app with audio recording."
- "Save recordings locally."
- "Add playback controls."

---

### Phase 3 — Pitch Detection Prototype (2–4 weeks)
**Goal:** Detect sung pitch in real time.

Key concept: Convert audio frequency → musical note.

Flutter packages:
- `pitch_detector_dart`
- `tartini`

Features:
- [ ] Show current detected pitch
- [ ] Compare to target note
- [ ] Simple "sharp / flat / correct" feedback

> This is the first exciting milestone.

---

### Phase 4 — Upload Notes / Music (2–6 weeks)
**Goal:** Load simple note sequences into the app.

**Do NOT start with PDF sheet music parsing.**

Use simple JSON format first:
```json
[
  {"note": "C4", "duration": 1},
  {"note": "D4", "duration": 1}
]
```

Later add:
- MIDI import
- MusicXML support

---

### Phase 5 — Measure-by-Measure Pitch Scoring (ongoing)
**Goal:** Compare singing against expected notes over time.

Early version:
- One note at a time
- User sings slowly
- Compare average pitch

Advanced later:
- Real-time continuous analysis
- Timing alignment
- Tolerance ranges

Possible scoring output:
- Accuracy %
- Pitch curve graph
- Rhythm timing

---

### Phase 6 — UI Improvements
- [ ] Piano roll visualization
- [ ] Colored pitch feedback
- [ ] Progress tracking
- [ ] Practice history
- [ ] Song library

---

## MVP (Version 1) — Keep It Very Small

| Include | Skip for Now |
|---------|-------------|
| Upload simple note file | Social features |
| Play reference tone | AI singing coach |
| Record singing | Fancy animations |
| Detect pitch | Karaoke video |
| Show correct / incorrect | Sheet music OCR |

---

## Good First Milestones

| Week | Goal |
|------|------|
| Week 1 | Run Flutter app on simulator |
| Week 2 | Record and playback voice |
| Week 3 | Display detected pitch |
| Week 4 | Compare pitch against target note |
| Month 2 | Upload simple melodies |
| Month 3 | Measure-by-measure scoring |

---

## Recommended Development Workflow

1. Ask Claude for one small feature
2. Paste generated code into project
3. Run the app
4. Copy errors back to Claude
5. Commit working state to GitHub
6. Repeat

---

## Important Technical Reality

Pitch detection itself is manageable. The hard parts are:
- Aligning singing timing to the score
- Noisy microphone audio
- Latency
- Rhythm detection

**So start with:**
- Isolated single notes
- Slow singing
- Simple exercises

Then grow complexity gradually.

---

## Future Advanced Features (Post-MVP)

- AI vocal coaching
- Vibrato analysis
- Breathing analysis
- Piano accompaniment
- Karaoke mode
- Teacher / student accounts
- Cloud sync
- Singing games

---

## Mindset

Treat this as **many tiny engineering problems**, not one giant app.

> Build tiny working versions. Test constantly. Simplify aggressively. Avoid over-planning.
