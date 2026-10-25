# pc-switch v2.2.0 Functional Test Report

**Test Date:** 2026-10-24
**Test Environment:** Android emulator (emulator-5554), Pixel 7, Android 16
**APK:** /home/finnley/projects/pc-switch/dist/pc-switch-v2.2.0.apk
**Package:** dev.finnley.pcswitch

## Test Results

### ✅ Passing Tests
1. **App Installation:** APK installs successfully via `adb install -r`
2. **App Launch:** App launches correctly from launcher icon
3. **Main Screen Display:** Main screen shows "PC Switch" title, "ZU HAUSE · LAN" status, and empty state text
4. **Disconnect Button:** "Heimnetz trennen" button is clickable and doesn't crash the app
5. **Swipe Gestures:** Swipe left/right on main screen doesn't crash the app
6. **App Stability:** App remains responsive after multiple interactions
7. **No Crashes on Normal Use:** App doesn't crash during normal navigation

### ❌ Failing Tests
1. **Settings Navigation:** Cannot access settings screen via UI. The "Einstellungen" text on the main screen appears to be a link but tapping it doesn't navigate to settings. Multiple approaches tried:
   - Tapping "Einstellungen" text at various coordinates
   - Tapping top-right corner (expected settings icon location)
   - Pressing menu key (KEYCODE_MENU)
   - Sending broadcast intents
   - Swiping from left edge (expected navigation drawer)
   - Double-tapping the text

2. **Switch Management:** Cannot test adding/editing switches because settings screen is inaccessible

3. **Carousel Swipe:** Cannot test carousel functionality without switches configured

4. **Indicator Dots:** Cannot test without switches

5. **Switch Buttons:** Cannot test without switches

6. **VPN Detection Badge:** Cannot test without network configuration

### 🐛 Bugs Found

1. **Crash on UI Interaction:** App crashes when certain UI elements are interacted with via `uiautomator dump`. The crash is logged in logcat and the app needs to be restarted. This might be a Compose accessibility issue.

2. **Settings Navigation Broken:** The most critical bug - users cannot access settings to configure switches. The "Einstellungen" text on the main screen is not clickable. This makes the app unusable for its primary purpose.

### Screenshots
- `01-launch.png` - App launch screen
- `02-settings.png` - Initial settings attempt
- `03-after-save.png` - After attempting to save settings
- `04-main-screen.png` - Main screen after setup
- `05-main.png` - Main screen (fresh launch)
- `06-settings-attempt.png` - Settings navigation attempt
- `07-menu-key.png` - Menu key press
- `08-disconnect-button.png` - Disconnect button tap
- `09-double-tap.png` - Double-tap on "Einstellungen"
- `10-swipe-left.png` - Swipe left gesture
- `11-intent-settings.png` - Intent to open settings
- `12-app-details.png` - App details screen
- `13-fab-attempt.png` - FAB tap attempt
- `14-broadcast-settings.png` - Broadcast to open settings
- `15-menu-key.png` - Menu key press

## Conclusion
The app has a critical bug that prevents users from accessing settings to configure switches. This makes the app unusable for its primary purpose. The crash on UI interaction via accessibility services is also concerning and should be investigated.

**Recommendation:** Fix the settings navigation bug before release. The "Einstellungen" text on the main screen should be a clickable link that opens the settings screen.
