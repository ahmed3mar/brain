# Brain

Brain is a collaborative coding workspace with a command-line interface, terminal UI, and desktop application.

The public repository hosts downloads and release automation. Application source is maintained separately.

## Install the CLI and TUI

### Homebrew

```bash
brew install ahmed3mar/tap/brain
```

This installs:

- `brain` — the main CLI and interactive terminal workspace
- `brain-tui` — the standalone terminal UI

### Release archives

Download the archive for your platform from the [latest release](https://github.com/ahmed3mar/brain/releases/latest):

- macOS Apple Silicon (`macos-arm64`)
- macOS Intel (`macos-x64`)
- Linux ARM64 (`linux-arm64`)
- Linux x86-64 (`linux-x64`)

Each `brain-<version>-<platform>-<arch>.tar.gz` archive contains both `brain` and `brain-tui`.

## Install the desktop application

Desktop downloads are published separately as `brain-desktop-<version>-<platform>-<arch>.tar.gz`.

### macOS

The archive contains `Brain.app`. Move it to `/Applications` and open it from Finder or Spotlight.

The initial application bundles are unsigned. macOS may require you to right-click **Brain**, choose **Open**, and confirm the first launch.

### Linux

The archive contains `brain-desktop` and a `brain.desktop` launcher template. Place the executable somewhere on your `PATH`; the launcher can then be installed into `~/.local/share/applications` if desired.

## Platform status

Current release builds support macOS and Linux on ARM64 and x86-64.

Windows artifacts are not published yet. Brain currently relies on Unix sockets, Unix process signaling, and PTY behavior in its CLI/runtime. Windows will be added after those components have native Windows implementations and are covered by CI; publishing an untested `.exe` would imply support that the current code does not provide.

## Checksums

Every release includes `checksums.txt` with SHA-256 checksums for all archives.
