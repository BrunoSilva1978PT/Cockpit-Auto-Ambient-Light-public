# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin that learns the stable cockpit area from SimHub Ambient Lights screen capture and drives ambient lights from in-cockpit brightness and colour changes.

It is designed for cockpit-camera driving. Sunlight, shadows, night lighting, idle themes, VR themes and selected alert effects can be routed to Philips Hue, Govee, Bluetooth, Adalight and other endpoints used by SimHub Ambient Lights.

## License and Public Releases

Cockpit Auto Ambient Light is proprietary software. It is not open source.

Public release binaries, installers, documentation and repository materials are provided only under the included [End User License Agreement](EULA.md) and [proprietary license](LICENSE.md).

No permission is granted to copy, modify, redistribute, reverse engineer, decompile, disassemble, sell, sublicense, or create derivative or competing software from this project, except where applicable law does not allow such restriction.

All rights are reserved by Bruno Silva.

## Requirements

- SimHub with Ambient Lights configured and enabled.
- .NET Framework 4.8 through SimHub.
- One or more supported light outputs:
  - Philips Hue through SimHub native Hue V1 output.
  - Philips Hue Direct V2 through this plugin's Hue bridge pairing and a Hue Entertainment Area.
  - Govee devices exposed by SimHub Ambient Lights.
  - Generic Bluetooth lights exposed by SimHub Ambient Lights.
  - Adalight output configured in SimHub Ambient Lights.

Before troubleshooting, update SimHub to the latest available version. This plugin reuses SimHub's internal Ambient Lights capture, effects and device pipeline, and older SimHub builds can expose different behaviour where some plugin features may not work correctly.

## Main Features

- Automatic cockpit mask learning while driving. No manual rectangle drawing is required during normal use.
- Per-game device profiles for selected endpoints, cockpit zones, max brightness limits, fixed unselected output, dark-mode routing and alert permissions.
- Per-game tuning for light sensitivity, response smoothing, colour amount, sun warmth, dark cutoff and dark-mode cockpit blending.
- Separate copy controls for tuning profiles and device profiles, so the current game can inherit only the settings you choose from another game or from the defaults.
- Output type switches for Hue, Govee, Bluetooth and Adalight.
- Philips Hue Native V1 and Direct V2 backends.
- Hue Direct V2 bridge discovery through Hue cloud discovery, Hue mDNS discovery and local HTTPS probing.
- Hue Entertainment sync for Direct V2 output, including gradient light segments when the selected Entertainment Area exposes separate channels.
- Govee grouping by SimHub controller/device, while each segment remains an independent endpoint.
- Adalight virtual zones mapped to LED ranges.
- Five editable fixed-output themes shared by idle output, manual idle hold and VR output.
- Built-in alert effects for racing flags, spotter, two custom formulas, low fuel, redline, pit limiter, engine start, pedals, wheel lock and wheel spin.
- SimHub Control Mapper roles and bindable actions for toggling output, holding idle theme, cycling idle themes, resetting output and resetting the learned mask.

## First Setup

1. Update SimHub to the latest available version.
2. Configure and enable your lights in SimHub Ambient Lights.
3. Give every lamp, Hue segment, Govee segment or other endpoint a unique Effects index in SimHub Ambient Lights.
4. Enable Cockpit Auto Ambient Light in the plugin's General tab.
5. Choose an output mode and enable Full control if you want this plugin to own selected lights and run built-in alerts.
6. Use the Devices tab to enable the output types you need and choose which endpoints follow cockpit brightness.
7. Assign cockpit zones only where useful. All is the safest default for simple layouts.
8. Open the Themes tab to configure the fixed-output themes used for idle output, manual idle hold and VR output.
9. Optional: open the Effects tab to configure built-in alerts and choose which endpoints accept each alert on the Devices tab.
10. Drive in cockpit camera. The mask learns automatically while the car moves.

The Reset learned mask / Recalibrate action is an escape hatch for a bad car or setup result. It is not part of normal setup.

## Output Modes and Themes

Live driving uses the learned cockpit mask and the current game's tuning profile. After pause, replay, menus, no game, waiting for data, or no learned mask, the plugin holds the idle theme until the first ignition, engine, RPM or movement signal arms live cockpit output. Once armed, stopping the car or switching ignition off does not by itself return to idle; the cycle resets when runtime becomes unavailable again. VR output uses the VR theme selected on the Overrides tab while the selected VR detector is active.

Use Copy tuning profile from in Game tuning to apply another game's tuning profile, or the default tuning profile, to the current game only. It copies only light sensitivity, response smoothing, colour amount, sun warmth, dark-mode cockpit blend start and dark cutoff; device selections are unchanged.

The Themes tab provides five fixed-output theme slots:

- Single colour applies one colour and brightness to every available endpoint.
- Custom per endpoint lets each endpoint have its own colour and brightness.
- Edit mode previews changes live on the lights.
- Save stores the theme and returns the lights to normal output.
- Cancel restores the previous theme values.

Themes list available endpoints for enabled output types, not only endpoints ticked for cockpit output. This is intentional: themes are for fixed output outside live cockpit driving, so unselected lights can still be part of an idle or VR look.

The Themes tab also exposes bindings for Previous idle theme and Next idle theme. Their StreamDeck Control Mapper roles are:

- `Cockpit Ambient - Previous idle theme`
- `Cockpit Ambient - Next idle theme`

## Philips Hue

### Native V1

Native V1 uses SimHub Ambient Lights as the Hue backend. Configure the Hue bridge and entertainment group in SimHub first, then select the Hue endpoints in this plugin. This is the safest default path and behaves like other SimHub Ambient Lights devices.

### Direct V2

Direct V2 pairs directly with the Hue bridge, stores this plugin's application key, reads Hue V2 resources and sends output through Hue Entertainment sync. It can discover normal Hue bridges and Bridge Pro installations when they expose the same local Hue API.

Direct V2 requires a selected Hue Entertainment Area. Only lights and channels inside that area are controllable. Gradient-capable Hue lights appear as separate segment rows only when the selected Entertainment Area exposes separate Entertainment channels for that light.

A Hue bridge only allows one Entertainment sync owner for the same area at a time. While Direct V2 owns an area, the matching SimHub native Hue group is disabled and restored when Direct V2 is released, disabled or forgotten.

If you replace or migrate a bridge, use Forget Direct V2 bridge and pair again so the saved bridge ID, IP and Entertainment Area match the new bridge.

## Devices

The Devices tab is saved per game. When SimHub selects a game for the first time, this plugin creates that game's device profile from the default device settings.

Use Copy device profile from on the Devices tab to apply another game's device profile, or the default device profile, to the current game only. It copies selected endpoints, cockpit zones, output ranges, fixed unselected output, dark-mode routing and alert permissions; Game tuning and cockpit masks are unchanged. When possible, Hue settings are translated between Native V1 and Direct V2 endpoint IDs for the active backend.

- Ticked endpoints follow cockpit brightness during live output.
- Unticked endpoints can keep a fixed unselected colour and brightness.
- Unticked endpoints can still accept built-in alert effects, which is useful for effects-only lights.
- Cockpit max brightness limit applies only to cockpit/dynamic output and does not force lamps to stay on.
- Dark cutoff in Game tuning can force very low final dynamic cockpit output to real black per game.
- Idle and VR output use themes, not the cockpit max brightness limit.

Govee devices are grouped by SimHub controller/device, but each segment is selected, identified, zoned and themed independently.

Adalight uses SimHub's configured physical Adalight output. This plugin maps virtual zones to LED ranges and can also include those zones in fixed-output themes when a real Adalight output is configured, enabled and connected.

## Built-in Alert Effects

Built-in alerts run when Full control of selected lights is enabled. Each endpoint chooses which alerts it accepts on the Devices tab, including unticked effects-only endpoints.

Pit limiter and engine start are always highest priority. The Effects tab lets you reorder spotter, racing flags, Custom 1, Custom 2, wheel lock, wheel spin, low fuel, redline and pedals.

Custom 1 and Custom 2 are user-defined alert slots. Each one has its own optional display name, SimHub native JavaScript/NCalc formula, Blink/Fixed/Fade mode, colours, blink speed and optional duration limiting in seconds. They can be used for simulator-specific properties such as iRacing off-track, incident logic or any other SimHub formula without adding a separate built-in effect for every possible game property.

## Troubleshooting

Start by updating SimHub. This matters most for Ambient Lights capture, Govee, Bluetooth, effects routing and device-output issues.

If lights do not react:

- Check that SimHub Ambient Lights master output is enabled.
- Check that the devices are enabled in SimHub Ambient Lights.
- Check that the matching output type is Enabled on this plugin's Devices tab.
- Use Identify to confirm the bridge or SimHub device path is working.
- Make sure each endpoint has a unique Effects index in SimHub Ambient Lights.

If Direct Hue V2 does not sync:

- Check that a Hue Entertainment Area is selected.
- Make sure another app, Hue Sync session or SimHub native Hue group is not owning the same area.
- Click Resync once.
- If the bridge was replaced or migrated, Forget Direct V2 bridge and pair again.

If the Themes tab shows no endpoints for a group, check that the output type is enabled and that SimHub or Direct Hue V2 can currently see those endpoints. Adalight endpoints only appear when a physical Adalight output is configured, enabled and connected.

If colours are wrong in triples or multi-light layouts, lower Colour amount in the General tab to keep brightness response while reducing colour contamination from dashboards, HUDs, kerbs and trackside objects. If bright sunlight feels too white or cool, raise Sun warmth; it gently warms mostly bright/neutral cockpit output without changing alert effects, themes, dark mode or fixed colours.

If fullscreen capture freezes, check the game executable Compatibility settings and leave "Disable fullscreen optimizations" off, or use borderless/windowed mode.

If a setup still behaves like an old configuration is stuck, use Reset plugin settings on the Diagnostics tab. It resets only Cockpit Auto Ambient Light settings and known settings backups; SimHub Ambient Lights devices, profiles and Effects indexes are kept.

## Legal Notices

See [LICENSE.md](LICENSE.md), [EULA.md](EULA.md), and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
