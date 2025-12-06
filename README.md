# RunVenture Website

Marketing website for [runventure.ca](https://runventure.ca) - self-hosted with Caddy on Mac mini.

## Prerequisites

Install Caddy via Homebrew:
```bash
brew install caddy
```

## Development

Start local server on port 8080:
```bash
caddy run --config Caddyfile
```

Visit: http://localhost:8080

## Production Setup (Mac mini)

### 1. DNS Configuration
Point `runventure.ca` A record to your Mac mini's public IP (or use Cloudflare tunnel).

### 2. Port Forwarding
In UniFi, forward ports 80 and 443 to your Mac mini's local IP.

### 3. Update Caddyfile
Uncomment the production block in `Caddyfile` and comment out the dev block.

### 4. Run as Service
```bash
# Install as macOS service
sudo caddy stop
sudo caddy start --config /Users/erik/Documents/GitHub/runventure-website/Caddyfile
```

Or create a LaunchDaemon for auto-start on boot (see below).

### LaunchDaemon (Auto-start)

Create `/Library/LaunchDaemons/com.runventure.caddy.plist`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.runventure.caddy</string>
    <key>ProgramArguments</key>
    <array>
        <string>/opt/homebrew/bin/caddy</string>
        <string>run</string>
        <string>--config</string>
        <string>/Users/erik/Documents/GitHub/runventure-website/Caddyfile</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
```

Load it:
```bash
sudo launchctl load /Library/LaunchDaemons/com.runventure.caddy.plist
```

## Structure

```
runventure-website/
├── Caddyfile          # Caddy web server config
├── README.md
└── public/            # Static site files
    └── index.html
```

## License

Copyright © 2025. All rights reserved.
