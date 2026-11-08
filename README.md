# Prism — ESP32 Wireless Oscilloscope

Dark liquid-glass four-channel oscilloscope UI. Buildless HTML/CSS/JavaScript, responsive on phones, tablets and desktops.

## Preview

Open `index.html` or serve this folder with `python3 -m http.server 8080`.

The initial view uses **simulated data**, not physical measurements. Supports pause/resume, channel visibility, per-channel voltage scales, timebase, overlay/split display, CSV export and fullscreen where supported.

## ESP32 integration

No firmware is included yet. Send WebSocket text frames in the following format (volts):

```json
{"sampleRate":20000,"channels":[[0,0.2,0.4],[0,0.1,0.2],[0,0.3,0.6],[0,0.1,0.1]]}
```

`sampleRate` is the rate **per channel**. Exactly four equal-length arrays with 2–32768 finite samples. Each message replaces the visible frame. Live frequency is estimated from zero crossings and needs multiple periods; demo frequencies are known generator settings. Trigger controls currently synchronize the demo only; hardware acquisition controls require a firmware protocol. Voltage scales and timebase affect local display only. No claimed hardware sample rate or bandwidth.

## GitHub Pages and direct Wi-Fi

Enable Pages with GitHub Actions and push to `main`; the included workflow deploys the static UI. GitHub Pages uses HTTPS and browsers may block connections to local `ws://` endpoints. For reliable offline direct Wi-Fi, serve these same three frontend files from the ESP32 access point over HTTP and use `ws://192.168.4.1/ws`. Loading a Pages URL requires internet access; an isolated ESP32 AP does not supply that. Alternatively use an accessible WSS endpoint with a trusted certificate.

The frontend needs appropriate ESP32 firmware and a protected analog input circuit before measuring actual signals. Fonts use Google Fonts with a local sans-serif fallback.
