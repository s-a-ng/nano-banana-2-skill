# Setup tutorial (Linux and Windows)

You need an OpenRouter key (starts with `sk-or-`) with some credits on it. Each image costs about $0.04.

## Linux

**1. Install the requirements.** Pick the line for your distro. `ffmpeg` and `imagemagick` are only needed for transparent images (`-t`).

```bash
sudo apt install git unzip curl ffmpeg imagemagick       # Ubuntu / Debian / Mint
```
```bash
sudo pacman -S --needed git unzip curl ffmpeg imagemagick  # Arch
```
```bash
sudo dnf install git unzip curl ffmpeg ImageMagick       # Fedora
```

**2. Install the tool.** Paste this whole line. It installs Bun if you don't have it, downloads the tool, and asks for your key (the key stays hidden while you type or paste it):

```bash
command -v bun >/dev/null || curl -fsSL https://bun.sh/install | bash; export PATH="$HOME/.bun/bin:$PATH"; git clone https://github.com/s-a-ng/nano-banana-2-skill.git ~/tools/nano-banana-2 && cd ~/tools/nano-banana-2 && bun install && bun link && read -rsp "OpenRouter key: " k && echo && mkdir -p ~/.nano-banana && (umask 077; printf 'OPENROUTER_API_KEY=%s\n' "$k" > ~/.nano-banana/.env) && unset k && nano-banana --help | head -3
```

If it ends by printing `Nano Banana 2 - AI Image Generation CLI`, it worked. Close and reopen the terminal so `nano-banana` works everywhere.

## Windows

Use **PowerShell** (Start menu, type "PowerShell"). Run each block, one at a time.

**1. Install Git** (skip if you have it):

```powershell
winget install --id Git.Git -e
```

**2. Install Bun:**

```powershell
irm bun.sh/install.ps1 | iex
```

**Close PowerShell and open a new one** so it can find `git` and `bun`.

**3. Install the tool:**

```powershell
git clone https://github.com/s-a-ng/nano-banana-2-skill.git "$HOME\tools\nano-banana-2"; cd "$HOME\tools\nano-banana-2"; bun install; bun link
```

**4. Save your key.** It asks for the key; paste it and press Enter:

```powershell
mkdir -Force "$HOME\.nano-banana" | Out-Null; Set-Content "$HOME\.nano-banana\.env" ("OPENROUTER_API_KEY=" + (Read-Host "OpenRouter key")) -Encoding ascii
```

**5. Check it works:**

```powershell
nano-banana --help
```

If it says `nano-banana` is not recognized, open a new PowerShell and try again. If it still fails, you can always run it as `bun "$HOME\tools\nano-banana-2\src\cli.ts" --help`.

**6. Optional, for transparent images (`-t`):**

```powershell
winget install --id Gyan.FFmpeg -e
```
```powershell
winget install --id ImageMagick.ImageMagick -e
```

Then open a new PowerShell.

## Add it to Claude Code (both systems)

This teaches Claude Code how to use the tool, so you can just ask it to "generate an image of…":

```bash
claude plugin marketplace add s-a-ng/nano-banana-2-skill
```
```bash
claude plugin install nano-banana@nano-banana-2-skill-marketplace
```

Or, inside Claude Code, type `/plugin`, add the marketplace `s-a-ng/nano-banana-2-skill`, and install `nano-banana` from it.

## Using it

```bash
nano-banana "a cozy cabin in a snowy forest at night"
nano-banana "cinematic city skyline" -a 16:9 -s 2K -o skyline
nano-banana "same character but waving" -r character.png
nano-banana "cartoon treasure chest" -t -o chest
nano-banana --costs
```

| Option | What it does |
|--------|--------------|
| `-o name` | Output filename (no extension) |
| `-d folder` | Where to save (default: current folder) |
| `-a 16:9` | Aspect ratio (`1:1`, `16:9`, `9:16`, `4:3`, ...) |
| `-s 2K` | Size: `1K` (default), `2K`, `4K`. Bigger costs more |
| `-m flare` | Model: `nb2.1` (default, Nano Banana), `flare` / `sunburst` (GPT Image 2.5, cheaper), `pro` |
| `-r file.png` | Reference image to edit or copy the style of. Repeat for more |
| `-t` | Transparent background (needs ffmpeg + ImageMagick) |
| `--costs` | Show how much you've spent |

## Updating

Linux:
```bash
cd ~/tools/nano-banana-2 && git pull && bun install
```

Windows:
```powershell
cd "$HOME\tools\nano-banana-2"; git pull; bun install
```

## Changing your key

Edit the file `~/.nano-banana/.env` (Windows: `C:\Users\<you>\.nano-banana\.env`). It has one line: `OPENROUTER_API_KEY=sk-or-...`
