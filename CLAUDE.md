# Fire Directory

Lobby directory display for OTHA's 100 Commerce Drive building. Runs on a Fire TV Stick 4K Max in fullscreen kiosk mode via the Downloader app.

## Stack

- Single-page HTML/CSS/JS (`index.html`)
- Deployed via GitHub Pages: `https://otha-tech.github.io/fire_directory/`
- Remote: `otha-tech/fire_directory`

## How It Works

The `floors` array at the top of `index.html` defines all tenants. A JavaScript carousel cycles through floor pages automatically. No build step — edit HTML, push, done.

## Editing Tenants

1. Edit the `floors` array in `index.html`
2. Push to `main`
3. GitHub Pages deploys in ~60 seconds
4. On the Fire Stick: exit fullscreen → reload → fullscreen again

## Hardware

See `SETUP.md` for Fire TV Stick setup, ADB sleep disable, and deployment to the building.

## Notes

- No dependencies, no build step, no env vars
- The Fire Stick connects to building WiFi (Commerce credentials)
- ADB commands disable sleep/screensaver — may need re-running after power loss
