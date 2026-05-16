# Singomantic — Project Plan

## Vision

A lightweight collaborative music platform for choirs and vocal groups. Members can practice individually, receive pitch feedback, and share recordings — all within a shared choir workspace.

---

## Architecture

### Mobile App
- **Framework:** Flutter (iOS + Android)

### Backend
- **Platform:** Firebase
  - Authentication
  - Firestore (database)
  - Firebase Storage (audio/MIDI files)
  - Realtime sync

### Source Control
- GitHub with feature branches, small commits, and versioned releases

---

## App Structure

### Users
Each choir member has:
- Account (Firebase Auth)
- Recordings
- Practice history

### Choir Groups
Examples: Church choir, Youth choir, Chamber choir

Each group contains:
- Members
- Song library
- Uploaded recordings

### Songs
Each song contains:
- Title
- Sheet music
- MIDI or note data
- Reference recordings
- Practice tracks

### Practice Sessions
Each user can:
- Record attempts
- Receive pitch feedback
- Upload and share recordings

---

## Technical Approach

### Use MIDI First (Not PDFs)
- Do NOT start with PDF parsing, image recognition, or music OCR
- MIDI files already contain notes, timing, measures, and tempo
- Makes pitch comparison much easier

### Biggest Technical Challenge
> Aligning live singing to expected notes over time

This is a simplified music AI problem. Build progressively.

### Audio Analysis Strategy
| Version | Feature |
|---------|---------|
| v1 | Single-note accuracy only |
| v2 | Pitch over time graph |
| v3 | Measure scoring |
| v4 | Rhythm + pitch combined |

---

## Development Phases

### Stage 1 — Solo App (v0.1)
- [ ] Recording
- [ ] Playback
- [ ] Pitch detection
- Local only

### Stage 2 — Cloud Sync (v0.2)
- [ ] Firebase login
- [ ] Upload recordings
- [ ] Save song library

### Stage 3 — Shared Choir Features (v0.3)
- [ ] Choir groups
- [ ] Member access
- [ ] Shared recordings

### Stage 4 — Better Music Intelligence (v0.4+)
- [ ] Note-by-note scoring
- [ ] Rhythm scoring
- [ ] Measure analysis
- [ ] Pitch graph

---

## MVP Feature Set

### Choir Director Can
- Create a group
- Upload a song
- Upload MIDI
- Assign practice song to members

### Choir Member Can
- Open assigned song
- Listen to guide track
- Record singing
- See pitch accuracy
- Upload recording

---

## Database Design (Firestore)

```
users/
  {userId}/
    name, email, choirIds[], practiceHistory[]

choirs/
  {choirId}/
    name, directorId, memberIds[], songIds[]

songs/
  {songId}/
    title, midiUrl, referenceAudioUrl, choirId

recordings/
  {recordingId}/
    userId, songId, audioUrl, pitchScore, timestamp
```

---

## Flutter Packages

| Package | Purpose |
|---------|---------|
| `record` | Microphone recording |
| `just_audio` | Audio playback |
| `flutter_midi_pro` | MIDI playback |
| `firebase_auth` | Authentication |
| `cloud_firestore` | Database |
| `firebase_storage` | File storage |

---

## GitHub Workflow

- Use feature branches for each stage
- Small, focused commits
- Tag releases by milestone

### Release Tags
- `v0.1` — Recording works locally
- `v0.2` — Cloud sync + Firebase login
- `v0.3` — Pitch detection
- `v0.4` — Choir sharing

---

## Recommended Claude Workflow

Keep tasks small and focused. Example sequence:
1. "Create Firebase login screen in Flutter"
2. "Add upload audio recording to Firebase Storage"
3. "Display list of choir songs"
4. "Detect pitch from microphone input"
5. "Compare detected frequency to MIDI note"

---

## Realistic Timeline

| Timeframe | Milestone |
|-----------|-----------|
| 1–2 months | Basic working prototype |
| 3–6 months | Choir-sharing usable MVP |
| 6–12 months | Good pitch-analysis system |

---

## Future Possibilities

- Conductor feedback
- Sectional practice (SATB: soprano/alto/tenor/bass)
- AI vocal coaching
- Harmony checking
- Automatic accompaniment
- Rehearsal attendance tracking
- Choir scheduling
- Live rehearsal mode

> Build the musical core first before building community features.

---

## Full Stack Summary

| Layer | Technology |
|-------|-----------|
| Mobile | Flutter |
| Backend | Firebase |
| Source Control | GitHub |
| AI Coding | Claude + Cursor |
| Audio Data | MIDI (initially) |
| Storage | Firebase Storage |
| Database | Firestore |
