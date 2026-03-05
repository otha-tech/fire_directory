# Fire TV Stick 4K Max — Lobby Directory Setup

Display URL: `https://otha-tech.github.io/fire_directory/`

## Overview: Setup at Office, Deploy at Building

You'll do all the configuration at your office using your office WiFi, then move the stick to 100 Commerce Drive and switch it to that building's WiFi. The Fire Stick remembers all your settings — you just need to change the network on-site.

## 1. Initial Fire TV Stick Setup (at your office)

### Hardware

1. Plug the Fire TV Stick into any TV or monitor with HDMI (your office TV is fine for setup)
2. Connect the USB power cable to the included power adapter — **use the included adapter**, not the TV's USB port (not enough power)
3. Turn the TV to the correct HDMI input

### First boot

4. The Fire Stick will power on automatically and show the logo
5. It will prompt you to **pair the remote** — hold the Home button until it connects
6. Select **language**

### Connect to WiFi

7. It will scan for networks. Select your **office WiFi network**
8. Enter the WiFi password using the on-screen keyboard (use the remote's directional pad to navigate)
9. Wait for it to connect and verify internet access

### Amazon account

10. Sign into your **Amazon account** (or create one) — this is required to access the Appstore
11. You can skip any "register household" or "choose your apps" prompts
12. Let it complete any **firmware updates** — it may restart once or twice, just wait

## 2. Install Downloader App

Downloader (by AFTVnews.com) is a free app with a built-in browser that supports **true fullscreen mode** — no URL bar, no browser chrome.

### Install

You can install it two ways:

- **From the Fire Stick**: Open the Amazon Appstore, search for **"Downloader"**, install it
- **From your Mac**: Go to [amazon.com/appstore](https://www.amazon.com/appstore), search "Downloader", and click "Get App" → deliver to your Fire TV

### Load the directory

1. Open **Downloader**
2. Enter the URL: `https://otha-tech.github.io/fire_directory/`
3. The directory should load and start cycling through floor pages

### Save as favorite

4. Open the sidebar (click the three horizontal bars on the left)
5. Go to **Favorites** and save the current URL — this way you can always get back to it

### Go fullscreen

6. Press the **three horizontal bars button** on the Fire TV remote, OR click the fullscreen icon in the upper-right corner of the screen
7. The directory now fills the entire screen with no browser chrome

**Note**: A red cursor circle may remain on screen. Use the remote's directional pad to park it in a corner where it's not noticeable.

## 3. Disable Sleep / Screensaver via ADB

The Fire TV will try to sleep after inactivity. Disable this over ADB from a Mac on the same network.

### Enable Developer Options & ADB

1. **Settings → My Fire TV → About → Fire TV Stick 4K Max**
2. Click the device name **7 times** to enable Developer Options
3. **Settings → My Fire TV → Developer Options**
4. Enable **"ADB Debugging"**
5. Note the Fire TV's IP address: **Settings → My Fire TV → About → Network**

### Connect from Mac

```bash
# Install ADB if needed
brew install android-platform-tools

# Connect (replace with actual IP)
adb connect 192.168.1.XXX:5555

# Accept the connection prompt on the TV screen

# Disable sleep
adb shell settings put secure sleep_timeout 0
adb shell settings put system screen_off_timeout 2147460000

# Disable screensaver
adb shell settings put secure screensaver_enabled 0

# Verify
adb shell settings get system screen_off_timeout
# Should return: 2147460000
```

## 4. Deploy to 100 Commerce Drive

Once everything is configured and tested at your office, move the stick to the building.

### At the building

1. Plug the Fire Stick into the lobby TV's HDMI port and connect power
2. It will boot up and try to connect to your office WiFi — **it will fail**, that's expected
3. Press the **Home button** on the remote to get to the Fire TV home screen
4. Go to **Settings → Network**
5. Select the **building's WiFi network** (Commerce credentials) and enter the password
6. Wait for it to connect, then open **Downloader**
7. The directory URL should still be saved in Favorites — open it and go fullscreen
8. Verify the directory loads and cycles through the 3 floor pages

### Hide the remote

9. Once it's running, stash the remote somewhere accessible but out of sight (you'll need it if WiFi changes or the stick needs attention)

## Updating the Directory

1. Edit the `floors` array at the top of `index.html`
2. Commit and push to `main`
3. GitHub Pages deploys in ~60 seconds
4. To refresh on the Fire Stick: press the three-bars button on the remote to exit fullscreen, reload the page, then go fullscreen again

## Troubleshooting

- **Screen goes black**: Re-run the ADB sleep commands above
- **Directory not showing**: Open Downloader, check Favorites for the saved URL
- **Directory not updating after a push**: Exit fullscreen, reload the page in Downloader, go fullscreen again
- **WiFi disconnects**: Consider a USB ethernet adapter (Amazon Ethernet Adapter for Fire TV) for more reliable connectivity
- **Red cursor visible**: Use the remote's directional pad to park it in a corner of the screen
