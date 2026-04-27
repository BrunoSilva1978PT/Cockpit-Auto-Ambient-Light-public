# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin for automatic cockpit ambient lighting. It learns the stable cockpit area from SimHub's screen capture and drives selected Philips Hue and Govee Ambient Lights so the physical cockpit reacts to in-game sun, shadow and night lighting.

This public repository contains releases only. The plugin source code is private.

## What It Does

- Learns a cockpit mask automatically while you drive in cockpit camera.
- Keeps adapting the mask in the background for FOV, seat position and camera changes.
- Resets the mask after a stable game/car change.
- Drives Philips Hue and Govee devices already configured in SimHub Ambient Lights.
- Supports global lighting or screen-thirds output for triples.
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
2. Open the plugin settings in SimHub.
3. Use the Devices tab to select the lights the plugin may control.
4. Drive in cockpit camera. The plugin learns the mask automatically.
5. Use `Identify` on devices if you need to confirm the light routing.

## Notes

The plugin does not store cockpit masks on disk. It starts fresh each SimHub session because cockpit view, FOV, resolution and car layout can change. The mask is fast to learn and continuously adapts while driving.

## Support

Created by Bruno Silva.

Other plugins:

- https://github.com/BrunoSilva1978PT/WhatsApp-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/Encoders-SimHub-Plugin
- https://github.com/BrunoSilva1978PT/bezel-free-negative-correction
- https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin