# Cockpit Auto Ambient Light

Cockpit Auto Ambient Light is a SimHub plugin that learns the stable cockpit area from screen capture and drives ambient lights from in-cockpit brightness and colour changes.

It is designed for cockpit-camera driving. Sunlight, shadows, night lighting, idle themes, VR themes and selected alert effects can be routed to Philips Hue, Govee, Bluetooth and Adalight endpoints from SimHub Ambient Lights, to WLED network LED controllers, or to compatible SyncLight / DX Light USB controllers driven directly by the plugin.

## License and Public Releases

Cockpit Auto Ambient Light is proprietary software. It is not open source.

Public release binaries, installers, documentation and repository materials are provided only under the included [End User License Agreement](EULA.md) and [proprietary license](LICENSE.md).

No permission is granted to copy, modify, redistribute, reverse engineer, decompile, disassemble, sell, sublicense, or create derivative or competing software from this project, except where applicable law does not allow such restriction.

All rights are reserved by Bruno Silva.

## Requirements

- SimHub. Enable Ambient Lights only when using native Hue V1, Govee, Bluetooth or Adalight outputs. Direct outputs work with that module disabled; Nitro uses SimHub Devices.
- .NET Framework 4.8 through SimHub.
- One or more supported light outputs:
  - Philips Hue through SimHub native Hue V1 output.
  - Philips Hue Direct V2 through this plugin's Hue bridge pairing and a Hue Entertainment Area.
  - Govee devices exposed by SimHub Ambient Lights.
  - Generic Bluetooth lights exposed by SimHub Ambient Lights.
  - Adalight output configured in SimHub Ambient Lights.
  - Nitro Concepts IMMERSION LED Box configured in SimHub Devices, using its Telemetry LEDs profile.
  - WLED network LED controllers (ESP8266/ESP32) driven directly by this plugin over your local network, with no SimHub Adalight, app or cloud required.
  - Experimental support for compatible SyncLight / DX Light USB addressable LED controllers detected and driven directly by this plugin.

Before troubleshooting, update SimHub to the latest available version. This plugin uses SimHub's screen capture library and its Ambient Lights effects and device pipeline for native outputs, so older SimHub builds can cause compatibility problems.

CAAL saves its own capture monitor selection and automatically preserves the existing Ambient Lights selection when upgrading, including three independent monitors without Surround. Screen analysis can reuse a matching Ambient Lights capture when available, or capture independently through SimHub's screen capture library with Ambient Lights disabled. Both paths use reduced-resolution frames. Independent capture uses up to 30 FPS per monitor and releases its consumers during pause, menus and idle. Direct-only output leaves the Ambient Lights master switch and capture settings unchanged.

If Ambient Lights is absent after SimHub finishes loading, CAAL can automatically use a previously configured Hue Direct V2 or Govee Direct LAN setup. Native remains the saved preference; the active backend and any setup required are explained on Home and Hardware. Existing assignments, themes and monitors are kept. Automatic Hue switching requires a previously resolved native-to-direct light map when native assignments exist. Incompatible layouts or native exclusions require review in Direct setup. Bluetooth and Adalight native outputs still need Ambient Lights. A Direct LAN startup failure keeps Direct available for Retry when native output is unavailable.

Home always shows an informational status while CAAL operates without Ambient Lights. Setup warnings refer to configured hardware, rather than old profile or theme entries. Configure Direct opens the corresponding Hardware section; choose a backend there to start setup. An incomplete setup remains visible for configured devices until the required bridge/area or device list is ready.

## Main Features

- A dedicated Updates page with automatic stable plugin updates enabled by default. When an update is ready, a notice offers Update and restart now or When I close SimHub. Choosing later keeps the session running and activates the update on the next start, with a reminder on Home and Updates. The automatic window waits until SimHub is foreground and live driving output is inactive. SimHub only restarts immediately after an explicit choice. Running and installed versions are shown separately. Manual rollback cancels pending automatic work and persistently pauses automatic updates until explicitly re-enabled, including rollback through the standalone installer.

- Automatic cockpit mask learning while driving. No manual rectangle drawing is required during normal use.
- Optional Track ambience zone learned from moving non-cockpit scenery across the full capture, with robust internal regions and the same optional cool/neutral/warm-white protection as cockpit output.
- Per-game Lights setup profiles for selected endpoints, cockpit zones, max brightness limits, fixed unselected output, dark-mode routing and alert permissions, plus one global final driving brightness control on Home.
- Per-game tuning for brightness boost (formerly light sensitivity), response smoothing, surface colour protection, colour amount, colour warmth (formerly Sun warmth), dark cutoff and dark-mode cockpit blending.
- Separate copy controls for Light response profiles and Lights setup profiles, so the current game can inherit only the settings you choose from another game or from the defaults.
- Output type switches for Hue, Govee, Bluetooth, Adalight, WLED, SyncLight USB and Nitro.
- Philips Hue Native V1 and Direct V2 backends.
- Hue Direct V2 bridge discovery through Hue cloud discovery, Hue mDNS discovery and local HTTPS probing.
- Hue Entertainment sync for Direct V2 output, including gradient light segments when the selected Entertainment Area exposes separate channels.
- Govee grouping by SimHub controller/device, while each segment remains an independent endpoint.
- Adalight virtual zones mapped to LED ranges.
- Nitro Concepts box and strip names, independent local 1-65 LED layouts for both strips, configurable zones and Identify. Hardware layouts are global; zone behaviour uses the existing Lights setup, Themes and Streaming settings, while animation layers can address every LED in a strip using the controller's native order.
- WLED network LEDs driven directly over UDP (realtime DRGB/DRGBW), independent of SimHub Ambient Lights: add devices by IP or `wled-xxxx.local`, or scan the local network (mDNS with a subnet fallback for networks that block mDNS). Each device supports virtual zones mapped to LED ranges or fixed colours, a colour for LEDs outside any zone, alert effects per zone, per-device rename/identify, and automatic gamma correction so colours match the WLED colour picker.
- Experimental SyncLight / DX Light compatible USB controllers with automatic detection and reconnection, controller-reported identity and LED count, global virtual-zone layouts, per-profile zone behaviour, Rename, red Identify, previews, themes, individual-LED animations, Streaming and alert effects. Physically validated with the current 27-inch, 63-LED controller; other compatible models and LED layouts still need physical validation.
- An editable fixed-output theme library shared by idle output, manual idle hold and VR output, with Add, Delete, drag-and-drop ordering and optional animation layers. Hold idle can follow its own selected theme or the normal idle theme.
- A global Streaming mode with its own complete driving endpoint profile and optional OBS live-state automation, without altering per-game Lights profiles or creating separate themes.
- Built-in alert effects for racing flags, spotter, two custom formulas, low fuel, redline, pit limiter, engine start, pedals, wheel lock and wheel spin.
- SimHub Control Mapper roles and bindable actions for toggling output, holding idle theme, cycling idle themes, adjusting idle or global driving brightness, resetting output and resetting the learned mask.

## First Setup

1. Update SimHub to the latest available version.
2. Configure your direct devices in the plugin, or configure and enable native devices in SimHub Ambient Lights.
3. For native Ambient Lights outputs, give every lamp or segment a unique Effects index.
4. Turn on Enable plugin on Home.
5. Choose the Global output layout on Lights setup: Global for one shared cockpit value, or Cockpit zones / triples for per-light routing.
6. Use Lights setup to enable the output types needed by the profile and choose which endpoints follow cockpit brightness. For WLED, add each controller by IP or with Scan, set its LED count, split it into zones, and set that device's brightness to 100% in its own WLED web page. Compatible SyncLight / DX Light USB controllers are detected automatically; split their reported LED count into physical zones on Hardware.
7. Assign cockpit zones only where useful. All is the safest default for simple layouts.
8. Open the Themes tab to configure the fixed-output themes used for idle output, manual idle hold and VR output.
9. Optional: open Effects to configure built-in alerts and choose which endpoints accept each alert on Lights setup.
10. Optional: open Streaming mode and configure what every endpoint should do while driving on stream. Copy current Lights setup is the quickest starting point. OBS WebSocket automation can activate it only while OBS is actually live.
11. Drive in cockpit camera. The mask learns automatically while the car moves.

The Reset learned mask / Recalibrate action is an escape hatch for a bad car or setup result. It is not part of normal setup.

## Output Modes and Themes

Live driving uses the learned cockpit mask and the current game's tuning profile. After pause, replay, menus, no game, waiting for data, or no learned mask, the plugin holds the idle theme until the first ignition, engine, RPM or movement signal arms live cockpit output. Once armed, stopping the car or switching ignition off does not by itself return to idle; the cycle resets when runtime becomes unavailable again. VR output uses the VR theme selected on the VR mode page while the selected VR detector is active.

In Cockpit zones / triples mode, Track ambience can be assigned explicitly to one lamp or LED zone. The Lights map shows it as one amber outline around the complete capture in both single-screen and triple-screen layouts. It uses moving scenery outside the cockpit mask across the full capture and never becomes part of All. Its fixed response curve can still react to headlights, fog, signs and track floodlights because the detector only sees rendered pixels. Track ambience boost controls only this fixed curve: 0% sends raw Track mask brightness, 100% keeps the default response, and higher values strengthen it without enabling adaptive exposure.

Use Copy from on the Light response profile row to apply another game's profile, or the Global Light response profile, to the current game only. It copies only brightness boost, Track ambience boost, response smoothing, surface colour protection, colour amount, colour warmth (formerly Sun warmth), dark-mode cockpit blend start and dark cutoff; Lights setup selections are unchanged. Brightness boost was previously called light sensitivity and now controls only the adaptive cockpit zones: 100% is the default, lower values reduce cockpit brightness gain, and higher values make maximum cockpit output easier to reach. Track ambience boost is independent and never affects the cockpit zones. Neither boost controls transition speed; response smoothing does.

The Themes tab provides an editable library of fixed-output themes. Use Add theme, Delete theme and Reorder to manage the library:

- Single colour applies one colour and brightness to every available endpoint.
- Custom per endpoint lets each endpoint have its own colour and brightness.
- Animation layers can be added on top of the static theme base.
- Edit mode previews changes live on the lights.
- Save stores the theme and returns the lights to normal output.
- Cancel restores the previous theme values.

Themes list available endpoints for enabled output types, not only endpoints ticked for cockpit output. This is intentional: themes are for fixed output outside live cockpit driving, so unselected lights can still be part of an idle or VR look.

Streaming mode is a global driving override and does not have a separate theme. Themes remain general: the current idle theme still runs in menus, idle and Hold idle, and its Driving unselected layers remain available while driving. The Streaming endpoint list decides the driving role of every light or segment. Ticked endpoints follow their assigned cockpit zone; unticked endpoints keep their fixed output and may be animated by the current theme's Driving unselected layers. Alert and dark-mode permissions also come from this Streaming profile. The Driving brightness control on Home is global, shared by normal and Streaming driving output, and applied last to all cockpit, fixed, animation and alert output. Switching Streaming off restores the normal Global/per-game Lights setup profile without changing that shared brightness. Hold idle remains the highest-priority manual override, and VR fixed output is bypassed while Streaming is active.

OBS automation uses OBS WebSocket 5 (built into OBS Studio 28 and later). It watches the actual stream output state, not whether the OBS application is running. In OBS, open Tools > WebSocket Server Settings and enable the server. If Enable Authentication is active, use Show Connect Info to copy the Server Password into the plugin; this is not an OBS, Twitch or YouTube account password, and it is stored with Windows user-level protection. Authentication can be disabled in OBS and the plugin password then left empty, although keeping it enabled is recommended when OBS is reachable from the network.

Animation layers override the static base only for endpoints assigned to that layer. Each endpoint can belong to one layer at a time; endpoints outside layers keep the theme's single-colour or custom-per-endpoint base output. A theme can contain up to eight layers.

Layer speed is configured in milliseconds from 50 ms to 10000 ms. The layer brightness controls set the base/resting brightness and the effect brightness. Effects that support two colours show an effect colour plus a base colour; the base colour can be optional for effects that can also run as a one-colour animation.

Current animation effects:

| Effect | Setup | Configuration tip |
| --- | --- | --- |
| Breathing | Any light or group. | Use a slow speed and a low Base brightness for a calm fade. Optional base colour gives the fade a second colour. |
| Sparkle breath | Any light or group. | A breathing base with a short sparkle section. Use when Breathing is too plain but Confetti is too busy. |
| Colour fade | Any light or group. | Pick two colours and use a slower speed for smooth ambient transitions. |
| Aurora wave | Ordered lights or strip segments. | Order lights from one end to the other, then use two soft colours and a slow speed. |
| Heartbeat | Any light or small group. | Two quick pulses followed by a rest. Add a base colour if the lights should not go dark between beats. |
| Twinkle | Several lights or strip segments. | Random soft sparkles over the base colour. Works best with a darker base and moderate speed. |
| Confetti | Several lights or strip segments. | Short random colour pops. Use lower brightness or slower speed if it feels too busy. |
| Marquee | Ordered lights, ideally four or more. | Alternating on/off pattern. Move rows until the chase follows the physical strip direction. |
| Runner | Ordered lights or strip segments. | A single bright block runs through the order. Use Comet when you want a visible tail. |
| Comet | Ordered lights or strip segments. | Bright head with fading tail. Works best with three or more lights. |
| Comet sweep | Ordered lights or strip segments. | Comet run followed by a fill and fade. Good when Comet feels too repetitive. |
| Scanner | Ordered lights or strip segments. | A bright point sweeps to the end and back. Enable base colour if the unlit part should stay visible. |
| Scanner sweep | Ordered lights or strip segments. | Scanner followed by mirrored sweep and a short hit. Best with the order running end-to-end. |
| Sweep fill | Ordered lights or strip segments. | Accumulates in order until all lights are on, then resets. Useful for checking strip direction. |
| Center out | Ordered lights or strip segments. | Starts in the middle and expands to both ends. Order the layer from one physical end to the other. |
| Edge in | Ordered lights or strip segments. | Starts at both ends and closes toward the middle. Order the layer from one physical end to the other. |
| Mirror sweep | Ordered lights or strip segments. | Alternates Center out and Edge in in one loop. Good when you do not want to choose one direction. |
| Mirror pulse | Ordered lights or strip segments. | Center/edge sweeps with stronger full-layer hits. Use when Mirror sweep feels too soft. |
| Random burst | Several lights or strip segments. | Small random groups flash and fade. Keep speed moderate to avoid a strobe feel. |
| Side pulse | Lights assigned to Left, Center and Right. | Left and Right pulse opposite each other; Center sits between the two beats. |
| Side alert | Lights assigned to Left, Center and Right. | Side pulse, heartbeat hits and a short strobe burst. Lower brightness if it feels aggressive. |
| Police | Lights assigned to Left, Center and Right. | Split bursts, ping-pong, in/out and full-sync strobe phases. Only Strobe brightness is used; lights are off between flashes. |
| Flash | Any light or group. | One short flash per loop. Increase speed duration for occasional flashes. |
| Strobe burst | Any light or group. | Three quick flashes per loop. Use longer speeds for occasional bursts instead of constant strobe. |

Ordered effects use the rows in "Light order in this layer"; move rows up or down until the animation follows the physical strip or lamp layout. Center out, Edge in, Mirror sweep, Mirror pulse and the sequence effects expect the order to run from one physical end to the other. Nitro and SyncLight run across the individual LEDs inside each assigned strip/controller using its native LED order; use **Reverse LEDs** on one assigned strip when it is mounted in the opposite direction. Position effects use Left, Center and Right per light instead of row order, so they are best for left/right rigs.

The Themes tab also exposes bindings for Previous idle theme and Next idle theme. Their StreamDeck Control Mapper roles are:

- `Cockpit Ambient - Previous idle theme`
- `Cockpit Ambient - Next idle theme`
- `Cockpit Ambient - Toggle streaming mode`
- `Cockpit Ambient - Idle brightness down`
- `Cockpit Ambient - Idle brightness up`
- `Cockpit Ambient - Driving brightness down`
- `Cockpit Ambient - Driving brightness up`

The driving-brightness actions always change the one global driving master shared by normal and Streaming output. Idle-brightness actions change the separate idle master. Hardware encoders use 5% steps and support 1% fine adjustment while held where the controller platform exposes a held-rotation event.

## Philips Hue

### Native V1

Native V1 uses SimHub Ambient Lights as the Hue backend. Configure the Hue bridge and entertainment group in SimHub first, then select the Hue endpoints in this plugin. This is the safest default path and behaves like other SimHub Ambient Lights devices.

### Direct V2

Direct V2 pairs directly with the Hue bridge, stores this plugin's application key, reads Hue V2 resources and sends output through Hue Entertainment sync. It can discover normal Hue bridges and Bridge Pro installations when they expose the same local Hue API.

Direct V2 requires a selected Hue Entertainment Area. Only lights and channels inside that area are controllable. Gradient-capable Hue lights appear as separate segment rows only when the selected Entertainment Area exposes separate Entertainment channels for that light.

A Hue bridge only allows one Entertainment sync owner for the same area at a time. While Direct V2 owns an area, the matching SimHub native Hue group is disabled and restored when Direct V2 is released, disabled or forgotten.

If you replace or migrate a bridge, use Forget Direct V2 bridge and pair again so the saved bridge ID, IP and Entertainment Area match the new bridge.

## Lights

The Lights setup profile can be saved per game. When SimHub selects a game for the first time, this plugin creates that game's profile from the Global Lights setup settings.

Use Copy from on the Lights setup profile row to apply another game's profile, or the Global Lights setup profile, to the current game only. It copies selected endpoints, cockpit zones, output ranges, fixed unselected output, dark-mode routing and alert permissions; Light response and cockpit masks are unchanged. When possible, Hue settings are translated between Native V1 and Direct V2 endpoint IDs for the active backend.

- Ticked endpoints follow cockpit brightness during live output.
- Unticked endpoints can keep a fixed unselected colour and brightness.
- Unticked endpoints can still accept built-in alert effects, which is useful for effects-only lights.
- Cockpit max brightness limit applies only to cockpit/dynamic output and does not force lamps to stay on.
- Dark cutoff in Game tuning can force very low final dynamic cockpit output to real black per game.
- Idle and VR output use themes, not the cockpit max brightness limit.

Govee devices are grouped by SimHub controller/device, but each segment is selected, identified, zoned and themed independently.

Adalight uses SimHub's configured physical Adalight output. This plugin maps virtual zones to LED ranges and can also include those zones in fixed-output themes when a real Adalight output is configured, enabled and connected.

### Hardware connection status

Hardware uses the same status colours for WLED, SyncLight USB and Nitro wherever the state is available:

- Grey: disabled or status unavailable.
- Blue: discovered but not configured.
- Yellow: checking, connecting or reconnecting.
- Orange: configuration warning.
- Red: configured hardware is offline, busy or in error.
- Green: connected or reachable.

Every device card also shows a written state. Hover its status dot for details and a recommended action, so colour is never the only indication.

### WLED

WLED devices are addressable LED controllers (ESP8266/ESP32) that this plugin drives directly over your local network using WLED's realtime UDP protocol. They do not go through SimHub Ambient Lights, Adalight, an app or the cloud, and they do not use a SimHub Effects index.

- Add a device by typing its IP (for example `192.168.1.50`) or its `wled-xxxx.local` name, or press Scan to find devices automatically. Scan uses mDNS and, because many routers block mDNS, also sweeps the local subnet, so it still finds devices when `wled.local` does not resolve. Scan shows the devices it found and lets you pick which to add.
- Each device shows its IP, LED count and reachability. Rename gives a device a friendly name (every WLED ships named "WLED", so duplicates are also numbered automatically), Identify blinks the strip, and Open launches its WLED web page.
- Each device is split into virtual zones mapped to LED ranges, exactly like Adalight. A zone can follow a cockpit zone (Ambient), show a Fixed colour, or be Disabled. LEDs outside any zone use a separate outside colour and brightness. Zones can also accept built-in alert effects and appear as endpoints in fixed-output themes.
- The device hardware, its Device enabled switch and gamma are global (shared by all games), but Enabled for this profile and the per-device zone routing are saved per game, exactly like the other output types. Choosing Global for Lights setup shares them across games instead, and copying a Lights setup profile brings the WLED zones along.
- Set the LED count, and if needed the realtime UDP port (default 21324; auto-detected on Add).
- Gamma correction is automatic: WLED does not apply its gamma curve to realtime data, so this plugin applies the same 2.2 RGB tone curve to driving, idle, themes and effects, while preserving the lowest levels to avoid crushing dark colours. Leave the gamma options in the WLED web page as they are.
- Set each device's brightness to 100% in its WLED web page, because WLED scales the plugin's output by its own brightness. Prefer reserving a static IP for each device in your router so it does not change.

### SyncLight / DX Light USB — experimental

This support is experimental. It has been physically validated with the current 27-inch, 63-LED controller; other compatible controller models and LED layouts have not yet been physically verified. Compatible USB addressable LED controllers are detected, identified and driven directly by the plugin. They do not need a SimHub Ambient Lights device or Effects index.

- Detection and reconnection are automatic. Use **Find controllers now** only after connecting a new compatible controller. Product ID, UUID, firmware, display size and LED count are read from the controller; its non-lighting HID interfaces are ignored.
- Hardware shows connection state, Enable device, Rename and Identify. Identify blinks only the selected controller or virtual zone red while the rest of the strip continues its normal output.
- The controller's raw brightness is kept at the full 255 internally. User-facing brightness remains in the plugin's percentage controls, including live previews.
- RGB channel order is selected automatically from the Product ID using the vendor SyncLight model rules (the controller does not report it directly). Hardware offers an advanced manual override only for unusual revisions or compatible clones with swapped colour channels.
- Split the reported LED count into named virtual zones. The physical zone layout is global; Ambient / Fixed / Disabled, cockpit mapping, output range, dark mode and alerts follow the Global or per-game Lights setup profile.
- Zones are available to normal cockpit output, fixed theme colours, Streaming mode and effect previews. Animation layers list the whole controller once and animate its reported LEDs individually in the controller's native order; ordered effects offer **Reverse LEDs** per assigned controller, and driving-unselected layers preserve every cockpit-selected range. Targeted Identify and previews leave all non-target LEDs under normal output.
- **When plugin stops** defaults to Turn off. It can instead leave a Fixed color at a chosen percentage brightness or Keep last color. These choices apply on deliberate release and normal SimHub shutdown; crashes and power loss cannot guarantee a final frame.
- The vendor SyncLight / DX Light application may exclusively own the USB lighting interface. When possible, the plugin reports the conflicting process and PID and offers **Close app and retry**. If it restarts automatically, disable its lighting plugin or startup entry before retrying.

Nitro hardware also has **When plugin stops**. Its default, Restore SimHub profile, returns the box to its selected Telemetry LEDs profile. Turn off and Fixed color are available when a persistent final state is preferred.

## Built-in Alert Effects

Built-in alerts run on endpoints that permit them. Each endpoint chooses which alerts it accepts on Lights setup, including unticked effects-only endpoints.

Pit limiter and engine start are always highest priority. The Effects tab lets you reorder spotter, racing flags, Custom 1, Custom 2, wheel lock, wheel spin, low fuel, redline and pedals.

Custom 1 and Custom 2 are user-defined alert slots. Each one has its own optional display name, SimHub native JavaScript/NCalc formula, Blink/Fixed/Fade mode, colours, blink speed and optional duration limiting in seconds. They can be used for simulator-specific properties such as iRacing off-track, incident logic or any other SimHub formula without adding a separate built-in effect for every possible game property.

## Troubleshooting

Start by updating SimHub. This matters most for Ambient Lights capture, Govee, Bluetooth, effects routing and device-output issues.

If lights do not react:

- For native outputs, check that the Ambient Lights module, master output and devices are enabled. Direct outputs do not require that module.
- Check that the matching output type is enabled for the profile on Lights setup.
- Use Identify to confirm the bridge or SimHub device path is working.
- For native outputs, make sure each endpoint has a unique Effects index in SimHub Ambient Lights.

If Direct Hue V2 does not sync:

- Check that a Hue Entertainment Area is selected.
- Make sure another app, Hue Sync session or SimHub native Hue group is not owning the same area.
- Click Resync once.
- If the bridge was replaced or migrated, Forget Direct V2 bridge and pair again.

If the Themes tab shows no endpoints for a group, check that the output type is enabled and that SimHub or Direct Hue V2 can currently see those endpoints. Adalight endpoints only appear when a physical Adalight output is configured, enabled and connected.

If WLED does not react or looks wrong:

- Check that WLED is enabled for the profile on Lights setup and that the device shows as Reachable.
- Set that device's brightness to 100% in its WLED web page; a lower value dims everything the plugin sends.
- If Scan finds nothing, add the device by IP instead; some networks block the mDNS the scan starts with. If it is missed on the first scan, scan once more (the first attempt warms the network so the second finds it).
- Colour is gamma-corrected automatically to match the WLED colour picker. Do not change the gamma options in the WLED web page; the plugin replaces them for realtime output.
- If red and green (or other channels) look swapped, fix the Color Order in the WLED web page; the plugin sends standard RGB and WLED maps it to the strip.
- Reserve a static IP for each WLED device in your router so it does not change and drop out.

If a SyncLight / DX Light USB controller does not react:

- Check Enable device on Hardware, the SyncLight USB profile switch and the zone assignments on Lights setup.
- Reconnection is automatic. Use Find controllers now after connecting new hardware; it is not a separate operating mode.
- Use Identify to verify the selected LEDs blink red while the rest of the strip stays under normal output.
- If Hardware reports another process, close it with Close app and retry or disable its lighting plugin/startup entry so it does not reclaim the controller.

One shared colour pipeline drives All, every cockpit zone and Track ambience. Surface colour protection is off by default for new profiles. When enabled, it keeps plausible cool, neutral and warm white illumination while suppressing strong surface colours. Turn it off in the per-game or Global Light response profile to use the captured mask colour. Colour amount then controls that colour: 0% removes it and 100% preserves it. Colour warmth (previously Sun warmth, with saved values retained) applies afterwards: 0% adds no warmth and 100% gives whites a strong warm tone, even in bright scenes or with Colour amount at 0%. Night-time bright-point rejection and exposure calculate brightness separately; they do not attenuate the RGB samples used for colour. Channels are scaled together instead of clipped independently, so strong colour or warmth can limit achievable brightness. Alerts, themes, dark mode base colour and fixed outputs bypass these colour controls.

If fullscreen capture freezes, check the game executable Compatibility settings and leave "Disable fullscreen optimizations" off, or use borderless/windowed mode.

If a setup still behaves like an old configuration is stuck, use Factory reset on the Maintenance page. It resets Cockpit Auto Ambient Light settings and known settings backups while keeping your capture monitors and SimHub Ambient Lights devices, profiles and Effects indexes.

## Legal Notices

See [LICENSE.md](LICENSE.md), [EULA.md](EULA.md), and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
