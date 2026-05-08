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
- Maps the learned cockpit brightness into per-device brightness ranges.
- Drives selected Philips Hue lamps and Govee segments already exposed by SimHub Ambient Lights.
- Supports global lighting or screen-thirds output for triples.
- Lists Govee devices grouped by SimHub controller/device, with each segment selectable and identifiable independently.
- Supports per-lamp brightness ranges for Philips Hue and per-device brightness ranges for Govee segment groups.
- Includes dark mode support using local settings, Lovely Plugin true dark mode properties, or Daniel Newman Racing true dark mode properties.
- Lets the user scale the dark-mode base colour separately from cockpit highlights and per-device brightness ranges.
- Writes a base lighting layer below higher-priority SimHub colour effects, so temporary alerts and notification plugins can still take control.
- Can optionally prioritize cockpit lighting while dark mode is active when fixed external dark mode effects hide cockpit brightness changes.
- Can optionally force cockpit lighting above SimHub effects when a continuous external effect hides this plugin.

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
4. Keep `Enable cockpit ambient lighting` enabled in the Main tab.
5. Use the Devices tab to select the lights or Govee segments the plugin may control.
6. Adjust the brightness range per Hue lamp or per Govee device group if needed.
7. Drive in cockpit camera. The plugin learns the mask automatically.
8. Use `Identify` if you need to confirm the physical light or segment routing.

## Main Controls

- `Enable cockpit ambient lighting`: turns this plugin's output on or off without changing SimHub Ambient Lights settings or device selections.
- `Output mode`: use `Global` for one shared cockpit signal, or `Screen thirds / triples` to assign lamps to `All`, `Left`, `Center` or `Right` cockpit zones.
- `Colour amount`: controls how much sampled cockpit colour reaches the lamps. Lower values move output toward grayscale/brightness-only response.
- `Unselected SimHub lights`: sets the fixed colour and intensity for Ambient Light devices enabled in SimHub but not selected in this plugin.
- `Reset learned mask`: fallback button for cases where automatic mask learning clearly picked the wrong cockpit area.

## Devices Tab

- Philips Hue rows expose one brightness range per physical lamp.
- Govee rows are grouped by SimHub controller/device and expose one brightness range for the whole Govee device group.
- Govee segments remain selectable and identifiable independently.
- In `Screen thirds / triples`, each selected Hue lamp or Govee segment can be assigned to `All`, `Left`, `Center` or `Right`.
- New Hue lamps and Govee device groups start at `0%` minimum and `100%` maximum.

## Govee Devices

Govee support follows what SimHub Ambient Lights exposes. The plugin does not talk directly to Govee devices or discover models by itself.

SimHub exposes Govee devices by controller/device and creates one capture/effect endpoint per segment. This plugin keeps that structure visible:

- The Devices tab groups Govee rows by SimHub controller/device.
- Each Govee segment is listed as its own selectable endpoint.
- Each segment has its own `Identify` action.
- The brightness range is shared by the Govee device group, so all segments from that device keep the same physical min/max.
- In `Screen thirds / triples`, each segment can be assigned to `All`, `Left`, `Center` or `Right`.
- In `Global`, selected segments follow the combined cockpit signal.

This avoids guessing physical layouts. For example, some light-bar kits are two physical bars under one controller, while strips, ropes and panels expose different segment layouts.

## Effects Priority

Cockpit Auto Ambient Light writes a base lighting layer through SimHub's colour effects profile. This is intentional: normal SimHub effects with higher priority can overlay it.

Good examples of effects that can stay enabled:

- Yellow/blue/red flags.
- Low fuel or pit limiter warnings.
- Spotter, race control or notification flashes.
- Short notification effects.

If another plugin or profile runs a constant ambient-light effect on the same lamps, that effect can hide this plugin all the time. Disable those continuous ambient effects in that profile/plugin, keep only temporary alerts, or enable `Force cockpit lighting above SimHub effects` from the Main tab.

`Force cockpit lighting above SimHub effects` is a global priority override for selected lights. Use it only when another continuous effect keeps this plugin hidden. Pit limiter, flags and other alerts on those selected lights may be hidden while it is enabled.

When `Prioritize cockpit lighting during dark mode` is enabled, this priority is reversed only while dark mode is active. Cockpit lighting can then win over fixed external dark mode colours, but normal effects on the same lights may be hidden until dark mode turns off. If the global force option is enabled, the dark-mode-only priority option is already covered.

## Dark Mode

Dark mode is intended to imitate a steady endurance-style cockpit light, such as a red, cyan, purple or amber glow inside the cabin.

It can be controlled locally by this plugin or synced from Lovely Plugin / Daniel Newman Racing true dark mode properties. There is no separate dark-mode enable switch because external SimHub effects can still drive the same lights independently. When active, the selected dark mode colour is used as the base, and cockpit light blends over it when brightness is high enough.

`Base colour brightness` scales only the steady dark-mode base colour used by this plugin. Cockpit highlights, whites and brighter areas are still added by the capture blend, then each device min/max range is applied.

`Cockpit blend start` is based on the plugin's adaptive cockpit light output before each device applies its own min/max range:

- Lower values let real cockpit light start blending over the dark mode colour sooner.
- Higher values keep the dark mode colour dominant for longer.

`Prioritize cockpit lighting during dark mode` changes effect priority only while dark mode is active:

- Off: normal SimHub colour effects stay above this plugin. This preserves alert effects, but fixed external dark mode effects can hide cockpit brightness changes.
- On: this plugin moves above normal effects during dark mode so its dynamic dark-mode lighting remains visible. Compromise: normal effects on the same lights may not show until dark mode turns off.

External dark mode properties are read at a low rate for this feature; they are not polled every frame.

## Logging

Debug logging is disabled by default. Enable it from the Main tab only when troubleshooting.

When enabled, the plugin writes a single startup-cleared file at:

`Documents\CockpitAmbientLight\Logs\plugin.log`

The log focuses on SimHub bridge state and Govee discovery/selection details so Govee users can send useful diagnostics without large mask-preview logs.

## Updates

The in-plugin updater downloads the new DLL, replaces the current plugin DLL using the same direct self-update pattern used by the companion notification plugins, then asks SimHub to restart.

## Notes

The plugin stores the last accepted cockpit mask per game/car in SimHub settings. Stored masks are used only as startup seeds; the plugin keeps validating a candidate mask while driving so FOV, seat position and cockpit-camera changes can replace an old mask automatically.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin
