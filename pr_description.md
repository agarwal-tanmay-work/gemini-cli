## Summary

This PR implements a real-time **Voice Mode** for Gemini CLI, allowing users to
dictate prompts directly into the terminal. It supports both cloud-based
transcription via the Gemini Live API and local-first transcription via Whisper
(using `whisper.cpp`).

## Details

- **Transcription Backends**:
  - **Gemini Live API (Cloud)**: High-accuracy, real-time transcription using
    Google's Live API. Requires an API key.
  - **Whisper (Local)**: Privacy-focused, local-first transcription. Supports
    multiple model sizes (Tiny, Base, Large) and automatically manages model
    downloads to `~/.gemini/whisper_models/`.
- **UI Integration**:
  - New `/voice` slash command to toggle voice mode and switch backends.
  - **Push-To-Talk (PTT)**: Hold `space` to record, release to stop and submit.
  - **Continuous Mode**: Dictate naturally with real-time text updates in the
    input buffer.
  - Dedicated Voice settings in the configuration dialog.
- **Audio Infrastructure**:
  - Uses `sox` (`rec`) for cross-platform audio capture.
  - Robust handling of audio streams, including automatic VAD (Voice Activity
    Detection) support where available.

## Installation Requirements

To use Voice Mode, you must install the following dependencies:

### 1. SoX (Sound eXchange)

Required for capturing audio from your microphone.

- **macOS**: `brew install sox`
- **Linux**: `sudo apt install sox libsox-fmt-all`
- **Windows**: Download and install from
  [SoX SourceForge](https://sourceforge.net/projects/sox/). Ensure `sox.exe` is
  in your `PATH`.

### 2. whisper-stream (for Local Transcription)

Required only if using the **Whisper (Local)** backend.

1. Clone the [whisper.cpp](https://github.com/ggerganov/whisper.cpp) repository.
2. Build the `stream` example:
   ```bash
   make stream
   ```
3. Rename the resulting `stream` binary to `whisper-stream` and move it to a
   directory in your `PATH`.

## Testing

1. **Enable Voice Mode**: Run `/voice on` in the CLI or toggle it in
   `/settings`.
2. **Push-To-Talk**: Hold `space`, speak a prompt, and release. The text should
   appear in the input and be submitted.
3. **Switch Backends**:
   - For Cloud: `/settings` -> Voice -> Transcription Backend -> Gemini Live.
   - For Local: `/settings` -> Voice -> Transcription Backend -> Whisper.
4. **Validation**:
   - Run unit tests:
     `npm test packages/core/src/voice/liveTranscriptionService.test.ts`
   - Run integration tests: `npm test integration-tests/voice-mode.test.ts`

## Related Issues

Partially addresses #2592.

## Checklist

- [x] I have read the [CONTRIBUTING.md](CONTRIBUTING.md) document.
- [x] I have added/updated tests to cover my changes.
- [x] I have updated the documentation (if applicable).
- [x] I have run `npm run preflight` and all checks passed.
