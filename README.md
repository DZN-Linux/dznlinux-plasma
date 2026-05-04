# DZNLinux Plasma

KDE Plasma 6 desktop configuration for DZN Linux, deployed to new user home directories via `/etc/skel`. Sets up the full desktop environment — window decorations, panel layout, virtual desktops, effects, keyboard shortcuts, Konsole profile, and autostart entries — ready to use on first login.

## Features

- McMojave Aurorae window decoration (dark, macOS-inspired)
- ChromeOS-dark Look and Feel with matching color scheme
- Tela-purple-dark icon theme with material cursors
- 4 virtual desktops arranged in a 2×2 grid
- KWin effects: Wobbly Windows, Magic Lamp minimize, blur
- Panel with Kicker launcher, Icon Tasks, Pager, System Tray, and Digital Clock
- Yakuake drop-down terminal (F12) with autostart
- KGpg encryption tool in the system tray with autostart
- Konsole profile with Hack 10pt font, 100-column width, Breeze color scheme
- Full global shortcut map: Ctrl+Alt+T for Konsole, Meta+E for Dolphin, F12 for Yakuake, and more
- Distro info in KDE System Settings "About This System" showing DZNLinux
- Compatible with KDE Plasma 6.6 on both X11 and Wayland

## Compatibility

- **Desktop Environment**: KDE Plasma 6.6
- **Display Server**: X11 and Wayland
- **Distribution**: Arch Linux and Arch-based distributions

## Installation

### From DZN Linux Repository (Recommended)

First, add the DZN Linux repository to your system:

```bash
# Add the PGP key
sudo pacman-key --recv-key BB31837564255477
sudo pacman-key --lsign-key BB31837564255477
```

Add the following to `/etc/pacman.conf`:

```ini
[dznlinux_repo]
SigLevel = Required DatabaseOptional
Server = https://repo.dozzen.me/archlinux/$repo/$arch

[dznlinux_repo_3party]
SigLevel = Required DatabaseOptional
Server = https://repo.dozzen.me/archlinux/$repo/$arch
```

Then install the package:

```bash
sudo pacman -Sy
sudo pacman -S dznlinux-plasma-git
```

### Manual Installation

```bash
# Clone this repository
git clone https://github.com/DZN-Linux/dznlinux-plasma.git

# Copy configuration to your home directory
cp -r dznlinux-plasma/etc/skel/. ~/

# Copy system-wide config
sudo cp -r dznlinux-plasma/etc/xdg/. /etc/xdg/
```

## Configuration

### Applying to an Existing User

The package deploys to `/etc/skel/` which only takes effect for newly created users. To apply to an existing account:

```bash
cp -r /etc/skel/.config/. ~/.config/
cp -r /etc/skel/.local/share/. ~/.local/share/
```

Then log out and back in.

### Window Decoration

The config sets McMojave as the Aurorae window decoration. Install the theme from the AUR before logging in:

```bash
yay -S aurorae-theme-mcmojave
```

If the theme is missing, KWin falls back to Breeze automatically.

### Look and Feel

The ChromeOS-dark Look and Feel package sets the global color scheme and splash screen. Install it from the AUR:

```bash
yay -S plasma5-themes-chromeos-dark
```

If not installed, system defaults apply. The color values in `kdeglobals` still take effect regardless.

### Icons and Cursor

```bash
sudo pacman -S tela-icon-theme
yay -S material-cursors
```

### Keyboard Shortcuts

Key shortcuts configured out of the box:

| Shortcut | Action |
|---|---|
| Ctrl+Alt+T | Launch Konsole |
| F12 | Toggle Yakuake |
| Meta+E | Open Dolphin |
| Meta+D | Show Desktop |
| Meta+L | Lock Screen |
| Alt+Tab | Walk Through Windows |
| Ctrl+F9 | Present Windows (current desktop) |
| Ctrl+F10 | Present Windows (all desktops) |
| Ctrl+F8 | Show Desktop Grid |
| Meta+Left/Right/Up/Down | Quick Tile Window |
| Meta+PgUp / PgDown | Maximize / Minimize Window |
| Print | Launch Spectacle |

## File Structure

```
etc/
├── skel/
│   ├── .config/
│   │   ├── autostart/
│   │   │   ├── org.kde.kgpg.desktop      # KGpg autostart
│   │   │   └── org.kde.yakuake.desktop   # Yakuake autostart
│   │   ├── kcminputrc                    # Mouse/keyboard: material_cursors, repeat rate
│   │   ├── kdeglobals                    # Colors, icons, widget style, LnF package
│   │   ├── kglobalshortcutsrc           # All global keyboard shortcuts
│   │   ├── konsolerc                     # Konsole window settings and default profile
│   │   ├── kscreenlockerrc              # Screen locker: timeout and theme
│   │   ├── ksmserverrc                  # Session restore mode
│   │   ├── ksplashrc                    # Splash screen theme
│   │   ├── kwinrc                       # Window manager: decorations, effects, desktops
│   │   ├── plasma-org.kde.plasma.desktop-appletsrc  # Panel and desktop layout
│   │   └── yakuakerc                    # Yakuake: height 60%, width 80%, no tab bar
│   └── .local/share/konsole/
│       ├── Breeze.colorscheme           # Terminal color scheme
│       └── dznlinux.profile             # Konsole profile: Hack 10pt, 100 cols, UTF-8
└── xdg/
    └── kcm-about-distrorc               # "About This System" distro info
```

## License

Licensed under the GNU General Public License v3.0 or later (GPL-3.0-or-later).

See [LICENSE](LICENSE) file for details.

## Credits

DZN Linux desktop configuration: Seth Dawson

## Links

- GitHub: https://github.com/DZN-Linux/dznlinux-plasma
- DZN Linux: https://github.com/DZN-Linux

## Troubleshooting

### Window decorations show Breeze instead of McMojave

The McMojave Aurorae theme is not installed. Install it and log out/in:

```bash
yay -S aurorae-theme-mcmojave
```

### Icons show as missing or default

Install the Tela icon theme:

```bash
sudo pacman -S tela-icon-theme
```

Then apply in **System Settings → Appearance → Icons**.

### Ctrl+Alt+T does not open Konsole

Ensure Konsole is installed:

```bash
sudo pacman -S konsole
```

The shortcut is registered via `kglobalshortcutsrc` and takes effect on next login.

### Yakuake does not start on login

Ensure Yakuake is installed:

```bash
sudo pacman -S yakuake
```

The autostart entry in `~/.config/autostart/org.kde.yakuake.desktop` handles launch on login.

### Config changes are not applied after install

The package writes to `/etc/skel/`. To apply to your existing user account:

```bash
cp -r /etc/skel/.config/. ~/.config/
cp -r /etc/skel/.local/share/. ~/.local/share/
```

Log out and back in for all changes to take effect.
