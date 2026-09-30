# Brain

Brain is a collaborative coding workspace with a command-line interface and desktop application. The terminal UI is built into the `brain` command.

The public repository hosts downloads and release automation. Application source is maintained separately.

## Supported platforms

| Platform | Architectures | CLI | Desktop |
| --- | --- | --- | --- |
| macOS | Apple Silicon, Intel | Yes | Yes |
| Linux | ARM64, x86-64 | Yes | Yes |
| Windows | — | Not yet | Not yet |

## Install the CLI

### Installer for macOS and Linux

The installer detects macOS or Linux and ARM64 or x86-64, verifies the release checksum, and installs `brain` into `~/.local/bin`:

```bash
curl -fsSL https://raw.githubusercontent.com/ahmed3mar/brain/main/install.sh | sh
```

To inspect the script before running it:

```bash
curl -fsSL https://raw.githubusercontent.com/ahmed3mar/brain/main/install.sh -o install.sh
less install.sh
sh install.sh
```

Install a specific version:

```bash
curl -fsSL https://raw.githubusercontent.com/ahmed3mar/brain/main/install.sh | BRAIN_VERSION=0.1.19 sh
```

Choose another installation directory:

```bash
curl -fsSL https://raw.githubusercontent.com/ahmed3mar/brain/main/install.sh | BRAIN_INSTALL_DIR=/usr/local/bin sh
```

### Homebrew on macOS or Linux

```bash
brew install ahmed3mar/tap/brain
```

Upgrade an existing installation with:

```bash
brew update
brew upgrade brain
```

### Manual Linux installation with `curl`

The following command detects ARM64 or x86-64, downloads the latest release, and installs `brain` into `~/.local/bin`:

```bash
set -e
case "$(uname -m)" in
  x86_64|amd64) arch="x64" ;;
  aarch64|arm64) arch="arm64" ;;
  *) echo "Unsupported architecture: $(uname -m)" >&2; exit 1 ;;
esac
version="$(curl -fsSL https://api.github.com/repos/ahmed3mar/brain/releases/latest | sed -n 's/.*"tag_name": *"v\([^"]*\)".*/\1/p')"
tmp="$(mktemp -d)"
curl -fL "https://github.com/ahmed3mar/brain/releases/download/v${version}/brain-${version}-linux-${arch}.tar.gz" \
  -o "$tmp/brain.tar.gz"
tar -xzf "$tmp/brain.tar.gz" -C "$tmp"
mkdir -p "$HOME/.local/bin"
install -m 755 "$tmp/brain-${version}-linux-${arch}/brain" "$HOME/.local/bin/brain"
rm -rf "$tmp"
"$HOME/.local/bin/brain" --version
```

Ensure `~/.local/bin` is on your `PATH`. Add this to `~/.bashrc` or `~/.zshrc` if necessary:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

For a system-wide installation, copy the extracted binary to `/usr/local/bin` instead:

```bash
sudo install -m 755 brain /usr/local/bin/brain
```

### macOS without Homebrew

Determine your Mac architecture:

```bash
uname -m
```

- `arm64` means Apple Silicon; download `macos-arm64`.
- `x86_64` means Intel; download `macos-x64`.

Download the matching `brain-<version>-macos-<arch>.tar.gz` from the [latest release](https://github.com/ahmed3mar/brain/releases/latest), then run:

```bash
tar -xzf brain-<version>-macos-<arch>.tar.gz
sudo install -m 755 brain-<version>-macos-<arch>/brain /usr/local/bin/brain
brain --version
```

## Run Brain

```bash
brain
```

The interactive terminal workspace is part of this command; there is no separate `brain-tui` installation.

Verify the installed version with:

```bash
brain --version
```

## Install the desktop application

Desktop downloads are published separately as `brain-desktop-<version>-<platform>-<arch>.tar.gz` on the [releases page](https://github.com/ahmed3mar/brain/releases/latest).

### macOS desktop

1. Download the archive matching `macos-arm64` or `macos-x64`.
2. Extract it:

   ```bash
   tar -xzf brain-desktop-<version>-macos-<arch>.tar.gz
   ```

3. Move the application into `/Applications`:

   ```bash
   mv brain-desktop-<version>-macos-<arch>/Brain.app /Applications/
   ```

4. Open **Brain** from Finder or Spotlight.

The initial application bundles are unsigned. On first launch, macOS may require you to right-click **Brain**, choose **Open**, and confirm.

### Linux desktop

Download the matching desktop archive or install the latest version with:

```bash
set -e
case "$(uname -m)" in
  x86_64|amd64) arch="x64" ;;
  aarch64|arm64) arch="arm64" ;;
  *) echo "Unsupported architecture: $(uname -m)" >&2; exit 1 ;;
esac
version="$(curl -fsSL https://api.github.com/repos/ahmed3mar/brain/releases/latest | sed -n 's/.*"tag_name": *"v\([^"]*\)".*/\1/p')"
tmp="$(mktemp -d)"
curl -fL "https://github.com/ahmed3mar/brain/releases/download/v${version}/brain-desktop-${version}-linux-${arch}.tar.gz" \
  -o "$tmp/brain-desktop.tar.gz"
tar -xzf "$tmp/brain-desktop.tar.gz" -C "$tmp"
dir="$tmp/brain-desktop-${version}-linux-${arch}"
mkdir -p "$HOME/.local/bin" "$HOME/.local/share/applications"
install -m 755 "$dir/brain-desktop" "$HOME/.local/bin/brain-desktop"
sed "s|^Exec=brain-desktop$|Exec=$HOME/.local/bin/brain-desktop|" "$dir/brain.desktop" \
  > "$HOME/.local/share/applications/brain.desktop"
rm -rf "$tmp"
```

Launch it from your desktop application menu or run:

```bash
brain-desktop
```

## Verify downloads

Every release includes `checksums.txt`. Verify an archive before installing it:

```bash
curl -fLO https://github.com/ahmed3mar/brain/releases/download/v<version>/checksums.txt
sha256sum -c checksums.txt --ignore-missing
```

On macOS, use:

```bash
shasum -a 256 <downloaded-archive>
```

and compare the result with the matching entry in `checksums.txt`.

## Uninstall

Homebrew installation:

```bash
brew uninstall brain
```

Manual CLI installation:

```bash
rm -f "$HOME/.local/bin/brain"
# Or, for a system-wide installation:
sudo rm -f /usr/local/bin/brain
```

Linux desktop installation:

```bash
rm -f "$HOME/.local/bin/brain-desktop"
rm -f "$HOME/.local/share/applications/brain.desktop"
```

macOS desktop installation:

```bash
rm -rf /Applications/Brain.app
```

## Windows status

Windows artifacts are not published yet. Brain currently relies on Unix sockets, Unix process signaling, and PTY behavior in its CLI/runtime. Windows support will be added after those components have native Windows implementations and CI coverage.
