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

## 2. Install Fully Kiosk Browser (Recommended)

Fully Kiosk Browser runs the directory in true fullscreen with no browser chrome, auto-launches on boot, and can auto-refresh on a schedule. One-time license: **$7.90 per device**.

### Enable sideloading

1. **Settings → My Fire TV → About → Fire TV Stick 4K Max**
2. Click the device name **7 times** to enable Developer Options
3. **Settings → My Fire TV → Developer Options**
4. Enable **"Apps from Unknown Sources"**

### Install via Downloader app

1. Open the **Amazon Appstore**, search for **"Downloader"**, install it
2. Open Downloader, enter this URL: `https://www.fully-kiosk.com/apk/`
3. Download the latest **Fully Kiosk Browser** APK
4. Install when prompted

### Configure Fully Kiosk

1. Open Fully Kiosk Browser
2. Set **Start URL** to: `https://otha-tech.github.io/fire_directory/`
3. **Web Content Settings**
   - Enable JavaScript: ✅
4. **Other Settings → Launch on Boot**: ✅
5. **Other Settings → Keep Screen On**: ✅
6. **Other Settings → Fullscreen Mode**: ✅
7. **Web Auto Reload** (optional): Set to reload every 3600 seconds (1 hour) to pick up directory changes

## 3. Disable Sleep / Screensaver via ADB

The Fire TV will try to sleep after inactivity. Disable this over ADB from a Mac on the same network.

### Enable ADB

1. **Settings → My Fire TV → Developer Options**
2. Enable **"ADB Debugging"**
3. Note the Fire TV's IP address: **Settings → My Fire TV → About → Network**

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

## 4. Silk Browser Fallback

If Fully Kiosk isn't available, the built-in Silk browser works but shows a URL bar at the top.

1. Open **Silk Browser**
2. Navigate to `https://otha-tech.github.io/fire_directory/`
3. **Settings → General → Homepage**: set to the URL above
4. Tap the fullscreen icon if available

Note: Silk doesn't support auto-launch on boot or kiosk lockdown.

## 5. Deploy to 100 Commerce Drive

Once everything is configured and tested at your office, move the stick to the building.

### At the building

1. Plug the Fire Stick into the lobby TV's HDMI port and connect power
2. It will boot up and try to connect to your office WiFi — **it will fail**, that's expected
3. If Fully Kiosk launches, press the **Home button** on the remote to get to the Fire TV home screen
4. Go to **Settings → Network**
5. Select the **building's WiFi network** and enter the password
6. Wait for it to connect — once online, go back to Fully Kiosk (it should be in your recent apps) or it will auto-launch on next reboot
7. Verify the directory loads and cycles through the 3 floor pages

### Hide the remote

8. Once it's running, stash the remote somewhere accessible but out of sight (you'll need it if WiFi changes or the stick needs attention)

## Updating the Directory

1. Edit the `floors` array at the top of `index.html`
2. Commit and push to `main`
3. GitHub Pages deploys in ~60 seconds
4. The Fire TV will show updated content on next page load or auto-refresh

## Troubleshooting

- **Screen goes black**: Re-run the ADB sleep commands above
- **Fully Kiosk not starting on boot**: Verify "Launch on Boot" is enabled in Fully Kiosk settings
- **Directory not updating**: Check Fully Kiosk auto-reload is set, or manually refresh
- **WiFi disconnects**: Consider a USB ethernet adapter (Amazon Ethernet Adapter for Fire TV) for more reliable connectivity
