# Playing Neonvale from Steam (Steam Controller / Steam Deck)

Neonvale runs as a full-screen browser "app" added to Steam as a non-Steam game. Steam Input turns the
controller into a virtual Xbox pad, which the game supports out of the box (standard browser gamepad layout).

## Windows: Chrome (recommended)

1. Steam → **Games → Add a Non-Steam Game to My Library… → Browse** → `C:\Program Files\Google\Chrome\Application\chrome.exe`.
2. Right-click the new shortcut → **Properties**:
   - **Name:** `Neonvale`
   - **Launch options** (replace the profile folder with any folder you like; it is created on first launch):
     ```
     --user-data-dir="C:\Games\Neonvale\chrome-profile" --kiosk --autoplay-policy=no-user-gesture-required --no-first-run https://neonvale.dev
     ```
     | Flag | Why |
     | --- | --- |
     | `--user-data-dir=…` | Starts a **separate** Chrome. Without it, Chrome hands the page to your already-open browser and exits straight away, so Steam thinks the game closed: Steam Input, the overlay and play time stop working. It also keeps Neonvale's local save in its own folder (don't delete it, or use Cloud save). |
     | `--kiosk` | Full screen, no tabs or address bar. |
     | `--autoplay-policy=no-user-gesture-required` | Sound starts without a mouse click (browsers otherwise wait for a click or key; a controller press isn't enough everywhere). |
     | `--no-first-run` | Skips Chrome's welcome screens in the new profile. |
     | `--force-device-scale-factor=1.25` (optional) | Bigger UI for couch / TV play. |
3. **Controller layout:** shortcut → **Manage → Controller layout** (or in game: Steam button → Controller settings):
   - Choose **Gamepad** (Xbox-style buttons), or for the Steam Controller / Deck **Gamepad with Mouse Trackpad**
     (right trackpad moves the mouse, right-pad click = left click; the game switches between mouse and
     controller seamlessly).
   - Don't use Steam's **Web Browser** layout: it turns the controller into a mouse and keyboard, so the game's
     controller support never sees it.
   - Make sure Steam Input is enabled for the shortcut (Properties → Controller → "Enable Steam Input").
   - Optional extras for grips/back buttons: bind them to keyboard keys **G** Goals, **F** Friends, **D** Districts,
     **M** map. (On the pad, Y opens Goals and LB/RB switch to Friends and Districts anyway.)
4. Check it: in game, **Menu → Controls** lists the controllers the browser sees. You want
   `Xbox 360 Controller … · standard layout ✓`.
5. **Quit:** Steam button → **Exit game** (or Alt+F4).

## Steam Deck / SteamOS: Chrome (Flatpak)

1. Desktop Mode → **Discover** → install **Google Chrome** → add it to Steam (Steam → Add a Non-Steam Game → Google Chrome).
2. Once, in **Konsole**, let Chrome see the Deck's controls:
   ```
   flatpak --user override --filesystem=/run/udev:ro com.google.Chrome
   ```
3. Shortcut **Properties → Launch options**: keep what Steam put there and add the Neonvale part on the end, e.g.
   ```
   run --branch=stable --arch=x86_64 --command=/app/bin/chrome --file-forwarding com.google.Chrome @@u @@ --kiosk --autoplay-policy=no-user-gesture-required --no-first-run --user-data-dir=/home/deck/.var/app/com.google.Chrome/neonvale https://neonvale.dev
   ```
4. Controller layout: **Gamepad with Mouse Trackpad** (set the right trackpad click to Left Mouse Click).
5. Typing a cloud save code: **Steam + X** opens the on-screen keyboard.

The game's layout is tested at the Deck's 1280×800.

## Firefox (alternative)

- Target `firefox.exe` (or `firefox`), launch options:
  `-no-remote -profile "C:\Games\Neonvale\firefox-profile" --kiosk https://neonvale.dev`
  (`-no-remote -profile` is Firefox's version of a separate instance.)
- Sound without a click: in that profile open `about:config` and set `media.autoplay.default` = `0` and
  `media.autoplay.block-webaudio` = `false`.
- Chrome is simpler (one flag for sound) and has the more consistent gamepad support.

## Saves across devices

The local save lives in the browser profile folder. Use **Menu → Cloud save** to carry the city between the Steam
shortcut, your desktop browser and phone; only one device plays a city at a time (the other goes back to the title).

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| View scrolls on its own / buttons mixed up | The controller isn't reaching the game as a standard pad. Pick the **Gamepad** layout in Steam and check Menu → Controls. (The game also ignores triggers that rest "pressed", which used to drag the view north.) |
| No sound until you click | Add `--autoplay-policy=no-user-gesture-required` (Chrome) or the Firefox prefs above. |
| Steam says the game closed instantly; controller layout/overlay don't apply | Add `--user-data-dir=…` so Chrome starts its own instance. |
| Controller does nothing on the Deck | Run the Flatpak `/run/udev` override above. |
| Controller acts like a mouse | You're on the **Web Browser** layout: switch to **Gamepad** / **Gamepad with Mouse Trackpad**. |

## Controller map (in game)

| Action | Pad |
| --- | --- |
| Move the view | Left stick (hold L3 to go faster); right stick pans too |
| Inspect / talk / clear / place | A (aim with the reticle) |
| Cancel / back | B |
| Build menu | X (LB / RB switch tabs) |
| Goals → Friends → Districts | Y, then LB / RB |
| Hotbar | LB / RB |
| Nudge the view a tile (precise placing) | D-pad |
| Zoom | LT / RT |
| Map | View (press again to close) |
| Menu | Start |

Tested with `scripts/steamtest.mjs` (Steam's virtual Xbox pad plus a raw controller with resting triggers, at
1280×800 and 1080p) and `scripts/padtest.mjs` (controller-only tutorial).
