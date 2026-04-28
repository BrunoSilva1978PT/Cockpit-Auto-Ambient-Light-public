# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's native Ambient Lights screen capture and drives selected Philips Hue and Govee devices so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains releases only. The plugin source code is private.

## What It Does

- Learns a cockpit mask automatically while you drive in cockpit camera.
- Uses SimHub's native Ambient Lights capture engine instead of opening a second screen-capture pipeline.
- Keeps adapting a candidate mask in the background for FOV, seat position and cockpit-camera changes.
- Resets the mask after a stable game/car change.
- Maps the learned cockpit brightness range into the user-selected lamp output range.
- Drives Philips Hue and Govee devices already configured in SimHub Ambient Lights.
- Supports global lighting or screen-thirds output for triples.
- Keeps Govee multi-segment devices simple: each device is listed once and receives one colour across its segments.
- Includes dark mode support using local settings or Lovely Plugin true dark mode properties.
- Runs below higher-priority SimHub effect layers, so notification plugins can still take control.

## Requirements

- SimHub installed on Windows.
- Cockpit camera usage in-game.
- SimHub Ambient Lights configured with Philips Hue and/or Govee devices.
- SimHub Ambient Lights master output enabled.

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

1. Configure your Hue/Govee devices in SimHub Ambient Lights.
2. Make sure the SimHub Ambient Lights master output is enabled.
3. Open the plugin settings in SimHub.
4. Use the Devices tab to select the lights the plugin may control.
5. Drive in cockpit camera. The plugin learns the mask automatically.
6. Use `Identify` on selected devices if you need to confirm the light routing.

## Main Controls

- `Output mode`: use `Global` for one shared cockpit signal, or `Screen thirds / triples` to assign lamps to left, center and right cockpit zones.
- `Lamp minimum` and `Lamp maximum`: define the physical brightness range the adaptive exposure mapper may use.
- `Protect zone colours`: in triples mode, helps side lamps avoid strong wall, grass, kerb or advertising colours.
- `Reset learned mask`: fallback button for cases where automatic mask learning clearly picked the wrong cockpit area.

## Dark Mode

Dark mode is intended to imitate a steady endurance-style cockpit light, such as a red, cyan, purple or amber glow inside the cabin.

It can be controlled locally by this plugin or synced from Lovely Plugin true dark mode properties. When active, the selected dark mode colour stays dominant until cockpit brightness is high enough to blend over it.

`Cockpit blend start` is based on lamp output brightness, not raw mask percentage:

- Lower values let real cockpit light start blending over the dark mode colour sooner.
- Higher values keep the dark mode colour dominant for longer.
- `Keep dark mode colour fixed` disables cockpit blending while dark mode is active.

Lovely Plugin properties are read at a low rate for this feature; they are not polled every frame.

## Logging

Debug logging is disabled by default. Enable it from the Main tab only when troubleshooting.

When enabled, the plugin writes a single startup-cleared file at:

`Documents\CockpitAmbientLight\Logs\plugin.log`

The log focuses on SimHub bridge state and Govee discovery/selection details so Govee users can send useful diagnostics without large mask-preview logs.

## Updates

The in-plugin updater stages the new DLL and applies it after SimHub closes, then restarts SimHub. This avoids replacing the plugin DLL while SimHub still has it loaded.

## Notes

The plugin does not store cockpit masks on disk. It starts fresh each SimHub session because cockpit view, FOV, resolution and car layout can change. The mask is fast to learn and continuously adapts while driving.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin