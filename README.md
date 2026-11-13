# WebAudio-RTA

A real-time audio analyzer that runs in your web browser. Free, no install, one HTML file.

It's built for live sound: checking a PA, finding feedback, keeping an eye on SPL during a show. It also handles installation and system tuning work with a dual-channel transfer function. Think of it as a lightweight, browser-based alternative to tools like Smaart.

All audio is analyzed on your device. Nothing is recorded or uploaded.

---

## Features

### RTA mode
- Fractional-octave spectrum: 1/1, 1/3, 1/6, 1/12, 1/24 or 1/48 octave
- Z, A or C frequency weighting
- Averaging from 125 ms to 8 s, or infinite
- Peak hold: 1 s, 3 s, 10 s or hold
- Bars or line trace, plus a scrolling spectrograph underneath
- **Freeze** and **Capture** a trace to compare against the live one
- **Markers**: click the plot to pin up to 4 bands with their readings
- **Target curve overlay**: flat, -1.5, -3 or -4.5 dB/oct, auto-levelled to your signal between 100 Hz and 4 kHz
- **Feedback finder**: flags steady peaks ringing above their neighbours and suggests where to notch

### Transfer mode (dual channel)
- Frequency response magnitude and phase (H1 estimator, multi-time-window)
- Coherence trace, with optional blanking of low-coherence data
- Reference source: input 2 (loopback from your console), input 1, or the internal generator
- Delay finder (one shot or tracking), with ±0.1 ms fine adjustment
- Adjustable smoothing

### Meters
- LZF / LZS (Fast / Slow), LZeq, LZpk, peak frequency and input peak in dBFS with a clip lamp
  (the "Z" changes to A or C when you change the weighting)
- **SPL limit alarms**: separate Slow and Fast limits. Each tile turns green when safe, yellow within 3 dB, red at or over
- **VU meter** (dBFS RMS and peak with hold):
  - Position: top, above the plot, bottom, or vertically left / right of the plot
  - LED segments or a continuous bar, adjustable length and thickness
  - Colours: stepped zones, smooth fade, or a single colour, with custom colours and thresholds
  - Optional glow, tinted unlit LEDs, and a peak flash mode

### Signal generator
- Pink noise, white noise, or sine (20 Hz – 20 kHz), from -70 to 0 dBFS

### Calibration
- **SPL calibration** with a 94 dB (or any level) 1 kHz calibrator, or type the offset in by hand
- **Mic correction file**: a text file of frequency / dB pairs (`.txt`, `.csv`, `.cal`). Applies to the spectrum, not the SPL meters

### Other
- Export the graph as **PNG** or the curve as **CSV**
- **Settings file**: export every setting, including the mic correction curve, to JSON and import it on another PC
- Dark and light themes, fullscreen, keep-screen-on (where the browser supports it)
- Hover tips on every control, which you can switch off
- Built-in **demo signal**, so you can try everything without a mic

---

## Getting started

### Option 1: open it locally
1. Download or clone this repo.
2. Serve the folder with any local web server, for example:
   ```bash
   cd WebAudio-RTA
   python -m http.server 8000
   ```
3. Open `http://localhost:8000` in Chrome or Edge.

> Browsers only allow microphone access on **HTTPS** or **localhost**. If you just double-click `index.html`, the mic may not work, but the demo signal will.

### Option 2: host it
Put `index.html` and the `img/` folder on any HTTPS static host (GitHub Pages, Netlify, Cloudflare Pages, etc.).

### First run
1. Click **Start microphone** and allow access, or try the **demo signal**.
2. In the **Input** tab, pick your audio interface and the channel your measurement mic is on.
3. For real dB SPL readings, go to **Calibrate**, put a calibrator on the mic, and press **Calibrate**.

---

## Measurement setup

**RTA only:** a measurement mic into input 1 (or 2) is all you need.

**Transfer function:** you need two inputs on the same interface.
- **Measurement:** measurement mic
- **Reference:** a loopback of the signal you send to the system (e.g. a console matrix/aux output into input 2)

Then switch to **Transfer**, press **Find delay** (`D`) to line up the two signals, and read the magnitude, phase and coherence.

Tips:
- Turn off any "enhancements", noise suppression or auto gain on the interface in your OS. The app already asks the browser to switch these off.
- The feedback finder works best at 1/12 oct or finer with 0.5 – 2 s averaging.

---

## Keyboard & mouse

| Key | Action |
|---|---|
| `Space` | Freeze / unfreeze |
| `C` | Capture trace |
| `R` | Reset meters and averages |
| `T` | Toggle RTA / Transfer |
| `D` | Find delay (Transfer mode) |
| `P` | Clear markers |
| `F` | Fullscreen |
| `L` | Toggle light / dark theme |

| Mouse | Action |
|---|---|
| Wheel on plot | Move the top of the scale |
| Shift + wheel | Change the dB range |
| Click on plot | Pin / unpin a band marker |
| Shift + click | Clear all markers |
| Drag the dividers | Resize the side panel, spectrograph, or phase / magnitude panes |

---

## Browser support

Best in recent **Chrome** or **Edge** on desktop. It needs Web Audio with AudioWorklet support. Other modern browsers should work but are less tested.

Known limits:
- Only the first two input channels of an interface are used (measurement + reference).
- Accuracy depends on your interface and mic. For SPL work, use a proper measurement mic and calibrate it.

---

## Settings

Settings are saved in your browser automatically. To move them to another computer, use **Tools → Settings file → Export**, then **Import** on the other machine.

`Setting.json` in this repo is an example export.

---

## License

[GNU GPL v3](LICENSE)
