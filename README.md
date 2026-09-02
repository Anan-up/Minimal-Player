[English](https://github.com/Anan-up/Minimal-Player/blob/main/README.md) | [简体中文](https://github.com/Anan-up/Minimal-Player/blob/main/README_Simplified_Chinese.md) | [繁体中文](https://github.com/Anan-up/Minimal-Player/blob/main/README_Classical_Chinese.md.md)

This is a **fully self-contained local media player** (HTML + CSS + JS) with zero external dependencies. All data is processed locally in the browser only, emphasizing privacy and minimalist interaction.

---

### 1. Core Purpose & Interaction
- **Dual-mode support**: Automatically distinguishes **video** (MP4/WebM/MOV) from **audio** (MP3/WAV/FLAC/M4A).
- **Loading methods**: Supports **click-to-select** files or **drag-and-drop** files onto the playback stage.
- **Privacy & security**: Uses `URL.createObjectURL` to generate local temporary links; files are never uploaded to any server.

### 2. Playback Control System
- **Basic controls**: Play/pause (click the button or the video frame), progress scrubbing (click the waveform bar to seek), volume adjustment, and mute.
- **Advanced controls**: **Playback speed** (0.5× to 3.0×), **fullscreen mode** (adapts to both video and audio stages).
- **Status feedback**: The top shows the file name and format label; the bottom updates the playback time in real time.

### 3. Audio Visualization (Technical Highlights)
The player provides **two independent visual feedback mechanisms** for audio mode:

- **Bottom waveform (overview & seeking)**:
  - Uses `AudioContext` to decode audio data, computes peaks (`computePeaks`) and renders them as a bar-style waveform.
  - **Interactive**: Click anywhere on the waveform to jump directly to the corresponding playback position (`video.currentTime`).
  - During playback, the waveform highlights the played portion based on the current progress (gray turns black).

- **Center-stage spectrum (real-time dynamics)**:
  - When audio plays, the stage hides the video element and shows a `canvas`.
  - Uses **`AnalyserNode`** to fetch frequency-domain data (FFT) in real time and renders it as a dynamic bar spectrum.
  - When audio pauses or ends, the spectrum automatically falls back to a static, low-amplitude decorative waveform, keeping the interface from ever looking empty.

### 4. Key Technical Architecture Details
- **Web Audio API lazy initialization**: The `ensureAudioGraph` method creates the `AudioContext` only on the first audio playback, circumventing the browser's autoplay policy restrictions, and `MediaElementSource` is connected only once to prevent errors.
- **High-DPI rendering**: All Canvas drawing is multiplied by `devicePixelRatio` to ensure crisp display on Retina screens.
- **Event-driven design**: Strictly listens to `play`/`pause`/`ended`/`timeupdate` events to achieve precise linkage of UI states (button icons, spectrum toggle, waveform progress).

### 5. Visual & Experience Style
- **Minimalism**: Light background, thin borders, large rounded corners, stripped of redundant decoration.
- **Interaction feedback**: Hover color changes, click animations, drag highlight borders — smooth and natural operation feel.
![project-screenshot](player.png)
---

### License

[MIT](LICENSE)
