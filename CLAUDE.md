# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Fika Menu is a minimalist digital menu board for Cafe Fika, built with static HTML/CSS for display on a dedicated screen. The project runs on a Raspberry Pi using systemd services to automatically launch the menu in fullscreen mode on boot.

## Project Structure

```
fika/
├── src/              # Source files organized by menu type
│   ├── main/         # Main cafe menu
│   │   ├── menu.html           # Landscape menu
│   │   ├── menu_portrait.html  # Portrait menu
│   │   └── styles.css          # Shared styles
│   └── bazaar/       # Seasonal bazaar menu
├── assets/           # Images and static assets
├── scripts/          # SystemD service files
│   ├── fika-menu.service
│   └── bazaar-menu.service
└── README.md
```

## Architecture

### Display System
- **Primary menu**: `src/main/menu.html` - landscape layout for horizontal displays
- **Portrait menu**: `src/main/menu_portrait.html` - portrait layout optimized for vertical displays
- **Shared styling**: `src/main/styles.css` - uses CSS Grid for responsive layout
- **Assets**: Logo and images stored in `assets/` directory (referenced from src/main/ via `../../assets/`)
- **Bazaar menu**: `src/bazaar/` - directory for seasonal Holiday Bazaar menu files

### Layout Structure
Both HTML files use a CSS Grid-based container system:
- `menu.html`: 3-column grid (logo, coffee section, non-coffee section) with syrup/milk options below
- `menu_portrait.html`: Single-column portrait layout with similar sections arranged vertically
- Grid areas: `logo`, `item-1` (coffee), `item-2` (not coffee), `item-3` (syrups), `item-4` (milk options), `footer`

### SystemD Services
Two service files in `scripts/` directory:
- `fika-menu.service` - launches Midori browser in fullscreen with `src/main/menu.html`
- `bazaar-menu.service` - launches xpdf in fullscreen with a Holiday Bazaar PDF from assets/ (seasonal variant)

Both services:
- Run as user `pi`
- Start after network is available
- Auto-restart on failure
- Target the graphical environment

## Development Commands

### Viewing Changes
Since this is a static HTML/CSS project, simply open the HTML files in a browser:
```bash
# Open main menu in default browser
xdg-open src/main/menu.html
# Or for portrait version
xdg-open src/main/menu_portrait.html
```

### Service Management
```bash
# After editing service files
sudo systemctl daemon-reload
sudo systemctl restart fika-menu.service

# View service status
sudo systemctl status fika-menu.service

# View logs
sudo journalctl -u fika-menu.service -f

# Enable/disable autostart
sudo systemctl enable fika-menu.service
sudo systemctl disable fika-menu.service
```

## Important Notes

- The systemd services are configured for user `pi` and display `:0` - adjust these if deploying to different hardware
- Service files use absolute paths (`//home/pi/code/fika-menu/src/main/`) - these need to be updated if the repository is cloned to a different location
- Menu source files are organized by type: `src/main/` for the main cafe menu, `src/bazaar/` for seasonal menus
- Assets remain at root level and are referenced via `../../assets/` from HTML files in src/main/
- The menu has two HTML files with slightly different items (e.g., "The Signature" coffee only appears in landscape version, "Tea" only in portrait version) - keep both in sync when updating menu items
- This is a ministry project for BCC; prices represent suggested donations
