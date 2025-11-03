# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Fika Menu is a minimalist digital menu board for Cafe Fika, built with static HTML/CSS for display on a dedicated screen. The project runs on a Raspberry Pi using systemd services to automatically launch the menu in fullscreen mode on boot.

## Architecture

### Display System
- **Primary menu**: `menu.html` - landscape layout for horizontal displays
- **Portrait menu**: `menu_portrait.html` - portrait layout optimized for vertical displays
- **Shared styling**: `styles.css` - uses CSS Grid for responsive layout
- **Assets**: Logo and images stored in `assets/` directory

### Layout Structure
Both HTML files use a CSS Grid-based container system:
- `menu.html`: 3-column grid (logo, coffee section, non-coffee section) with syrup/milk options below
- `menu_portrait.html`: Single-column portrait layout with similar sections arranged vertically
- Grid areas: `logo`, `item-1` (coffee), `item-2` (not coffee), `item-3` (syrups), `item-4` (milk options), `footer`

### SystemD Services
Two service files in `scripts/` directory:
- `fika-menu.service` - launches Midori browser in fullscreen with `menu.html`
- `bazaar-menu.service` - launches xpdf in fullscreen with a Holiday Bazaar PDF (seasonal variant)

Both services:
- Run as user `pi`
- Start after network is available
- Auto-restart on failure
- Target the graphical environment

## Development Commands

### Viewing Changes
Since this is a static HTML/CSS project, simply open the HTML files in a browser:
```bash
# Open in default browser
xdg-open menu.html
# Or for portrait version
xdg-open menu_portrait.html
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
- Service files use absolute paths (`//home/pi/code/fika-menu/`) - these need to be updated if the repository is cloned to a different location
- The menu has two HTML files with slightly different items (e.g., "The Signature" coffee only appears in landscape version, "Tea" only in portrait version) - keep both in sync when updating menu items
- This is a ministry project for BCC; prices represent suggested donations
