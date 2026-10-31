# SoundShape – Turn Music Into Visual Art

> **Feel the Sound. See the Music.**

SoundShape is a browser-based music visualizer. Load your own tracks and watch real-time visuals react to the actual audio: bass, mids and treble drive the bars, rings, waves and particles. Everything runs locally in your browser. There is no backend and no account, and your music is never uploaded.

## Features

- **Local audio playback:** upload MP3, WAV, OGG, M4A and other browser-supported formats, by file picker or drag and drop.
- **Playlist and library:** multiple tracks, previous/next, track durations, remove tracks, current track highlighted.
- **Player controls:** play/pause, seek bar, current time and duration, volume, mute, file info (name, size, type).
- **Four visualization modes**, switchable without interrupting playback:
  - **Spectrum Bars:** configurable bar count and spacing, neon glow.
  - **Circular Pulse:** mirrored circular spectrum, bass-reactive ring, orbiting particles.
  - **Waveform:** smooth lines with adjustable thickness and 1–5 layers.
  - **Galaxy Particles:** spiral particle field with trails; bass hits trigger expansion.
- **Customization studio:** primary, secondary and background colors; five presets (Neon Purple, Electric Blue, Cyberpunk Pink, Cosmic Gold, Emerald Green); sliders for particle density, animation speed, sensitivity, glow, size, bar count, spacing, line thickness and wave layers; reset button.
- **Export:** save the canvas as PNG at its current size, record the animation as WebM (**video only, no audio**), and fullscreen mode (button or double-click the canvas).
- **Demo beat:** generates an original synthesized track in the browser, so you can try the app without your own music.
- **Responsive:** desktop, tablet and mobile, with a bottom drawer for controls on small screens, touch interaction on the canvas, and dark/light UI themes.
- **Persistence:** your settings are saved in LocalStorage.

## Tech stack

- Vanilla JavaScript (ES6+), HTML5 Canvas, CSS3
- Web Audio API (`AudioContext`, `MediaElementAudioSourceNode`, `AnalyserNode`, `getByteFrequencyData`, `getByteTimeDomainData`)
- `MediaRecorder` and `canvas.captureStream()` for WebM recording
- No build step, no dependencies, no backend

The entire app is a single file: `soundshape.html`.

## Getting started

### Run locally

Open the file directly in a modern browser:

```bash
open soundshape.html        # macOS
xdg-open soundshape.html    # Linux
start soundshape.html       # Windows
```

Or serve it with any static server:

```bash
npx serve .
# or
python3 -m http.server 8000
```

Then visit the URL it prints (for example `http://localhost:8000/soundshape.html`).

### Production build

There is nothing to build. The file is already production-ready.

### Deploy to Vercel

1. Create a folder and put the file in it, renamed to `index.html`:
   ```
   soundshape/
   └── index.html
   ```
2. Deploy:
   ```bash
   npm i -g vercel
   cd soundshape
   vercel --prod
   ```
   Alternatively, push the folder to GitHub and import the repo in the Vercel dashboard. Choose the **Other** framework preset and leave the build command empty.

Any static host (Netlify, GitHub Pages, Cloudflare Pages, S3) works the same way.

## How it works

1. A single `Audio` element plays the selected track. Files are loaded with `URL.createObjectURL`, and the URLs are revoked when tracks are removed or the page is closed.
2. The `AudioContext` is created or resumed on the first user interaction to satisfy browser autoplay rules. The audio is routed through one `AnalyserNode` and out to the speakers.
3. Each animation frame reads frequency and waveform data from the analyser, then computes smoothed bass, mid and treble levels (scaled by the Sensitivity slider).
4. The active visualizer draws to the canvas using those values. A single `requestAnimationFrame` loop drives everything, and canvases that are off-screen are skipped.
5. The hero section animates with synthetic data purely as decoration. The studio canvas only ever reacts to the audio you play. When nothing is playing, it settles to idle.

## Privacy and copyright

- Your audio files stay in your browser's memory. Nothing is uploaded or sent to any server.
- The library is not persisted: after a refresh you need to add your tracks again. Only your settings are saved.
- SoundShape ships with **no commercial music**. Only upload tracks you own or have the right to use. The built-in demo beat is original and generated in code.
- No third-party assets, fonts or libraries are bundled, so there are no extra licenses to track.

## Browser support

Current versions of Chrome, Edge, Firefox and Safari (desktop and mobile). Notes:

- Supported audio formats depend on your browser (for example, FLAC and OGG support varies).
- WebM recording requires `MediaRecorder` and `captureStream` support. Safari support is limited.
- Fullscreen on iPhone may be unavailable because of platform restrictions.

## Known limitations

- Recorded WebM files contain the visuals only, without audio.
- The track library is session-only by design.
- Very high particle density settings can reduce frame rate on low-end devices.

## Customizing

- **Presets:** edit the `PRE` object in the script.
- **Defaults:** edit the `DEF` object.
- **New visualizer:** add a function to the `V` object and an entry to `MODES` and `FADE`. It receives `(renderer, freqData, timeData, bands, time, deltaTime)`.

## License

Add your preferred license here (for example MIT) before publishing.
