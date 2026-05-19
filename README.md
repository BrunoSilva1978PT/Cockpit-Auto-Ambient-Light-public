# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's native Ambient Lights screen capture and drives selected Philips Hue, Govee and other SimHub Ambient Lights devices so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains release binaries and user documentation. The plugin source code is private.

## Current Features

- Uses SimHub's native Ambient Lights capture engine; it does not open a second screen-capture pipeline.
- Learns a cockpit mask automatically while you drive in cockpit camera.
- Stores the last accepted cockpit mask per game/car and reuses it on the next session.
- Keeps checking a candidate mask in the background for FOV, seat position and cockpit-camera changes.
- Drives lights already configured and enabled inside SimHub Ambient Lights.
- Supports Philips Hue lamps and Govee segments exposed by SimHub.
- Supports `Global` output or `Screen thirds / triples` output.
- In triples/wide captures, zones use the screen layout. On single-screen captures, zones adapt to the learned cockpit mask instead of blindly splitting the monitor.
- Zone choices are `All`, `Left`, `Center-left`, `Center`, `Center-right` and `Right`. `Center` combines both center halves.
- Lets each Hue lamp use its own cockpit brightness range.
- Lets each Govee device group share one cockpit brightness range while each segment remains selectable and zoned independently.
- Provides `Effects first` and `Full control of selected lights` priority modes.
- Provides idle colour/brightness for pause, replay, menus, no game, waiting for data or no learned mask while Full control is enabled.
- Provides a hold-idle-colour action for button/StreamDeck workflows.
- Includes built-in alert effects for engine start, racing flags, spotter, low fuel, redline and pit limiter.
- Lets each selected lamp choose which built-in alert effects it accepts.
- Includes optional VR fixed-colour mode using automatic experimental detection or a user-selected SimHub property.
- Includes dark mode support using local plugin state, Lovely Plugin true dark mode properties, or Daniel Newman Racing true dark mode properties.
- Adds normal SimHub control actions and Control Mapper roles for StreamDeck workflows.
- Provides diagnostics for game/car state, mask state, brightness, adaptive exposure, SimHub capture, runtime gates, VR state and debug logging.
- Includes an in-plugin updater and a standalone installer with backup/restore around DLL replacement.

## Requirements

- Windows with SimHub installed.
- SimHub Ambient Lights configured with Hue, Govee or another supported Ambient Lights output.
- SimHub Ambient Lights master output enabled.
- Cockpit camera usage in-game.

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
2. Make sure SimHub Ambient Lights master output is enabled.
3. Open Cockpit Auto Ambient Light in SimHub.
4. Keep `Enable cockpit ambient lighting` enabled.
5. Open the Devices tab and select the lamps or Govee segments this plugin may control.
6. Choose `Global` or `Screen thirds / triples` output.
7. Optional: enable `Full control of selected lights` if this plugin should own selected lights and provide idle/alert output.
8. Optional: configure alert effects, VR mode and dark mode.
9. Drive in cockpit camera. The cockpit mask learns automatically while the car moves.
10. Use `Identify` on the Devices tab to confirm physical lamp or segment routing.

## Tabs Overview

### General

- `Enable cockpit ambient lighting`: turns this plugin output on/off without changing selected devices.
- `Toggle plugin button`: normal SimHub action binding for enabling/disabling the plugin.
- `Capture monitor`: uses SimHub's native Ambient Lights capture setup.
- `Output mode`: `Global` or `Screen thirds / triples`.
- `Colour amount`: 0% is grayscale/brightness-only, 100% uses the sampled cockpit colour.
- `Full control of selected lights`: selected lights are owned by this plugin; current SimHub profile effects do not run on those selected lights.
- `Idle colour` and `Idle brightness`: fallback output while Full control is enabled and live cockpit output is not available.
- `Hold idle colour`: keeps selected lights on the idle colour until toggled off. Requires Full control.
- `Reset plugin output`: restarts SimHub Ambient Lights output if the output path appears stuck. The learned mask is kept.

### Devices

- Select Hue lamps and Govee segments that this plugin may control.
- Assign each selected endpoint to `All`, `Left`, `Center-left`, `Center`, `Center-right` or `Right` when using `Screen thirds / triples`.
- `Center` follows both center halves together for backwards-compatible center output.
- Hue lamps have their own cockpit brightness range.
- Govee device groups share one cockpit brightness range, but each segment can still be selected, identified and zoned separately.
- `Alert effects` controls which built-in alert effects each selected lamp accepts while Full control is enabled.
- `Unselected SimHub lights` sets the fixed colour/brightness for Ambient Lights devices enabled in SimHub but not selected in this plugin.

### Effects

Built-in alert effects run only when `Full control of selected lights` is enabled.

Locked highest priority:

1. Pit limiter.
2. Engine start.

Custom priority list:

1. Spotter.
2. Racing flags.
3. Low fuel.
4. Redline.

Available effects:

- `Alert brightness`: shared brightness scale for built-in alert effects.
- `Engine start`: Warm afterglow, Hard flash or Ignition burst animation.
- `Racing flags`: per-flag enable, Static/Blink/Pulse style, speed, duration and flag priority.
- `Spotter`: one colour/speed, with separate left/right acceptance per lamp.
- `Low fuel`: percent or liters, low/critical thresholds, Static or Blink per stage, plus acknowledge action.
- `Redline`: uses SimHub's current car/current gear redline automatically.
- `Pit limiter`: alternates between two colours.

### Overrides

- `VR mode`: selected lights use a fixed VR colour/brightness while the chosen VR detector reports VR active.
- `Automatic experimental` VR detection checks OpenXR Toolkit and SteamVR/OpenVR.
- `SimHub property` VR detection is best when another plugin or workflow exposes a reliable VR state.
- `Dark mode`: uses a local plugin colour/state or external Lovely Plugin / Daniel Newman Racing true dark mode properties.
- Dark mode uses a steady base colour and can blend real cockpit light over it when brightness rises.

### Diagnostics

- Shows current game/car, mask state, signal summary, zone brightness, zone exposure and mask blocks.
- Shows SimHub Ambient Lights output state, capture source, capture override state, selected light count and runtime gates.
- Includes `Reset learned mask`, which clears the stored mask for the current game/car and starts learning again while driving.
- Includes optional debug logging to `Documents\CockpitAmbientLight\Logs\plugin.log`.

## Button Bindings and StreamDeck

Normal SimHub action bindings are available from the plugin UI for:

- Toggle plugin.
- Reset learned mask.
- Hold idle colour.
- Toggle dark mode.
- Acknowledge low fuel.

For StreamDeck workflows using SimHub Control Mapper roles, use these role names:

- `Cockpit Auto Ambient Light - Toggle plugin`
- `Cockpit Auto Ambient Light - Reset learned mask`
- `Cockpit Auto Ambient Light - Hold idle colour`
- `Cockpit Auto Ambient Light - Toggle dark mode`
- `Cockpit Auto Ambient Light - Acknowledge low fuel`

## Capture Notes

The plugin depends on SimHub Ambient Lights receiving live capture frames. If SimHub's native capture freezes, the plugin cannot learn or update cockpit lighting from new frames until SimHub capture becomes live again.

Some games can block native screen capture in fullscreen mode. If capture freezes after entering the track, check the game executable Compatibility settings and leave `Disable fullscreen optimizations` off. Borderless/windowed mode is the usual fallback when a game still blocks capture.

## Troubleshooting

- Lights do not react: confirm the lights are enabled in SimHub Ambient Lights and selected in this plugin.
- Wrong physical lamp/segment: use `Identify` in the Devices tab.
- Mask never becomes active: drive above walking speed in cockpit camera.
- Other SimHub effects hide cockpit light: use Full control or disable continuous effects on the same lamps.
- Output appears stuck: use `Reset plugin output` first. Use `Reset learned mask` only if the mask itself is wrong.
- Triples/screen zones look too colourful: lower `Colour amount` to keep brightness response while reducing colour contamination.

## Updates

The in-plugin updater downloads the new DLL, backs up the current DLL, replaces it, and asks SimHub to restart. If replacement fails, it attempts to restore the previous DLL.

The standalone installer also backs up the current DLL before replacing it.

## Full Guide

See [GUIDE.md](GUIDE.md) for a more detailed user guide.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin
