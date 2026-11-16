# EasyAngler
 
A macOS app that automates the **Angler quest** in Fisch. It talks to the Angler, accepts the job,
reads which fish is wanted, finds it in your backpack, and hands it over, then repeats on a cooldown.
 
**Version 1.0.0** · macOS 14 or later
 
## How it works
 
Each quest:
 
1. Presses **E** and waits for the Angler's dialog.
2. Clicks the **"What's the job?"** option.
3. Reads the Angler's reply with on-screen text recognition and pulls out the fish
   ("I need you to find a Treble Bass in Moosewood" gives *Treble Bass*).
4. Presses **G**, clicks the backpack search box, types the fish, clicks the fish slot to hold it,
   and presses **G** to close the backpack.
5. Presses **E** to talk to the Angler again and clicks **Give**.
6. Waits out the cooldown (default 2 minutes), then starts over.
If it can't read a fish name, it retries after 15 seconds. EasyAngler never uses shift lock, so the
camera stays put and the Angler's text stays readable.
 
## Install
 
1. Open `EasyAngler-1.0.0.dmg` and drag **EasyAngler** into **Applications**.
2. Launch it. If an orange **Permissions needed** banner shows, use its buttons to open System
   Settings, switch EasyAngler on under **Screen Recording** and **Accessibility**, then quit and
   reopen the app.
Don't have the .dmg yet? See [Build it yourself](#build-it-yourself).
 
## Set up
 
You only do this once. Your boxes and settings are saved.
 
1. Put Roblox on your **main display** and leave the window where it is.
2. Click **Set up missing boxes**. For each box, EasyAngler hides and the screen dims. Drag a
   rectangle over the spot in Roblox (Esc cancels):
   | Box | What to cover |
   | --- | --- |
   | **Angler text** | The Angler's speech line only. Keep it tight so nearby signs (like the "[Trout]" label) aren't included. |
   | **Backpack search** | The search field in your backpack (open it with G first). |
   | **Fish slot** | The first slot that appears after you search. |
   | **Job option** | The "What's the job?" choice in the dialog. |
   | **Give button** | The button that hands the fish over. |
3. Click **Show boxes** to outline all five on screen for 3 seconds and check them. Use **Redo** on
   any row to redraw it.
4. Click **Test reading** while the Angler's text is on screen. It shows what was read and which fish
   was matched.
## Use it
 
1. Stand next to the Angler.
2. Click **Start**, then click into the Roblox window within 5 seconds.
3. Watch the status pill in the header: it shows the current step, then a countdown to the next quest.
**To stop:** click **Stop**, or move the mouse into the **top-left corner** of the screen.
 
### Settings
 
- **Cooldown between quests:** 10 seconds to 10 minutes (default 2 minutes).
- **Action speed:** 0.5x to 2x. Multiplies every pause between actions. Raise it if the game lags.
## Build it yourself
 
You need Apple's command-line tools (`xcode-select --install`). In VS Code, install the **Swift**
extension (`swiftlang.swift-vscode`) and open the folder that directly contains `Package.swift` and
`build.sh`.
 
- **Debug build:** `swift build` (or Cmd+Shift+B).
- **App and installer:** `bash build.sh`, or Terminal > Run Task > "EasyAngler: package .app + .dmg".
  Output lands in `dist/`:
  - `dist/EasyAngler.app`
  - `dist/EasyAngler-1.0.0.dmg`
- **Universal app (Intel + Apple Silicon):** `UNIVERSAL=1 bash build.sh`.
- **Stable signing:** create a self-signed "Code Signing" certificate in Keychain Access
  (Certificate Assistant > Create a Certificate…), then build with
  `SIGN_ID="Your Certificate Name" bash build.sh`.
## Customize
 
| What | Where |
| --- | --- |
| App name | `APP_NAME` in `build.sh`; `CFBundleName`, `CFBundleDisplayName`, `CFBundleExecutable` in `Resources/Info.plist`; the target name in `Package.swift` and the `Sources/EasyAngler` folder |
| Icon | Put a 1024×1024 PNG at `Resources/icon.png` and rebuild |
| Version | `VERSION` in `build.sh` and `CFBundleShortVersionString` in `Resources/Info.plist` |
| Bundle ID | `CFBundleIdentifier` in `Resources/Info.plist` (currently `com.easyangler.app`) |
| Fish list | `Sources/EasyAngler/Fish.swift` |
| Pause lengths | the `pause(...)` values in `Sources/EasyAngler/Bot.swift` |
 
## Project layout
 
```
Package.swift
build.sh                     builds the .app and .dmg
Resources/Info.plist
Sources/EasyAngler/
  EasyAnglerApp.swift        app entry point
  ContentView.swift          the window
  SelectionOverlay.swift     drag-a-box and Show boxes overlays
  AnglerRunner.swift         app state and controls
  Bot.swift                  the quest loop
  Fish.swift                 fish list and name matching
  Screen.swift               screen capture and text recognition
  Input.swift                key presses, clicks, permissions
  Config.swift               saved boxes and settings
```
 
Settings are stored at `~/Library/Application Support/EasyAngler/config.json`.
 
## Troubleshooting
 
- **"Permissions needed" won't go away:** turn EasyAngler on under both Screen Recording and
  Accessibility, then quit and reopen the app. After a rebuild, macOS may forget these grants:
  remove EasyAngler from each list and add it again.
- **Test reading shows the wrong text or no fish:** redraw the Angler text box tightly around the
  speech line, and make sure the text is on screen when you test.
- **Clicks miss their targets:** don't move or resize the Roblox window after drawing the boxes.
  Use **Show boxes** to check, and redraw any that are off.
- **Actions happen too fast and get skipped:** raise **Action speed**.
- **`xcbuild` error when building:** use `bash build.sh` (not the `--arch` flags), which works with
  just the command-line tools.
- **Other Macs warn "unidentified developer":** the app isn't notarized. Right-click it and choose
  Open.
## Limitations
 
- Main display only.
- The Angler's text must name the fish; hints without the name aren't supported.
- Only the 16 fish in `Fish.swift` are recognized.
## Disclaimer
 
Automating gameplay may break Roblox's or the game's rules and could get an account banned. Use at
your own risk.
 
