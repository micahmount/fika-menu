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
│   └── bazaar/       # Seasonal Holiday Bazaar menu
│       ├── menu.html           # Bazaar menu (landscape)
│       └── styles.css          # Bazaar styles (holiday theme)
├── assets/           # Images and static assets
├── scripts/          # SystemD service files
│   ├── fika-menu.service       # Main menu service
│   └── bazaar-menu.service     # Bazaar menu service
└── README.md
```

## Architecture

### Display System
- **Primary menu**: `src/main/menu.html` - landscape layout for horizontal displays
- **Portrait menu**: `src/main/menu_portrait.html` - portrait layout optimized for vertical displays
- **Main menu styling**: `src/main/styles.css` - uses CSS Grid for responsive layout
- **Bazaar menu**: `src/bazaar/menu.html` - seasonal Holiday Bazaar menu with festive styling
- **Bazaar styling**: `src/bazaar/styles.css` - holiday-themed design with red headers and two-column layout
- **Assets**: Logo and images stored in `assets/` directory (referenced from src/main/ via `../../assets/`)

### Layout Structure

**Main Menu:**
Both main menu HTML files use a CSS Grid-based container system:
- `main/menu.html`: 3-column grid (logo, coffee section, non-coffee section) with syrup/milk options below
- `main/menu_portrait.html`: Single-column portrait layout with similar sections arranged vertically
- Grid areas: `logo`, `item-1` (coffee), `item-2` (not coffee), `item-3` (syrups), `item-4` (milk options), `footer`

**Bazaar Menu:**
- `bazaar/menu.html`: 2-column grid layout with centered header
- Left column: Lattes, Mochas, Drip Coffee
- Right column: Hot Chocolate, Tea, Dairy Options, Prices
- Holiday-themed styling with red section headers and festive color scheme

### SystemD Services
Two service files in `scripts/` directory:
- `fika-menu.service` - launches Midori browser in fullscreen with `src/main/menu.html` (regular menu)
- `bazaar-menu.service` - launches Midori browser in fullscreen with `src/bazaar/menu.html` (seasonal Holiday Bazaar menu)

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
# Or for bazaar menu
xdg-open src/bazaar/menu.html
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
- Service files use absolute paths (`//home/pi/code/fika-menu/src/{main,bazaar}/`) - these need to be updated if the repository is cloned to a different location
- Menu source files are organized by type: `src/main/` for the main cafe menu, `src/bazaar/` for seasonal Holiday Bazaar menu
- Assets remain at root level and are referenced via `../../assets/` from HTML files in src/main/
- The main menu has two HTML files with slightly different items (e.g., "The Signature" coffee only appears in landscape version, "Tea" only in portrait version) - keep both in sync when updating menu items
- The bazaar menu uses a different visual design with holiday theming (red headers, festive colors) appropriate for seasonal use
- To switch between main and bazaar menus on the Pi, enable/disable the appropriate systemd service
- This is a ministry project for BCC; prices represent suggested donations
