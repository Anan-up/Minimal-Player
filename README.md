[English](README.md) | [简体中文](README_Simplified_Chinese.md) | [繁體中文](README_Classical_Chinese.md)

This is a fully-featured, minimalist **local media player** built as a single HTML application that runs entirely in the browser — no backend required.

---

## 1. Overall Architecture & Tech Stack
- **Pure frontend**: A single-page HTML file with inline CSS and JavaScript, with no external dependencies.
- **Media handling**: Uses the `<video>` element and the Web Audio API for playback, real-time audio analysis, and gain control.
- **Rendering & animation**: Uses Canvas to draw the waveform and bar spectrum, driven by `requestAnimationFrame` for real-time visualization.
- **Responsive design**: Mobile-friendly, using `aspect-ratio` and flexible layouts.

---

## 2. Core Features
- **Local file loading**
  - Supports clicking "Choose File" or dragging a file onto the playback area.
  - Automatically recognizes video (MP4, WebM, MOV) and audio (MP3, WAV, FLAC, M4A).
  - Files are processed locally only and never uploaded to a server.
- **Playback control**
  - Play / pause (click the center area or the button).
  - Progress display (current time / total duration).
  - Seek: click anywhere on the waveform to jump to the corresponding time point.
- **Volume management**
  - Slider to adjust volume (0~1).
  - One-tap mute; when muted, the volume slider and enhancement controls are automatically hidden (keeping the UI clean).
- **Playback speed**
  - Preset speed levels: 0.5×, 0.75×, 1.0×, 1.25×, 1.5×, 2.0×, 3.0×.
  - Click the button to open a selection menu; clicking outside auto-closes it.
- **Fullscreen mode**
  - Supports fullscreen video playback (stage area goes fullscreen); in fullscreen, rounded corners are hidden and the aspect ratio is adjusted.

---

## 3. User Interaction Experience
- **Visual feedback**
  - A dashed border is shown when dragging.
  - The play button has a scaling animation on click (`pop` keyframe).
  - State toggles (play/pause, enhancement switch, limiter switch) are distinguished via button style changes (`on` class) and icons.
- **Control grouping & collapse**
  - The "Volume Boost" panel is collapsed by default; clicking the boost switch expands it, revealing a multiplier slider (1~10×) and a limiter switch.
  - The panel has a smooth `max-height` transition animation when expanding/collapsing.
- **Accessibility**: Semantic tags are used, and input controls have `title` tooltips.

---

## 4. Visualization Components
### (1) Audio waveform (`#wave` Canvas)
- **Computation**: After loading a file, the audio is decoded via `AudioContext.decodeAudioData`; the left-channel peak data is extracted (160 sample points), normalized, and stored as `peaks`.
- **Drawing**: Rounded rectangles are used; the played portion (relative to current playback position) is filled dark, while the unplayed portion is light gray, giving an intuitive view of playback position.
- **Interaction**: Clicking the waveform seeks to the corresponding time point.

### (2) Real-time spectrum (`#spectrum` Canvas)
- **Trigger**: Active only when an **audio file** is playing (video files show only the video image, no spectrum).
- **Data source**: Frequency-domain data obtained via `AnalyserNode` (FFT size = 256, 128 frequency bins total).
- **Drawing logic**:
  - A smoothing algorithm (fast rise, slow fall) suppresses jitter, producing a stable bar chart.
  - Updates in real time during playback; falls back to a static low-amplitude simulated waveform when paused or finished.
- **Performance**: Driven by `requestAnimationFrame`; the loop is cancelled when idle to save resources.

---

## 5. Audio Processing (Gain & Limiter)
To support volume boost (up to 10×) and prevent clipping distortion, a complete audio processing chain is built:
```
MediaElementSource → GainNode → DynamicsCompressor (limiter) → AnalyserNode → AudioContext.destination
```
- **Gain node** (`gainNode`): Dynamically adjusts gain based on the `boostOn` state and the `boost` slider value (gain is 1 by default when off).
- **Limiter** (`DynamicsCompressor`): Threshold -6 dB, ratio 20:1, knee 0, attack/release 3 ms / 250 ms, effectively preventing overload of large signals.
- **Bypass support**: The limiter can be toggled; when off, the gain node connects directly to the analyser, suiting different user preferences.
- **Application timing**: The audio context is lazily initialized on first playback (`ensureAudioGraph`), ensuring compatibility and avoiding wasted resources.

---

## 6. Other Details
- **Error tolerance**: When audio decoding fails (e.g., certain video containers), the waveform is not shown, but playback is unaffected.
- **Adaptive redraw**: Canvas size auto-adjusts on window resize (accounting for `devicePixelRatio` to ensure clarity).
- **Fullscreen adaptation**: Rounded corners and borders are removed in fullscreen, with the video filling the stage.
- **File info display**: Shows the file name, type (video/audio), and format extension.

---
## Project Screenshots

![Project Screenshots](player.png)
---
### License

[MIT](LICENSE)
