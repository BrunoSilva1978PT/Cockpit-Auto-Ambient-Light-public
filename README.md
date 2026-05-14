# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's native Ambient Lights screen capture and drives selected Philips Hue, Govee and other SimHub Ambient Lights devices so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains releases only. The plugin source code is private.

## What It Does

- Learns a cockpit mask automatically while you drive in cockpit camera.
- Uses SimHub's native Ambient Lights capture engine instead of opening a second screen-capture pipeline.
- Reuses lights already configured in SimHub Ambient Lights; it does not discover Hue or Govee devices directly.
- Stores the last accepted cockpit mask per game/car and reuses it as a startup seed.
- Keeps validating a candidate mask in the background for FOV, seat position and cockpit-camera changes.
- Saves the current mask on game/car changes, pause/runtime suspend and SimHub shutdown.
- Maps the learned cockpit brightness into per-device cockpit brightness ranges.
- Drives selected Philips Hue lamps and Govee segments already exposed by SimHub Ambient Lights.
- Supports global lighting or screen-thirds output for triples.
- Lists Govee devices grouped by SimHub controller/device, with each segment selectable, identifiable and zoned independently.
- Supports Full control mode, where this plugin fully owns selected lights and current SimHub profile effects no longer run on those selected lights.
- Provides an idle colour and brightness for pause, replay, menus, no game or before a cockpit mask is ready while Full control is enabled.
- Includes built-in alert effects for racing flags, spotter, low fuel, redline, pit limiter and engine start.
- Lets each selected lamp choose which built-in alert effects it accepts.
- Includes optional VR fixed-colour mode using automatic experimental detection or a user-selected SimHub property.
- Includes dark mode support using local settings, Lovely Plugin true dark mode properties, or Daniel Newman Racing true dark mode properties.
- Provides diagnostics for runtime gates, speed gates, capture frames, VR state, brightness signals and debug logging.

## Requirements

- SimHub installed on Windows.
- Cockpit camera usage in-game.
- SimHub Ambient Lights configured with Philips Hue, Govee or other supported devices.
- SimHub Ambient Lights master output enabled.

## Screen Capture Notes

The plugin uses SimHub's native Ambient Lights screen capture. If that capture freezes, the plugin cannot learn or update cockpit lighting from new frames until SimHub receives live capture again.

Some games can block or freeze native screen capture when running in fullscreen mode. If SimHub Ambient Lights capture freezes after entering the track, check the game executable Compatibility settings and make sure `Disable fullscreen optimizations` is not enabled. Borderless/windowed mode is the usual fallback when a game still blocks capture.

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

## First Setup

1. Configure your lights in SimHub Ambient Lights first.
2. Make sure the SimHub Ambient Lights master output is enabled.
3. Open the plugin settings in SimHub.
4. Keep `Enable cockpit ambient lighting` enabled in the General tab.
5. Use the Devices tab to select the lights or Govee segments the plugin may control.
6. Adjust the cockpit brightness range per Hue lamp or per Govee device group if needed.
7. If you use Full control, choose which alert effects each lamp accepts on the Devices tab.
8. Optional: tune built-in alerts on the Effects tab.
9. Drive in cockpit camera. The plugin learns the mask automatically.
10. Use `Identify` if you need to confirm the physical light or segment routing.

## General Tab

- `Enable cockpit ambient lighting`: turns this plugin's output on or off without changing SimHub Ambient Lights settings or device selections.
- `Capture monitor`: follows the SimHub Ambient Lights capture setup and lets the plugin use the same native capture path.
- `Output mode`: use `Global` for one shared cockpit signal, or `Screen thirds / triples` to assign lamps to `All`, `Left`, `Center` or `Right` cockpit zones.
- `Colour amount`: controls how much sampled cockpit colour reaches the lamps. Lower values move output toward grayscale/brightness-only response.
- `Full control of selected lights`: makes this plugin fully control selected lights. Current SimHub Ambient Lights profile effects will not run on those selected lights. Built-in alert effects from the Effects tab can still override cockpit or dark-mode output per lamp.
- `Idle colour` and `Idle brightness`: fixed output used during pause, replay, menus, no game, waiting for data, or before a cockpit mask is ready while Full control is enabled. Stopped cars keep the current cockpit output.
- `Reset learned mask`: fallback button for cases where automatic mask learning clearly picked the wrong cockpit area.

## Devices Tab

- Philips Hue rows expose one cockpit brightness range per physical lamp.
- Govee rows are grouped by SimHub controller/device and expose one cockpit brightness range for the whole Govee device group.
- Govee segments remain selectable, identifiable and zoned independently.
- In `Screen thirds / triples`, each selected Hue lamp or Govee segment can be assigned to `All`, `Left`, `Center` or `Right`.
- `Cockpit brightness range` affects only cockpit/dynamic output. Idle colour, VR fixed colour and built-in alert effects use their own brightness controls.
- `Alert effects` decides which built-in alerts each lamp is allowed to show while Full control is enabled.
- `Unselected SimHub lights` sets the fixed colour and brightness for Ambient Light devices enabled in SimHub but not selected in this plugin.

## Effects Tab

Built-in alert effects run only when `Full control of selected lights` is enabled on the General tab. Each lamp decides which alerts it accepts on the Devices tab.

Per-lamp alert priority is:

1. Pit limiter.
2. Engine start.
3. Spotter left/right.
4. Racing flags.
5. Low fuel.
6. Redline.
7. Normal cockpit or dark-mode output.

Available alert effects:

- `Alert brightness`: shared brightness scale for all built-in alert effects.
- `Engine start`: short Pulse, Alternating or Triple burst animation when SimHub reports the engine starting. The preview plays the full animation.
- `Racing flags`: configure each flag separately. Each flag can be disabled, Static, Blink or Pulse, and can optionally show only for a fixed duration before staying hidden until the sim clears and raises that flag again.
- `Spotter`: one colour and blink speed, with separate per-lamp acceptance for left and right spotter alerts on the Devices tab.
- `Low fuel`: percent or liters reference, separate low/critical thresholds, and Static or Blink style per stage.
- `Redline`: uses SimHub's current car/current gear redline automatically.
- `Pit limiter`: alternates between two user colours.

Flag priority is yellow, green, blue, checkered, white, orange.

## Overrides Tab

Overrides temporarily replace the live cockpit colour when a specific mode is active.

### VR Mode

When automatic VR detection is enabled, selected lights use the fixed VR colour and brightness while SimHub has an active game and the selected VR source reports VR active.

Detection sources:

- `Automatic experimental`: checks OpenXR Toolkit's running app registry value and SteamVR/OpenVR's current scene process.
- `SimHub property`: uses a user-selected SimHub property and compares it with user-defined ON/OFF values. This is the most reliable option when another SimHub plugin or button workflow can expose the VR state.

Special SimHub property values:

- `<any>`: any non-empty value.
- `<null>`: missing or null.
- `<empty>`: empty text.

VR fixed output uses its own colour and brightness. It does not use each device cockpit brightness range.

### Dark Mode

Dark mode is intended to imitate a steady endurance-style cockpit light, such as a red, cyan, purple or amber glow inside the cabin.

It can be controlled locally by this plugin or synced from Lovely Plugin / Daniel Newman Racing true dark mode properties. There is no separate dark-mode enable switch because external SimHub effects can still drive the same lights independently. When active, the selected dark mode colour is used as the base, and cockpit light blends over it when brightness is high enough.

`Base colour brightness` scales only the steady dark-mode base colour used by this plugin. Built-in alert effects still have priority while Full control is enabled.

`Cockpit blend start` is based on the plugin's adaptive cockpit light output before each device applies its own min/max range:

- Lower values let real cockpit light start blending over the dark mode colour sooner.
- Higher values keep the dark mode colour dominant for longer.

`Prioritize cockpit lighting during dark mode` changes effect priority only while dark mode is active:

- Off: normal SimHub colour effects stay above this plugin. This preserves normal SimHub effects, but fixed external dark mode effects can hide cockpit brightness changes.
- On: this plugin moves above normal effects during dark mode so its dynamic dark-mode lighting remains visible. Compromise: normal effects on the same lights may not show until dark mode turns off.

If `Full control of selected lights` is enabled, cockpit lighting already has priority globally, so the dark-mode-only priority control is disabled.

External dark mode properties are read at a low rate for this feature; they are not polled every frame.

## Diagnostics Tab

The Diagnostics tab shows the current game/car state, mask state, brightness values, adaptive exposure, SimHub Ambient Lights bridge state, runtime gate, speed gate, learning gate, capture frame counter and VR detection state.

Use this tab when the plugin does not leave idle, does not learn a mask, or SimHub capture appears frozen.

## Logging

Debug logging is disabled by default. Enable it from the Diagnostics tab only when troubleshooting.

When enabled, the plugin writes a single startup-cleared file at:

`Documents\CockpitAmbientLight\Logs\plugin.log`

The log focuses on SimHub bridge state, capture state and Govee discovery/selection details so users can send useful diagnostics without large mask-preview logs.

## Fullscreen Capture Note

This plugin uses SimHub's native Ambient Lights screen capture. If that capture freezes when a game enters the track in fullscreen mode, check the game executable Compatibility settings and leave `Disable fullscreen optimizations` off. Borderless/windowed mode is the usual fallback when a game blocks native screen capture.

## Updates

The in-plugin updater downloads the new DLL, backs up the current DLL, replaces it, and asks SimHub to restart. If replacement fails, the plugin can restore the backup.

## Notes

The plugin stores the last accepted cockpit mask per game/car in SimHub settings. Stored masks are used only as startup seeds; the plugin keeps validating a candidate mask while driving so FOV, seat position and cockpit-camera changes can replace an old mask automatically.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin
