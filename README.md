<div align="center">

<!-- LOGO PLACEHOLDER: replace with docs/images/logo.png (recommended ~640px wide, transparent PNG) -->
<img src="docs/images/logo.png" alt="Kid Icarus: Uprising PC logo" width="480">

# Kid Icarus: Uprising — PC

**A native-feeling PC port of Kid Icarus: Uprising, with real mouse aim and a single-screen layout.**

Statically recompiled with [3dsrecomp](tools/3dsrecomp) · runs on a fork of [Zakuro](tools/zakuro)

![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux-informational)
![Title](https://img.shields.io/badge/title-0004000000030100%20(USA)-blue)
![Status](https://img.shields.io/badge/status-work%20in%20progress-orange)

<!-- HERO SCREENSHOT PLACEHOLDER: docs/images/hero.png (16:9 air battle, 1920x1080) -->
<img src="docs/images/hero.png" alt="Air battle with mouse aim" width="900">

</div>

---

## Features

### 🎯 Controls
- **True mouse aiming** — absolute reticle in air battles, relative mouse-look camera on the ground.
- **Fully rebindable** keyboard, mouse and controller input.
- **Optional gyro aim** for supported controllers.
- **Mouse-driven menus** — hover and click call the game's own cursor and confirm logic, so menus behave exactly as they would on hardware.

### 🖥️ One Screen
- **Battle HUD, dialogue, pause menu and scripted menus** relocated from the bottom screen onto the main view.
- **Widescreen** — 16:9 is recommended, since menu layouts are hand-placed for it; ultrawide is supported in battle.
- Every relocated Screen can be switched back to its original 3DS presentation individually.

### ⚡ Performance
- **Ahead-of-time recompiled** game code — no JIT warm-up, no interpreter.
- **60 FPS patch**, generated from your own `code.bin` by `scripts/make_60fps_ips.py` ([how](docs/BUILDING.md#5-optional-the-60-fps-patch)).
- Headless benchmark harness to track frame rate across upstream merges.

<!-- SCREENSHOT GRID PLACEHOLDERS: replace each with docs/images/<name>.png -->
<table>
  <tr>
    <td><img src="docs/images/ground-battle.png" alt="Ground battle" width="440"></td>
    <td><img src="docs/images/menus.png" alt="Single-screen menus" width="440"></td>
  </tr>
  <tr>
    <td align="center"><sub>Ground battle with mouse-look</sub></td>
    <td align="center"><sub>Menus on one screen</sub></td>
  </tr>
  <tr>
    <td><img src="docs/images/ultrawide.png" alt="Ultrawide" width="440"></td>
    <td><img src="docs/images/settings.png" alt="Input settings" width="440"></td>
  </tr>
  <tr>
    <td align="center"><sub>Ultrawide in battle</sub></td>
    <td align="center"><sub>Rebinding & PC settings</sub></td>
  </tr>
</table>

---

## How It Works

| Layer | Role |
| --- | --- |
| **3dsrecomp** | Translates the game's ARM `code.bin` into native code ahead of time. |
| **Overrides** | Small C replacements for selected game functions (`overrides/`, tracked in `docs/decomp/LEDGER.md`). |
| **Zakuro fork** | Runtime: GPU, audio, services, plus the PC frontend, input and screen-layout hooks. |
| **kiutrace** | Headless harness for save states, scripted input, screenshots and benchmarks. |

Only a handful of game functions are hooked; everything else runs as the original game logic.

---

## Getting Started

> [!IMPORTANT]
> You must provide your own **decrypted** dump of Kid Icarus: Uprising (USA). No game data is included or distributed.

1. Install Rust 1.95+ and, on Linux, the packages listed in [Building](docs/BUILDING.md#1-install-the-prerequisites).
2. Clone with submodules (the Zakuro and 3dsrecomp forks live in `tools/`):
   ```sh
   git clone --recursive https://github.com/patricksmill/UprisingHack
   cd UprisingHack
   ```
3. Build and launch:
   ```sh
   cargo run --release --manifest-path tools/zakuro/Cargo.toml -p zakuro -- path/to/kiu.cci
   ```
4. Press **Recompile** in the library. It takes about ten minutes and needs a C compiler (Zakuro offers to download one). Recompile once, and again after each update.
5. Press **F12** in-game for the emulator menu.

Full instructions, including the 60 FPS patch and updating: [docs/BUILDING.md](docs/BUILDING.md).

### Restoring original presentation

Set any of these to `0` to show that feature as the 3DS draws it:

| Variable | Feature |
| --- | --- |
| `ZAKURO_MENUS_TOP` | Scripted menus on the top screen |
| `ZAKURO_NARRATION_TOP` | Dialogue / narration |
| `ZAKURO_PAUSE_TOP` | Pause menu |
| `ZAKURO_CAULDRON_TOP` | Fiend's Cauldron |
| `ZAKURO_MIRROR_BOTTOM` | Bottom-screen mirroring |
| `ZAKURO_MENU_HOVER` | Mouse hover in menus |

---

## Repository Layout

| Path | Contents |
| --- | --- |
| `tools/zakuro` | Zakuro fork (submodule): hooks and frontend |
| `tools/3dsrecomp` | 3dsrecomp fork (submodule): recompiler |
| `tools/kiutrace` | Headless test & benchmark harness |
| `tools/funclist` | Function list export for Ghidra |
| `ghidra/` | Headless analysis scripts |
| `overrides/` | C overrides |
| `scripts/` | Benchmarks, IPS builder, image conversion |
| `analysis/` | `FINDINGS.md` (addresses), `PERFORMANCE.md` |
| `docs/BUILDING.md` | Building from source |
| `docs/adr/` | Design decisions |
| `GLOSSARY.md` | Project terminology |

Roadmap and open issues: [`PLAN.md`](PLAN.md).

---

<div align="center">
<sub>Not affiliated with or endorsed by Nintendo, Project Sora or Sora Ltd. Kid Icarus is a trademark of Nintendo.</sub>
</div>
