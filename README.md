# Fika Menu

A minimalist digital menu board for Cafe Fika, built with HTML/CSS for display on a dedicated screen.

## Setup

### Prerequisites

- A system with systemd (Linux)
- Web browser or kiosk mode setup

### Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/micahmount/fika-menu.git ~/code/fika
   cd ~/code/fika
   ```

2. **Create symlinks for the systemd service files:**

   ```bash
   sudo ln -s ~/code/fika/scripts/fika-menu.service /etc/systemd/system/fika-menu.service
   ```

3. **Reload systemd to recognize the new service:**

   ```bash
   sudo systemctl daemon-reload
   ```

4. **Enable the service to start on boot:**

   ```bash
   sudo systemctl enable fika-menu.service
   ```

5. **Start the service:**

   ```bash
   sudo systemctl start fika-menu.service
   ```

6. **Check the service status:**

   ```bash
   sudo systemctl status fika-menu.service
   ```

## Managing the Service

- **Start:** `sudo systemctl start fika-menu.service`
- **Stop:** `sudo systemctl stop fika-menu.service`
- **Restart:** `sudo systemctl restart fika-menu.service`
- **View logs:** `sudo journalctl -u fika-menu.service -f`
- **Disable autostart:** `sudo systemctl disable fika-menu.service`

## Updating

After making changes to the service file:

```bash
sudo systemctl daemon-reload
sudo systemctl restart fika-menu.service
```

## Development

The menu is a static HTML/CSS page designed for fullscreen display. Menu files are located in the `src/main/` directory. Edit the HTML and CSS files directly, and refresh the browser to see changes.

### Project Structure

```
fika/
├── src/              # Source files organized by menu type
│   ├── main/         # Main cafe menu
│   │   ├── menu.html           # Landscape menu
│   │   ├── menu_portrait.html  # Portrait menu
│   │   └── styles.css          # Shared styles
│   └── bazaar/       # Seasonal Holiday Bazaar menu
│       ├── menu.html           # Bazaar menu
│       └── styles.css          # Bazaar styles
├── assets/           # Images and static assets
├── scripts/          # SystemD service files
│   ├── fika-menu.service       # Main menu service
│   └── bazaar-menu.service     # Bazaar menu service
└── README.md
```

## License

MIT
