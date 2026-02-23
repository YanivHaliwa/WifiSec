# WiFiSec

Yes, there are plenty of WiFi auditing tools out there — wifite, fluxion, airgeddon and others. Most of them are either bloated, slow to set up, or built around a single attack method. WiFiSec was built differently.

**What makes it stand out:**

- **All attacks, one tool** — other tools make you pick an attack method. WiFiSec assembles them all together: PMKID passive scan, mdk4 deauth, aireplay-ng injection, cowpatty verification, aircrack-ng validation — running in the right order automatically, no switching tools
- **Dead simple** — one command, auto-detects your interface, scans, selects targets, captures. No config files, no setup wizards
- **Multi-target capture** — one of the very few tools that lets you run deauth and capture against multiple targets simultaneously in a single session
- **Dual attack path** — tries PMKID passive capture first (no deauth, completely silent), falls back to active deauth automatically
- **Handshake triple-verification** — validates every capture with up to 3 tools before calling it done, so you never waste cracking time on a bad cap
- **Integrated cracking** — `crackcap` picks up right where the capture ends, handles conversion, session management and result reporting in one step
- **Live dashboard** — real-time status across all targets while capturing, not a wall of raw airodump output
- **Clean and fast** — no Python dependency hell, no GUI, no bloat. Runs lean on any Kali setup

## Demo

<!-- VIDEO_PLACEHOLDER -->
[![WiFiSec Demo](https://img.youtube.com/vi/VIDEO_ID/maxresdefault.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)
<!-- Replace VIDEO_ID with your YouTube video ID -->

---

> **Access required** — tools verify your license on every run via a cloud vault. You need a valid access code before starting.

---

## > **This project is closed-source and distributed by license only.**

Unlike most of my other open-source tools and projects, WiFiSec is **not publicly available as source code**. This is a deliberate decision — the level of automation this tool provides carries real misuse risk, and significant time and effort went into building and hardening it.

**If you're interested in access, contact:** [yaniv@yanivhaliwa.com](mailto:yaniv@yanivhaliwa.com)

---

## Files You Receive

| File | Purpose |
|---|---|
| `WiFiSec.bin` | Main capture tool (compiled binary) |
| `crackcap.bin` | Post-capture cracking wrapper (compiled binary) |
| `hashc` | Hashcat helper script |
| `setup` | Automated installation script |
| `register.bin` | Access registration binary |

## Features

- Combines all major attack methods in one flow — PMKID, mdk4, aireplay-ng, cowpatty, aircrack-ng — no need to switch tools
- Auto-detects wireless interface and enables monitor mode
- Scans nearby WPA/WPA2 APs — select one or multiple targets
- PMKID passive pre-scan (no deauth) when `hcxdumptool` + `hcxpcapngtool` are available
- Multi-target deauth capture with live status dashboard
- Handshake verified by up to 3 tools (hcxpcapngtool → cowpatty → aircrack-ng)
- Deauth capture runs for up to 30 minutes then exits cleanly — protects your adapter and keeps sessions manageable
- Optionally launches `crackcap` immediately after a successful capture

## Installation

### Automated (Recommended)

`setup` does everything in one step:
- Installs all required apt packages
- Copies tools to `/usr/local/bin/`
- Runs `register.bin --setup` to activate your access

```bash
chmod +x setup
sudo ./setup
```

### Manual

**Step 1 — Install dependencies:**

```bash
sudo apt update
sudo apt install -y aircrack-ng hcxdumptool hcxtools mdk4 cowpatty hashcat wireless-tools iw network-manager python3
```

**Step 2 — Copy tools to PATH:**

```bash
sudo install -m 755 WiFiSec.bin /usr/local/bin/WiFiSec
sudo install -m 755 crackcap.bin /usr/local/bin/crackcap
sudo install -m 755 hashc /usr/local/bin/hashc
```

**Step 3 — Register your access:**

```bash
chmod +x register.bin
./register.bin --setup
```

Enter your full name and access code when prompted. Credentials are saved to `~/.wifisec_auth/` and verified on every run.

## Quick Start

```bash
sudo WiFiSec              # auto-detect interface
sudo WiFiSec wlan0        # specify interface
sudo WiFiSec --help
```


## Output & Logs

Captures are saved to the current working directory:

- `capture_<essid>_<timestamp>.cap` — WPA handshake capture
- `pmkid_<essid>_<timestamp>.hash` — PMKID capture (hashcat-ready)

Session logs:

- `~/temp/wifi_hack.log` — full session log
- `~/temp/wifi_hack_success.log` — successful captures history
- `~/temp/crackcap_sessions.log` — crackcap cracking session history

## Requirements

- Linux with a wireless adapter that supports monitor mode and injection
- Root privileges for capture operations
- Required: `airmon-ng`, `airodump-ng`, `aireplay-ng`, `aircrack-ng`, `hcxdumptool`, `hcxpcapngtool`, `mdk4`, `cowpatty`, `hashcat`

## Legal Notice

Use this project only on networks you own or are explicitly authorized to test.

Created by [Yaniv Haliwa](https://github.com/YanivHaliwa) for security testing.
