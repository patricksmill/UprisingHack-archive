# Building from source

The port is three repositories. This one holds the analysis, overrides and
tooling. The runtime and the recompiler are forks that live as git
submodules under `tools/`:

| Path | Repository | Branch |
| --- | --- | --- |
| `tools/zakuro` | [patricksmill/zakuro](https://github.com/patricksmill/zakuro) | `kiu-mouse` |
| `tools/3dsrecomp` | [patricksmill/3dsrecomp](https://github.com/patricksmill/3dsrecomp) | `kiu` |

Zakuro finds the recompiler at `../3dsrecomp`, so the two have to stay side
by side under `tools/`. The submodules do that for you.

## 1. Install the prerequisites

All platforms need [git](https://git-scm.com/) and Rust 1.95 or newer from
[rustup](https://rustup.rs/).

**Windows:** rustup offers to install the Visual Studio C++ build tools.
Accept, and nothing else is needed.

**Linux (Debian, Ubuntu, Mint):**

```sh
sudo apt install build-essential cmake pkg-config libasound2-dev libudev-dev
```

On Arch: `base-devel cmake pkgconf alsa-lib systemd-libs`.

**macOS:** `xcode-select --install`. For Vulkan, also
`brew install molten-vk vulkan-loader`. Without them it presents with
OpenGL and draws on the CPU. macOS is untested for this port.

## 2. Clone with submodules

```sh
git clone --recursive https://github.com/patricksmill/UprisingHack
cd UprisingHack
```

If you already cloned without `--recursive`:

```sh
git submodule update --init
```

## 3. Build and run

```sh
cargo run --release --manifest-path tools/zakuro/Cargo.toml -p zakuro -- path/to/kiu.cci
```

The first build takes several minutes. The executable ends up in
`tools/zakuro/target/release/` (`zakuro.exe` on Windows).

You need your own **decrypted** dump of Kid Icarus: Uprising (USA,
`0004000000030100`), as `.cci` or `.3ds`. Without a path, Zakuro opens its
library, where you pick the folder holding your games.

## 4. Recompile the game

In the library, press **Recompile** next to the game. It takes around ten
minutes and needs a C compiler. Zakuro uses gcc or clang when it finds one,
and otherwise offers to download Zig into its own folder. Recompile again
after each update to the port. Until then the game runs in the interpreter,
well below full speed.

## 5. Optional: the 60 FPS patch

The patch is generated from your own `code.bin`, after the script checks
that the words it changes are the original ones:

```sh
python3 scripts/extract_cci.py path/to/kiu.cci build/kiu
python3 scripts/make_60fps_ips.py build/kiu/code.bin build/code.ips
```

Copy `build/code.ips` into the game's mod folder,
`mods/0004000000030100/`, inside Zakuro's data folder
(`%APPDATA%\zakuro\` on Windows, `~/.local/share/zakuro/` on Linux). The
**Mods** button next to the game in the library opens it. Then recompile
the game, so the patched code runs at full speed.

## Updating

```sh
git pull
git submodule update --init
```

Then rebuild, and press **Recompile** again.

## Working on the forks

Each submodule is a full checkout of its fork, pinned to one commit. To
change one:

1. In `tools/zakuro` (or `tools/3dsrecomp`), check out the branch
   (`git switch kiu-mouse`), commit, and push to the fork.
2. In this repository, commit the new pointer: `git add tools/zakuro`.

Push the fork before this repository, or the pinned commit won't exist for
anyone else.
