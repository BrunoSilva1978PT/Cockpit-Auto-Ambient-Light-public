# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's native Ambient Lights screen capture and drives selected Philips Hue, Govee and other SimHub Ambient Lights devices so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains release binaries and user documentation. The plugin source code is private.

## Current Features

- Uses SimHub's native Ambient Lights capture engine; it does not open a second screen-capture pipeline.
- Learns a cockpit mask automatically while you drive in cockpit camera.
- Works best with a stable cockpit camera and minimal HUD/overlay elements over the cockpit view.
- Stores the last accepted cockpit mask per game/car and reuses it on the next session.
- Keeps checking a candidate mask in the background for FOV, seat position and cockpit-camera changes.
- Drives lights already configured and enabled inside SimHub Ambient Lights.
- Supports Philips Hue lamps and Govee segments exposed by SimHub.
- Supports `Global` output or `Cockpit zones / triples` output.
- In triples/wide captures, zones use the screen layout. On single-screen captures, zones adapt to the learned cockpit mask instead of blindly splitting the monitor.
- Zone choices include `All`, left/right inner and outer zones, center-left/right, lower cockpit, and lower cockpit left/center/right. `Left`, `Center` and `Right` remain available as combined outputs for simpler rigs.
- Provides per-game light sensitivity and response smoothing for games with softer or more abrupt brightness transitions.
- Lets each Hue lamp use its own cockpit brightness range.
- Lets each Govee device group share one cockpit brightness range while each segment remains selectable and zoned independently.
- Provides `Effects first` and `Full control of selected lights` priority modes.
- Provides idle colour/brightness for pause, replay, menus, no game, waiting for data or no learned mask while Full control is enabled.
- Provides a hold-idle-colour action for button/StreamDeck workflows.
- Includes built-in alert effects for engine start, racing flags, spotter, low fuel, redline, pit limiter, pedals, wheel lock and wheel spin.
- Lets each enabled endpoint choose which built-in alert effects it accepts, even when it is not selected for cockpit output.
- Includes optional VR fixed-colour mode using automatic experimental detection or a user-selected SimHub property.
- Includes dark mode support using local plugin state, Lovely Plugin true dark mode properties, or Daniel Newman Racing true dark mode properties, with per-endpoint dark mode selection.
- Adds normal SimHub control actions and Control Mapper roles for StreamDeck workflows.
- Provides diagnostics for game/car state, mask state, direct cockpit zones, lower cockpit zones, adaptive exposure, SimHub capture, runtime gates, VR state and debug logging.
- Includes an in-plugin updater and a standalone installer with backup/restore around DLL replacement.

## Requirements

- Windows with SimHub installed.
- SimHub Ambient Lights configured with Hue, Govee or another supported Ambient Lights output.
- SimHub Ambient Lights master output enabled.
- One unique SimHub Ambient Lights Effects index per lamp or segment.
- Cockpit camera usage in-game, preferably with camera shake/look-to-apex/head movement disabled or kept low.

## Installation

Recommended:

1. Download `Install-CockpitAutoAmbientLight.exe` from the latest release.
2. Run it.
3. Let it find SimHub and install the latest plugin DLL.
4. Restart SimHub if needed.

Manual:

1. Download `Cockpit-Auto-Ambient-Light.dll` from the latest release.
2. Close SimHub.
3. Copy the DLL into your SimHub install folder, usually `C:\Program Files (x86)\SimHub`.
4. Start SimHub and enable the plugin.

## Quick Setup

1. Configure your Hue, Govee or other lights in SimHub Ambient Lights first.
2. Check that each lamp or segment has a different Effects index in SimHub Ambient Lights.
3. Make sure SimHub Ambient Lights master output is enabled.
4. Open Cockpit Auto Ambient Light in SimHub.
5. Keep `Enable cockpit ambient lighting` enabled.
6. Open the Devices tab and select the lamps or Govee segments this plugin may control for cockpit output.
7. Choose `Global` or `Cockpit zones / triples` output.
8. Optional: enable `Full control of selected lights` if this plugin should own selected cockpit lights and provide idle/alert output.
9. Optional: configure alert effects, including effects-only unticked endpoints, VR mode and dark mode.
10. Drive in cockpit camera. For best learning, avoid heavy cockpit camera movement and large overlays over the cockpit view.
11. Use `Identify` on the Devices tab to confirm physical lamp or segment routing.

## Tabs Overview

### General

- `Enable cockpit ambient lighting`: turns this plugin output on/off without changing selected devices.
- `Toggle plugin button`: normal SimHub action binding for enabling/disabling the plugin.
- `Capture monitor`: uses SimHub's native Ambient Lights capture setup.
- `Output mode`: `Global` or `Cockpit zones / triples`.
- `Colour amount`: 0% is grayscale/brightness-only, 100% uses the sampled cockpit colour.
- `Game tuning`: per-game light sensitivity and response smoothing. Defaults are 100% sensitivity and 0% smoothing.
- `Full control of selected lights`: selected cockpit lights are owned by this plugin; current SimHub profile effects do not run on those selected lights, and built-in alert effects can also drive effects-only unticked endpoints.
- `Idle colour` and `Idle brightness`: fallback output while Full control is enabled and live cockpit output is not available.
- `Hold idle colour`: keeps selected lights on the idle colour until toggled off; unticked endpoints keep their fixed unselected output. Requires Full control.
- `Reset plugin output`: restarts SimHub Ambient Lights output if the output path appears stuck. The learned mask is kept.
- On enable, the plugin saves SimHub Ambient Lights scene brightness/gamma settings before applying its 100% brightness and neutral gamma defaults. On disable, it restores the saved settings. If those SimHub settings were changed while the plugin was active, the plugin keeps them and saves them as the new restore point.

### Devices

- Select Hue lamps and Govee segments that this plugin may control.
- Assign each selected endpoint to `All`, detailed cockpit zones, or lower cockpit zones when using `Cockpit zones / triples`.
- `Left`, `Center` and `Right` are combined outputs. Use `Left outer`, `Left inner`, `Center-left`, `Center-right`, `Right inner` and `Right outer` for direct sampled zones.
- `Lower cockpit` uses the lowest part of the learned cockpit mask. Use `Lower cockpit left`, `Lower cockpit center` and `Lower cockpit right` when you want that lower area split by side.
- Hue lamps have their own cockpit brightness range.
- Govee device groups share one cockpit brightness range, but each segment can still be selected, identified and zoned separately.
- `Effects` controls per-endpoint dark mode and which built-in alert effects each endpoint accepts. Alert effects require Full control; dark mode can also run on unselected lights.
- `Unselected SimHub lights` sets the fixed colour/brightness for Ambient Lights devices enabled in SimHub but not selected in this plugin.
- SimHub Ambient Lights Effects indexes must be unique per lamp/segment. Duplicated indexes can make endpoints mirror each other or ignore different zone assignments.

### Effects

Built-in alert effects run only when `Full control of selected lights` is enabled. They can be assigned to selected cockpit endpoints or unticked effects-only endpoints.

Locked highest priority:

1. Pit limiter.
2. Engine start.

Custom priority list:

1. Spotter.
2. Racing flags.
3. Wheel lock.
4. Wheel spin.
5. Low fuel.
6. Redline.
7. Pedals.

Available effects:

- `Alert brightness`: shared brightness scale for built-in alert effects.
- `Engine start`: Warm afterglow, Hard flash or Ignition burst animation.
- `Racing flags`: per-flag enable, Static/Blink/Pulse style, speed, duration and flag priority.
- `Spotter`: one colour/speed, with separate left/right acceptance per lamp.
- `Low fuel`: percent or liters, low/critical thresholds, Static or Blink per stage, plus acknowledge action.
- `Redline`: uses SimHub's current car/current gear redline automatically.
- `Pit limiter`: alternates between two colours. `All lamps together` is the default and works with any set of lamps; `Use cockpit zones` starts left-side zones on colour 1 and right-side zones on colour 2, then swaps them each blink.
- `Pedals`: throttle and brake colours, either static or brightness-scaled by pedal travel. Brake has priority.
- `Wheel lock`: ABS active when available, with calculated wheel lock while braking as fallback.
- `Wheel spin`: TC active when available, with calculated wheel spin while accelerating as fallback.

### Overrides

- `VR mode`: selected lights use a fixed VR colour/brightness while the chosen VR detector reports VR active.
- `Automatic experimental` VR detection checks OpenXR Toolkit and SteamVR/OpenVR.
- `SimHub property` VR detection is best when another plugin or workflow exposes a reliable VR state.
- `Dark mode`: uses a local plugin colour/state or external Lovely Plugin / Daniel Newman Racing true dark mode properties.
- Dark mode uses a steady base colour and can layer real cockpit light over it when brightness rises, so modest cockpit highlights are not hidden by the base colour.
- The Devices tab decides which selected or unselected endpoints accept dark mode output.

### Diagnostics

- Shows current game/car, mask state, signal summary, direct zone brightness, lower cockpit zone brightness, exposure, gain and mask blocks.
- Shows SimHub Ambient Lights output state, capture source, capture override state, selected light count and runtime gates.
- Includes `Reset learned mask`, which clears the stored mask for the current game/car and starts learning again while driving.
- Includes optional debug logging to `Documents\CockpitAmbientLight\Logs\plugin.log`.

## Button Bindings and StreamDeck

Normal SimHub action bindings are available from the plugin UI for:

- Toggle plugin.
- Reset plugin output.
- Reset learned mask.
- Hold idle colour.
- Toggle dark mode.
- Acknowledge low fuel.

For StreamDeck workflows using SimHub Control Mapper roles, use these role names:

- `Cockpit Ambient - Toggle plugin`
- `Cockpit Ambient - Reset output`
- `Cockpit Ambient - Reset mask`
- `Cockpit Ambient - Hold idle colour`
- `Cockpit Ambient - Toggle dark mode`
- `Cockpit Ambient - Acknowledge low fuel`

## Capture Notes

The plugin depends on SimHub Ambient Lights receiving live capture frames. If SimHub's native capture freezes, the plugin cannot learn or update cockpit lighting from new frames until SimHub capture becomes live again.

Some games can block native screen capture in fullscreen mode. If capture freezes after entering the track, check the game executable Compatibility settings and leave `Disable fullscreen optimizations` off. Borderless/windowed mode is the usual fallback when a game still blocks capture.

For best mask learning, use a stable cockpit camera. Camera shake, look-to-apex/head movement and large HUD or overlay elements over the cockpit view can confuse the detector because they change what appears stable inside the captured cockpit area.

## Troubleshooting

- Lights do not react: confirm the lights are enabled in SimHub Ambient Lights and selected for cockpit output or at least one alert effect in this plugin.
- Wrong physical lamp/segment: use `Identify` in the Devices tab.
- Mask never becomes active or looks wrong: drive above walking speed in cockpit camera, use a stable cockpit camera and keep large overlays away from the cockpit area when possible.
- Other SimHub effects hide cockpit light: use Full control or disable continuous effects on the same lamps.
- Multiple lamps behave the same in zones: check that each lamp or segment has a unique Effects index in SimHub Ambient Lights.
- Output appears stuck: use `Reset plugin output` first. Use `Reset learned mask` only if the mask itself is wrong.
- Triples/screen zones look too colourful: lower `Colour amount` to keep brightness response while reducing colour contamination.

## Updates

The in-plugin updater downloads the new DLL, backs up the current DLL, replaces it, and asks SimHub to restart. If replacement fails, it attempts to restore the previous DLL.

The standalone installer also backs up the current DLL before replacing it.

## Full Guide

See [GUIDE.md](GUIDE.md) for a more detailed user guide.

## Support

Created by Bruno Silva.

For questions, problems or setup help:

- Email: bruno.1978@gmail.com
- GitHub issues: https://github.com/BrunoSilva1978PT/Cockpit-Auto-Ambient-Light-public/issues
- Discord: Bruno Silva (username: brunosilva1978)

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin
