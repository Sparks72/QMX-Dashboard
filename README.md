# QMX Fusion · Health Dashboard V27

> A browser-based live monitoring and control panel for the **QRP Labs QMX** (and QDX) transceiver,
> connecting directly to the radio via the **Web Serial API** — no drivers, no software to install.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-brightgreen?style=for-the-badge)](https://sparks72.github.io/QMX-Dashboard/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![Web Serial API](https://img.shields.io/badge/Requires-Chrome%20%2F%20Edge-orange?style=for-the-badge)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API)

---

## ✨ Features

| Panel | What it shows / does |
|---|---|
| **VFO Frequency** | Live operating frequency with band highlighting |
| **Band buttons** | One-click QSY to CW spots on 160 m – 10 m |
| **Tuning knob** | Drag to tune; velocity-sensitive (slow = 1 Hz fine, fast = kHz coarse); scroll or ↑↓ keys step by the selected increment |
| **Mode chip** | Click to cycle CW → FSK → CW-R → FSK-R |
| **VFO A / B** | Click to swap the active VFO; B frequency shown live |
| **RIT** | Click to toggle; offset displayed when on |
| **Split** | Click to toggle split/simplex |
| **TX key** | Click to key / unkey the transmitter |
| **SWR gauge** | Semicircular analogue gauge + 60-point sparkline |
| **RF Power gauge** | Semicircular analogue gauge + 60-point sparkline |
| **S-meter / AGC** | Bar graph S-meter with dBFS and AGC attenuation readout |
| **AF Gain** | Interactive rotary knob with drag, scroll, and keyboard control |
| **RF Gain** | Interactive rotary knob with drag, scroll, and keyboard control |
| **Keyer Speed** | Interactive rotary knob (5 – 60 WPM) |
| **Audio Monitor** | Routes QMX USB audio to speakers via Web Audio API; software boost up to 8×; VU meter |
| **UTC Clock** | Synced from the QMX firmware via CAT; falls back to system UTC |
| **Radio Status** | TX/RX, Mode, Split, RIT, Filter BW, Firmware version |
| **Dark / Light theme** | Toggle at any time; preference saved across sessions |
| **Poll rate** | Selectable 500 ms / 1 s / 2 s / 5 s |

---

## 🚀 Live Demo

**[https://sparks72.github.io/QMX-Dashboard/](https://sparks72.github.io/QMX-Dashboard/)**

The demo is the real application — connect your QMX over USB and it goes live immediately.

> **Requirements:**
> - Google Chrome or Microsoft Edge (desktop) — Firefox does not support Web Serial API
> - QMX connected via USB at 115 200 baud (default) — 57 600 and 9 600 also selectable
> - HTTPS (GitHub Pages satisfies this automatically)

---

## 📸 Screenshots

| Dark theme | Light theme |
|---|---|
| *(add screenshot)* | *(add screenshot)* |

---

## 🔌 Connecting

1. Open the [Live Demo](https://sparks72.github.io/QMX-Dashboard/) in Chrome or Edge.
2. Plug your QMX into a USB port.
3. Click **Connect Radio** (or the pill in the header) — the browser serial port picker appears.
4. Select the QMX COM / tty port and click **Connect**.
5. All panels update live at the selected poll rate.

---

## 🛠 Running Locally

No build step — it is a single self-contained HTML file:

```bash
git clone https://github.com/Sparks72/QMX-Dashboard.git
cd QMX-Dashboard
# Open index.html in Chrome or Edge
# (file:// also works for Web Serial — no server needed)
```

Or just download `index.html` and open it directly.

---

## 📡 CAT Commands Used

The dashboard polls the QMX using standard Kenwood-compatible CAT:

| Command | Purpose |
|---|---|
| `IF;` | Frequency, mode, RIT, TX status, Split — all in one frame |
| `FA;` / `FB;` | VFO A and VFO B frequencies |
| `SM;` | S-meter (dBFS) |
| `SA;` | AGC attenuation |
| `SW;` | SWR |
| `PC;` | RF power output |
| `AG0;` | AF gain |
| `RG;` | RF gain |
| `KS;` | Keyer speed |
| `FW;` | Filter bandwidth |
| `TM;` | QMX UTC time |
| `VN;` | Firmware version |

Set commands (`FA`, `FB`, `MD`, `FR`, `RT`, `SP`, `TX`, `RX`, `AG0`, `RG`, `KS`) are sent write-only via a separate writer path so they never disturb the read poll.

---

## ⚙️ Architecture Notes

- **Single HTML file** — CSS custom properties for theming, vanilla JS, no framework or bundler.
- **Web Serial API** — 115 200 baud, 8N1, no flow control.
- **Poll / write separation** — CAT reads use a response-resolving reader loop; CAT writes (tuning, control chips) use a fire-and-forget writer that never contends with the read path.
- **Tune lock** — during a drag or for 700 ms after a keyboard/wheel tune, the poll read-back is held off so fine (1 Hz) increments are not stomped by the rig's 10 Hz CAT resolution.
- **Velocity-sensitive tuning** — angular speed on the drag knob maps through a power curve (exp 2.6) to Hz/degree, giving a wide fine-tuning zone and coarse travel on fast spins.

---

## 🪪 Licence

MIT — see [LICENSE](LICENSE).  
Made with ☕ and 73s by **Paul Harrison · G4ADF / DJ0CU** from Fehmarn Island, Germany.
