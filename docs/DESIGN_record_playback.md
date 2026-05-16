# Design Doc: Record & Playback Voice

**Phase:** V1 Stage 2  
**Status:** In Progress  

---

## Goal

Let a user record their singing voice and play it back. No pitch analysis yet — just reliable audio capture and playback on both iOS and Android.

---

## User Flow

```
Home Screen
    │
    ▼
[ Record Button ]
    │ tap to start
    ▼
[ Recording Screen ]
  - Live timer (0:00, 0:01 ...)
  - Waveform animation (visual feedback)
  - Stop button
    │ tap to stop
    ▼
[ Playback Screen ]
  - Play / Pause button
  - Seek bar
  - Recording duration
  - Save button
  - Discard button
    │ save
    ▼
[ Recordings List ]
  - List of saved recordings
  - Tap to play any recording
  - Swipe to delete
```

---

## Screens

### 1. Home Screen
- Large **Record** button (center)
- Link to **Recordings** list

### 2. Recording Screen
- Red pulsing record indicator
- Timer showing elapsed time (MM:SS)
- Simple waveform animation to show mic is active
- **Stop** button

### 3. Playback Screen (post-recording)
- Filename / timestamp label
- **Play / Pause** toggle button
- Seek bar with current position and total duration
- **Save** button → saves to local library
- **Discard** button → deletes and returns home

### 4. Recordings List Screen
- Scrollable list of saved recordings
- Each row shows: date, duration
- Tap row → opens Playback Screen
- Swipe left → delete with confirmation

---

## Technical Design

### Flutter Packages

| Package | Purpose |
|---------|---------|
| `record` | Microphone access and audio capture |
| `just_audio` | Audio playback with seek support |
| `path_provider` | Get local file storage path |
| `intl` | Format timestamps and durations |

### File Storage

Recordings saved to app's local documents directory:

```
/Documents/singomantic/recordings/
  recording_2026-05-15_143022.m4a
  recording_2026-05-15_150301.m4a
```

Filename format: `recording_YYYY-MM-DD_HHmmss.m4a`

Audio format: **AAC / M4A**
- Good compression
- Supported natively on iOS and Android
- Works well with `record` package

### State Management

Simple `StatefulWidget` for each screen — no complex state management needed at this stage.

#### Recording State
```dart
enum RecordingState { idle, recording, stopped }
```

#### Playback State
```dart
enum PlaybackState { idle, playing, paused }
```

---

## Data Model

### Recording
```dart
class Recording {
  final String id;        // UUID
  final String filePath;  // absolute path to .m4a file
  final DateTime createdAt;
  final Duration duration;
}
```

Stored as a JSON list in local app storage (no database needed for V1).

---

## Key Technical Considerations

### Permissions
- **iOS:** Add `NSMicrophoneUsageDescription` to `Info.plist`
- **Android:** Add `RECORD_AUDIO` permission to `AndroidManifest.xml`
- Request permission at runtime before first recording

### Latency
- No real-time processing in this phase — just raw capture
- Latency is not a concern until pitch detection is added

### Max Recording Length
- Cap at **10 minutes** for V1 to prevent runaway file sizes
- Show warning at 9:00 remaining

---

## Out of Scope for This Phase

- Pitch detection
- Note comparison
- Cloud upload
- Sharing recordings
- Waveform visualization of saved audio
- Audio trimming

---

## Acceptance Criteria

- [ ] App requests microphone permission on first use
- [ ] Recording starts and stops reliably
- [ ] Timer increments correctly during recording
- [ ] Recorded audio plays back at correct speed and pitch
- [ ] Seek bar works during playback
- [ ] Recordings persist after app is closed and reopened
- [ ] User can delete a recording
- [ ] Works on both iOS simulator and Android emulator

---

## Claude Prompt Sequence

Build this feature by giving Claude one task at a time:

1. `"Create a Flutter screen with a record button using the record package. Save audio to local documents directory as .m4a."`
2. `"Add a timer that counts up while recording in MM:SS format."`
3. `"Add playback of the recorded file using just_audio with play/pause and a seek bar."`
4. `"Add a scrollable list screen showing all saved recordings with date and duration."`
5. `"Add swipe-to-delete on the recordings list with a confirmation dialog."`
6. `"Add microphone permission handling for iOS and Android."`
