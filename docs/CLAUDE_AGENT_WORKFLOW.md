# How to Use Claude Agents for Singomantic

A step-by-step guide for building this app using Claude Code agents effectively.

---

## Overview

You have 5 specialized agents in this project:

| Agent | Role | When to Use |
|-------|------|-------------|
| `/architect` | System design, tech decisions | Before starting any new phase |
| `/project-manager` | Task planning, prioritization | Start of each week or phase |
| `/dev-eng` | Write and fix Flutter code | Every coding task |
| `/qa` | Test plans and test code | After each feature is built |
| `/quality-eval` | Code review | Before every git commit |

The agents work in a loop. Each feature goes through all 5 agents before it is committed.

---

## The Agent Loop (Use This Every Feature)

```
project-manager → architect → dev-eng → qa → quality-eval → commit
```

---

## Step-by-Step Workflow

---

### STEP 1 — Project Manager: Plan the Phase
**Agent:** `/project-manager`
**When:** Start of each new phase or when you're not sure what to build next.

**What to say:**
> "We are starting Phase 2. What are the tasks I need to complete, in order?"

**What you get back:**
- Ordered task list
- Which agent handles each task
- What each task depends on

**Time estimate:** 5–10 minutes

---

### STEP 2 — Architect: Design Before You Build
**Agent:** `/architect`
**When:** Before building anything non-trivial. Skip for tiny UI changes.

**What to say:**
> "I need to design the record and playback feature. What is the architecture — screens, data flow, packages, and file storage?"

**What you get back:**
- Component diagram
- Data model
- Package recommendations
- Risks and open questions

**Time estimate:** 10–20 minutes

**When to skip:** Simple UI-only changes with no new data or packages.

---

### STEP 3 — Dev Eng: Build One Feature at a Time
**Agent:** `/dev-eng`
**When:** You have a clear task from the project manager and a design from the architect.

**Golden rule: One task per prompt. Never ask for two features at once.**

**Good prompts:**
> "Create a Flutter screen with a Record button. Use the `record` package. Save audio to the local documents directory as .m4a. Show me the full code for recording_screen.dart."

> "The recording timer is not updating the UI. Here is the code and the error: [paste code and error]. Fix it."

**Bad prompts:**
> "Build the recording and playback screens and add pitch detection."

**What you get back:**
- Complete, runnable Dart code
- Which file to put it in
- pubspec.yaml changes needed
- iOS/Android config changes

**Time estimate per task:** 10–30 minutes (including pasting, running, debugging)

**Typical number of dev-eng tasks per phase:** 5–8 tasks

---

### STEP 4 — QA: Write Tests for What Was Built
**Agent:** `/qa`
**When:** A feature is working and you want to lock in its behavior.

**What to say:**
> "The recording screen is built. Write a test plan and Flutter widget tests covering: permission request, record start/stop, timer update, and file save."

**What you get back:**
- Numbered test plan
- Flutter test file with runnable tests
- Edge cases to manually verify

**Time estimate:** 15–30 minutes per feature

**How to run tests:**
```bash
flutter test
```

---

### STEP 5 — Quality Eval: Review Before Committing
**Agent:** `/quality-eval`
**When:** Before every `git commit`. Paste the code that was written.

**What to say:**
> "Review this Flutter code before I commit it. [paste recording_screen.dart]"

**What you get back:**
- Issues ranked Critical / High / Medium / Low
- Specific fix suggestions
- Final verdict: Ready / Needs fixes / Needs rework

**Time estimate:** 5–15 minutes

**Rule:** Fix all Critical and High issues before committing. Medium and Low can be filed as future tasks.

---

### STEP 6 — Commit and Push
After quality-eval gives the green light:

```bash
git add lib/screens/recording_screen.dart
git commit -m "Add recording screen with mic capture and timer"
git push origin feature/audio-recording
```

**Time estimate:** 2–5 minutes

---

## Full Example: Building the Recording Screen

| Step | Agent | Prompt Summary | Time |
|------|-------|---------------|------|
| 1 | `/project-manager` | "Break down Phase 2 into tasks" | 10 min |
| 2 | `/architect` | "Design the recording feature architecture" | 15 min |
| 3 | `/dev-eng` | "Create recording_screen.dart with record button" | 20 min |
| 3b | `/dev-eng` | "Add MM:SS timer that updates during recording" | 15 min |
| 3c | `/dev-eng` | "Add permission handling for iOS and Android" | 15 min |
| 4 | `/qa` | "Write tests for recording screen" | 20 min |
| 5 | `/quality-eval` | "Review recording_screen.dart" | 10 min |
| 6 | — | git commit and push | 5 min |
| **Total** | | | **~1.5–2 hrs** |

---

## Time Estimates by Phase

### Phase 1 — Environment Setup
| Task | Time |
|------|------|
| Install Flutter | 30–60 min |
| Install Android Studio | 30 min |
| Install Xcode (Mac) | 60–120 min |
| Run hello world on simulator | 30 min |
| Learn Flutter basics (optional) | 1–2 weeks |
| **Phase 1 Total** | **3–5 hours setup** |

---

### Phase 2 — Record and Playback (Local)
| Task | Time |
|------|------|
| Home screen with record button | 1–2 hrs |
| Recording screen with timer | 1–2 hrs |
| Save audio to local storage | 1 hr |
| Playback screen with seek bar | 2–3 hrs |
| Recordings list screen | 1–2 hrs |
| Swipe to delete | 1 hr |
| Permission handling (iOS + Android) | 1 hr |
| QA + quality-eval for all screens | 2–3 hrs |
| **Phase 2 Total** | **~10–15 hours** |

---

### Phase 3 — Pitch Detection
| Task | Time |
|------|------|
| Integrate pitch_detector_dart | 2–3 hrs |
| Display live pitch on screen | 2–3 hrs |
| Compare pitch to target note | 3–5 hrs |
| Show correct / sharp / flat feedback | 2–3 hrs |
| Tune detection accuracy | 3–8 hrs |
| QA + quality-eval | 3–4 hrs |
| **Phase 3 Total** | **~15–25 hours** |

*Note: Pitch detection accuracy tuning is the hardest part and time varies widely.*

---

### Phase 4 — Upload Note Sequences (JSON/MIDI)
| Task | Time |
|------|------|
| Define JSON note format | 1 hr |
| File picker to import JSON | 2–3 hrs |
| Parse and display note sequence | 2–3 hrs |
| Play reference tone for each note | 3–4 hrs |
| MIDI file support | 5–10 hrs |
| QA + quality-eval | 3–4 hrs |
| **Phase 4 Total** | **~15–25 hours** |

---

### Phase 5 — Measure-by-Measure Scoring
| Task | Time |
|------|------|
| Align singing timing to score | 5–10 hrs |
| Score per note (pitch accuracy %) | 3–5 hrs |
| Score per measure | 3–5 hrs |
| Visual pitch graph over time | 4–6 hrs |
| QA + quality-eval | 4–5 hrs |
| **Phase 5 Total** | **~20–30 hours** |

---

### Phase 6 — UI Polish
| Task | Time |
|------|------|
| Piano roll visualization | 5–10 hrs |
| Color-coded pitch feedback | 2–3 hrs |
| Practice history screen | 3–5 hrs |
| Song library screen | 3–5 hrs |
| QA + quality-eval | 4–5 hrs |
| **Phase 6 Total** | **~20–30 hours** |

---

## Full Project Time Summary

| Phase | Description | Estimated Hours |
|-------|-------------|----------------|
| 1 | Environment Setup | 3–5 hrs |
| 2 | Record & Playback | 10–15 hrs |
| 3 | Pitch Detection | 15–25 hrs |
| 4 | Note Sequences | 15–25 hrs |
| 5 | Measure Scoring | 20–30 hrs |
| 6 | UI Polish | 20–30 hrs |
| **Total** | **V1 Complete App** | **83–130 hours** |

At **2–3 hours per day** of focused work with Claude:
- Phase 2 done in **1–2 weeks**
- Phases 2–4 done in **6–10 weeks**
- Full V1 done in **3–5 months**

---

## Tips for Working with Claude Agents

### Do
- Give one task at a time
- Paste the full error message when debugging
- Run the code after every change before asking for more
- Commit working code frequently — small commits
- Use `/project-manager` when you feel lost or overwhelmed

### Don't
- Ask an agent to build multiple features at once
- Skip `/quality-eval` before committing
- Continue building on broken code — fix errors first
- Ask for architecture and code in the same prompt

### When You Hit a Bug
1. Copy the full error from the terminal
2. Copy the relevant code
3. Open `/dev-eng` and paste both
4. Ask: "Here is my code and the error. What is wrong and how do I fix it?"

### When You're Stuck on Design
1. Open `/architect`
2. Describe what you're trying to build in plain English
3. Ask: "What is the best way to architect this given our stack?"

### When You Don't Know What to Do Next
1. Open `/project-manager`
2. Say: "We finished [last thing]. What should we build next and in what order?"

---

## Branch Strategy

```
main                          ← stable, working code only
feature/phase2-recording      ← current work
feature/phase2-playback
feature/phase3-pitch
```

**Rule:** Never commit broken code to `main`. Merge a feature branch only after tests pass and quality-eval approves.

---

## Recommended Daily Workflow

```
1. Open /project-manager → "What is the next task?"
2. Open /dev-eng → build the task
3. Run the app on simulator — does it work?
4. If bug: paste error into /dev-eng → fix
5. Open /qa → write tests
6. Open /quality-eval → review the code
7. Fix any Critical/High issues
8. git commit + push
9. Repeat
```

One focused session (2–3 hours) = 1–2 completed tasks with tests and review.
