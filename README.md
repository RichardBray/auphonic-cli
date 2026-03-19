# auphonic-cli

A fast, zero-dependency CLI for processing audio files through [Auphonic](https://auphonic.com). Upload, process, and download your audio — all from the terminal.

## Installation

### Quick install (macOS & Linux)

```bash
curl -fsSL https://github.com/RichardBray/auphonic-cli/releases/latest/download/install.sh | bash
```

### Install a specific version

```bash
VERSION=v0.1.0 curl -fsSL https://github.com/RichardBray/auphonic-cli/releases/latest/download/install.sh | bash
```

### Windows

Download `auphonic-windows-x64.exe` from the [latest release](https://github.com/RichardBray/auphonic-cli/releases/latest) and add it to your PATH.

### From source

```bash
git clone https://github.com/RichardBray/auphonic-cli.git
cd auphonic-cli
bun install
```

## Setup

Get your API key from [Auphonic API settings](https://auphonic.com/engine/api/) and set it as an environment variable:

```bash
export AUPHONIC_API_KEY="your-api-key-here"
```

Add this to your shell profile (`~/.bashrc`, `~/.zshrc`, `~/.config/fish/config.fish`, etc.) to persist it.

## Usage

```bash
auphonic <file> [options]
```

### Options

| Flag | Description | Default |
|------|-------------|---------|
| `-p, --preset <name>` | Auphonic preset name | `Usual-2` |
| `-o, --output-dir <path>` | Directory to save processed files | `~/Downloads/auphonic_results` |
| `-t, --timeout <seconds>` | Max time to wait for processing | `300` |
| `--list-presets` | List available presets and exit | |
| `-v, --version` | Show version | |
| `-h, --help` | Show help | |

### Examples

Process a file with the default preset:

```bash
auphonic recording.wav
```

Use a specific preset:

```bash
auphonic recording.wav -p "My Podcast Preset"
```

Save output to a custom directory:

```bash
auphonic recording.wav -o ./processed
```

List your available presets:

```bash
auphonic --list-presets
```

### Running from source

If you cloned the repo instead of installing globally:

```bash
bun run index.ts recording.wav -p "My Preset"
```

## Uninstallation

Remove the binary:

```bash
rm ~/.local/bin/auphonic
```

Then remove the PATH entry added by the installer from your shell config(s). Look for and delete these lines:

- **bash/zsh** (`~/.bashrc`, `~/.bash_profile`, or `~/.zshrc`):
  ```
  # Added by auphonic-cli installer
  export PATH="$HOME/.local/bin:$PATH"
  ```
- **fish** (`~/.config/fish/config.fish`):
  ```
  fish_add_path /Users/<your-username>/.local/bin
  ```

> Skip removing the PATH entry if other tools in `~/.local/bin` depend on it.

## Using Claude Code

If you use [Claude Code](https://claude.com/claude-code), you can install, update, or uninstall by pasting these prompts:

**Install:**
```
Download the latest auphonic-cli binary for my OS and architecture from
https://github.com/RichardBray/auphonic-cli/releases/latest — save it to
~/.local/bin/auphonic, make it executable, and add ~/.local/bin to my
shell PATH if it isn't already.
```

**Update:**
```
Update auphonic-cli to the latest version by downloading the correct binary
for my OS and architecture from
https://github.com/RichardBray/auphonic-cli/releases/latest and replacing
~/.local/bin/auphonic with it.
```

**Uninstall:**
```
Remove ~/.local/bin/auphonic and remove any "Added by auphonic-cli installer"
PATH entries from my shell config files.
```

## How it works

1. Looks up your preset by name via the Auphonic API
2. Uploads your audio file and creates a production
3. Starts the production and polls for completion (every 15s)
4. Downloads the processed output files to your output directory

## License

MIT
