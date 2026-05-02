# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's native Ambient Lights screen capture and drives selected Philips Hue, Govee and other SimHub Ambient Lights devices so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains releases only. The plugin source code is private.

## What It Does

- Learns a cockpit mask automatically while you drive in cockpit camera.
- Uses SimHub's native Ambient Lights capture engine instead of opening a second screen-capture pipeline.
- Reuses lights already configured in SimHub Ambient Lights; it does not discover Hue or Govee devices directly.
- Keeps adapting a candidate mask in the background for FOV, seat position and cockpit-camera changes.
- Resets the mask after a stable game/car change.
- Maps the learned cockpit brightness range into the user-selected lamp output range.
- Drives selected Philips Hue lamps and Govee segments already exposed by SimHub Ambient Lights.
- Supports global lighting or screen-thirds output for triples.
- Lists Govee devices grouped by SimHub controller/device, with each segment selectable and identifiable independently.
- Includes dark mode support using local settings, Lovely Plugin true dark mode properties, or Daniel Newman Racing true dark mode properties.
- Writes a base lighting layer below higher-priority SimHub colour effects, so temporary alerts and notification plugins can still take control.

## Requirements

- SimHub installed on Windows.
- Cockpit camera usage in-game.
- SimHub Ambient Lights configured with Philips Hue, Govee or other supported devices.
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

1. Configure your lights in SimHub Ambient Lights first.
2. Make sure the SimHub Ambient Lights master output is enabled.
3. Open the plugin settings in SimHub.
4. Use the Devices tab to select the lights or Govee segments the plugin may control.
5. Drive in cockpit camera. The plugin learns the mask automatically.
6. Use `Identify` if you need to confirm the physical light or segment routing.

## Main Controls

- `Output mode`: use `Global` for one shared cockpit signal, or `Screen thirds / triples` to assign lamps to `All`, `Left`, `Center` or `Right` cockpit zones.
- `Colour amount`: controls how much sampled cockpit colour reaches the lamps. Lower values move output toward grayscale/brightness-only response.
- `Lamp minimum` and `Lamp maximum`: define the physical brightness range the adaptive exposure mapper may use.
- `Unselected SimHub lights`: sets the fixed colour and intensity for Ambient Light devices enabled in SimHub but not selected in this plugin.
- `Reset learned mask`: fallback button for cases where automatic mask learning clearly picked the wrong cockpit area.

## Govee Devices

Govee support follows what SimHub Ambient Lights exposes. The plugin does not talk directly to Govee devices or discover models by itself.

SimHub exposes Govee devices by controller/device and creates one capture/effect endpoint per segment. This plugin keeps that structure visible:

- The Devices tab groups Govee rows by SimHub controller/device.
- Each Govee segment is listed as its own selectable endpoint.
- Each segment has its own `Identify` action.
- In `Screen thirds / triples`, each segment can be assigned to `All`, `Left`, `Center` or `Right`.
- In `Global`, selected segments follow the combined cockpit signal.

This avoids guessing physical layouts. For example, some light-bar kits are two physical bars under one controller, while strips, ropes and panels expose different segment layouts.

## Effects Priority

Cockpit Auto Ambient Light writes a base lighting layer through SimHub's colour effects profile. This is intentional: normal SimHub effects with higher priority can overlay it.

Good examples of effects that can stay enabled:

- Yellow/blue/red flags.
- Low fuel or pit limiter warnings.
- Spotter, race control or notification flashes.
- Short WhatsApp/notification effects.

If another plugin or profile runs a constant ambient-light effect on the same lamps, that effect can hide this plugin all the time. Disable those continuous ambient effects in that profile/plugin, or keep only temporary alerts. Examples include DNR, ATSR-HUB, or any custom SimHub profile effect that constantly drives the same lights.

## Dark Mode

Dark mode is intended to imitate a steady endurance-style cockpit light, such as a red, cyan, purple or amber glow inside the cabin.

It can be controlled locally by this plugin or synced from Lovely Plugin / Daniel Newman Racing true dark mode properties. When active, the selected dark mode colour stays dominant until cockpit brightness is high enough to blend over it.

`Cockpit blend start` is based on lamp output brightness, not raw mask percentage:

- Lower values let real cockpit light start blending over the dark mode colour sooner.
- Higher values keep the dark mode colour dominant for longer.
- `Keep dark mode colour fixed` disables cockpit blending while dark mode is active.

External dark mode properties are read at a low rate for this feature; they are not polled every frame.

## Logging

Debug logging is disabled by default. Enable it from the Main tab only when troubleshooting.

When enabled, the plugin writes a single startup-cleared file at:

`Documents\CockpitAmbientLight\Logs\plugin.log`

The log focuses on SimHub bridge state and Govee discovery/selection details so Govee users can send useful diagnostics without large mask-preview logs.

## Updates

The in-plugin updater downloads the new DLL, replaces the current plugin DLL using the same direct self-update pattern used by the companion notification plugins, then asks SimHub to restart.

## Notes

The plugin does not store cockpit masks on disk. It starts fresh each SimHub session because cockpit view, FOV, resolution and car layout can change. The mask is fast to learn and continuously adapts while driving.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin
