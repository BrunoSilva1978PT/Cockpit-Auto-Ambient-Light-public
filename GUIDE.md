# Cockpit Auto Ambient Light Guide

This guide describes how the current Cockpit Auto Ambient Light plugin works and how to set it up in SimHub.

## What The Plugin Does

Cockpit Auto Ambient Light watches SimHub's native Ambient Lights screen capture, learns the stable cockpit area, and drives selected Ambient Lights devices from cockpit brightness and colour.

The goal is to make physical cockpit lights react to sun, shadow and night lighting inside the car. In the default priority mode, normal SimHub effects can still appear above it. With Full control enabled, this plugin owns selected lights and its built-in alert effects can override cockpit and dark-mode output per lamp.

Alert effects can also be assigned to endpoints that are not selected for cockpit output. That lets some lights follow the cockpit while other lights stay effects-only.

The plugin does not discover Hue or Govee devices directly. Configure and enable supported devices in SimHub Ambient Lights first; this plugin reuses those devices and their capture/effects pipeline.

The plugin also does not rewrite SimHub Ambient Lights effect indexes. Each lamp or segment should have its own unique Effects index in SimHub, otherwise multiple endpoints can share one output slot and appear to ignore their cockpit zone or alert assignment.

## First Setup

1. Configure Philips Hue, Govee or other supported devices in SimHub Ambient Lights.
2. Check that every lamp or segment has a different Effects index in SimHub Ambient Lights.
3. Make sure the SimHub Ambient Lights master output is enabled.
4. Open Cockpit Auto Ambient Light in SimHub.
5. Keep `Enable cockpit ambient lighting` enabled in the General tab.
6. Open the Devices tab, refresh if needed, and tick the lights or Govee segments this plugin should control for cockpit output.
7. Adjust each Hue lamp or Govee device cockpit range if needed.
8. If you use Full control, choose which alert effects each endpoint accepts, including unticked effects-only endpoints.
9. Optional: configure built-in alerts on the Effects tab.
10. Optional: configure VR mode or dark mode on the Overrides tab.
11. Drive in cockpit camera. For best learning, use a stable camera and avoid large overlays over the cockpit view.

## Capture And Mask Learning

The plugin uses SimHub's native Ambient Lights capture engine. It subscribes to SimHub capture updates, then copies and processes the latest bitmap outside the native capture callback. If SimHub is busy with its bitmap, the plugin skips that frame instead of waiting inside the capture path.

Mask learning only runs while the car is moving. This helps the detector separate stable cockpit surfaces from the moving outside world.

For best results, keep the cockpit camera stable while the mask learns. Camera shake, look-to-apex/head movement and large HUD or overlay elements over the cockpit view can confuse the detector because they change what appears stable inside the captured cockpit area.

The last accepted mask is stored per game and car. On the next session, the stored mask is used as a startup seed. While you drive, the plugin keeps learning a candidate mask in the background. If the candidate stays different long enough, it can replace the current mask automatically. This covers FOV changes, seat position changes and different cockpit-camera views.

A stable game/car change saves the previous mask, loads the new car's stored mask when available, or starts a fresh learn after a short debounce.

## Reset Buttons

`Reset plugin output` is for a stuck SimHub Ambient Lights output path. It restarts the SimHub Ambient Lights output toggle and keeps the learned mask.

`Reset learned mask` is for a wrong cockpit mask. It clears the stored mask for the current game/car and starts learning again while you drive.

Use `Reset plugin output` first when lights/capture output appear stuck. Use `Reset learned mask` only when the brightness response itself suggests the cockpit area was learned wrongly.

## Lighting Modes

### Global

Every selected lamp follows the combined cockpit signal. This is the safest mode for most rigs.

### Cockpit Zones / Triples

This mode exposes zone output for selected lamps.

Available lamp zones:

- `All`: combined cockpit signal.
- `Left outer` and `Left inner`: direct sampled left-side zones.
- `Center-left` and `Center-right`: direct sampled center zones.
- `Right inner` and `Right outer`: direct sampled right-side zones.
- `Lower cockpit`: the lowest part of the learned cockpit mask.
- `Lower cockpit left`, `Lower cockpit center` and `Lower cockpit right`: lower cockpit split by side.
- `Left`, `Center` and `Right`: combined outputs for simpler rigs.

Wide/triple captures use screen zones suited for triple layouts. Single-screen captures use adaptive mask zones instead of blindly splitting one monitor into equal thirds.

Use direct zones when you have enough lamps to make the cockpit react with more direction. Use `Left`, `Center` or `Right` when one lamp should follow a whole combined side/center area.

### Colour Amount

`Colour amount` controls how much sampled cockpit colour reaches the lamps:

- 0%: grayscale / brightness-only response.
- 100%: full sampled cockpit colour.

Lower values are calmer and reduce colour contamination from dashboards, HUDs, cockpit plastics, kerbs, walls and trackside objects.

### Cockpit Brightness Range

Cockpit brightness range is configured on the Devices tab.

- Hue rows use their own range per lamp.
- Govee rows share one range per SimHub Govee device group.
- Govee segments still keep their own selection, identify button and zone.

Cockpit brightness range applies only to cockpit/dynamic output. Idle colour, VR fixed colour and built-in alert effects use their own brightness controls.

### Game Tuning

`Light sensitivity` and `Response smoothing` are saved per game from the General tab.

- 100% sensitivity and 0% smoothing keep the default response.
- Lower sensitivity reduces the strength of bright/dark swings.
- Higher sensitivity makes cockpit light changes more aggressive.
- Higher smoothing makes transitions slower and calmer, useful for trucks, open cockpits or games with rapid shadow changes.

These sliders tune output only. They do not change cockpit mask learning.

### Unselected Lamps

Devices enabled in SimHub Ambient Lights but not selected in this plugin can use their own fixed colour and brightness. 0% brightness keeps that unselected endpoint dark.

Unticked endpoints can still accept built-in alert effects, so you can reserve lamps for alerts only.

Normal SimHub colour effects can still have priority above this base unless Full control is active.

## Effects Priority

Cockpit Auto Ambient Light writes a base layer through SimHub's Ambient Lights profile.

`Effects first` is the default. Normal SimHub effects and other plugins can overlay this plugin's cockpit output.

`Full control of selected lights` makes this plugin own selected cockpit lights. Current SimHub Ambient Lights profile effects do not run on those selected lights. Built-in alert effects from the Effects tab can override cockpit or dark-mode output on selected lights, and can also drive unticked effects-only endpoints. Pause, replay, menus, no game, waiting for data or no learned mask use the idle colour from the General tab.

`Hold idle colour` can be bound to a button. It keeps selected lights on the idle colour and brightness until toggled off, while unticked endpoints keep their fixed unselected output. It only works while Full control is enabled.

## Devices Tab

Use the Devices tab to select which SimHub Ambient Lights endpoints this plugin controls.

Hue devices are listed by lamp/area. Govee devices are grouped by SimHub controller/device, but each segment is listed as its own selectable endpoint. Use `Identify` per lamp or segment to confirm the physical routing.

Disabled Hue Direct V2 Entertainment Areas stay saved for later use, but their endpoints are not used by Identify, themes or live output until that area is enabled again.

Philips Hue Native V1 and Direct V2 can expose the same physical lights with different names and endpoint IDs. Direct V2 uses Hue's real V2 light and channel names, while Native V1 follows SimHub's Hue group/endpoint naming. Device and theme settings are copied only where the plugin can safely match the endpoints, so review themes after switching backend.

Important: keep one unique Effects index per physical lamp or Govee segment in SimHub Ambient Lights. If two endpoints share the same index, SimHub can route them through the same output slot, so they may mirror each other even when this plugin assigns different cockpit zones or alerts. The plugin warns about this setup requirement but leaves SimHub indexes untouched.

For each endpoint, choose:

- Alert effects accepted by that endpoint.
- Zone selection, when the endpoint is selected for cockpit output and Cockpit zones / triples is used.
- Dark mode selection, for selected or unselected endpoints that should accept dark-mode output.
- Cockpit brightness range, for Hue lamps or Govee groups.
- Fixed colour/brightness for unselected SimHub lights.

## Built-In Alert Effects

Built-in alert effects run only when Full control is enabled. Each endpoint decides which alerts it accepts on the Devices tab, even when it is not selected for cockpit output.

Priority is resolved per lamp, so different lamps can show different alerts at the same time.

Locked highest priority:

1. Pit limiter.
2. Engine start.

Custom priority list:

- Spotter.
- Racing flags.
- Wheel lock.
- Wheel spin.
- Low fuel.
- Redline.
- Pedals.

Normal cockpit, VR, dark-mode or idle output stays underneath alert effects.

### Engine Start

Engine start plays when SimHub reports the engine starting.

Modes:

- `Warm afterglow`: uses Colour 2 as a short flash, then fades Colour 1 like a warm glow.
- `Hard flash`: flashes Colour 1 twice with short off gaps.
- `Ignition burst`: runs Colour 1 and Colour 2 sparks, then finishes with a fading Colour 3 afterglow.

The preview plays the full animation on endpoints that accept Engine start.

### Racing Flags

Configure each flag separately. You can disable a noisy flag, choose Static, Blink or Pulse, set speed, and optionally show it only for a fixed duration.

Duration hides a flag until the sim clears it and raises it again. If multiple flags are active at once, the flag priority list decides which one wins.

Supported flags:

- Yellow.
- Green.
- Blue.
- Checkered.
- White.
- Orange.

### Spotter

Spotter uses one colour and blink speed, but the Devices tab lets each lamp separately accept left-side and right-side spotter alerts.

### Low Fuel

Low fuel can use percent or liters. There are separate low and critical thresholds, and each stage can be Static or Blink.

The acknowledge action hides the current low or critical alert until the fuel stage clears or changes.

### Redline

Redline uses SimHub's current car/current gear redline automatically.

### Pit Limiter

Pit limiter alternates between two user colours.

Modes:

- `All lamps together`: default and most compatible. Every lamp that accepts Pit limiter shows colour 1 together, then colour 2 together on the next blink.
- `Use cockpit zones`: left-side zones start on colour 1, right-side zones start on colour 2, then swap on each blink. Neutral zones such as All, Center and Lower cockpit alternate on their own.

### Pedals

Pedals can show throttle and brake as static colours or as brightness scaled from pedal travel. Brake has priority over throttle. Use the throttle and brake enable switches when a lamp should react to only one pedal.

### Wheel Lock And Wheel Spin

Wheel lock uses ABS active when the sim reports it, then calculated wheel lock while braking as a fallback.

Wheel spin uses TC active when available, then calculated wheel spin while accelerating as a fallback.

Both effects stay active only while the signal is active.

## VR Mode

VR mode temporarily replaces cockpit output with a fixed colour and brightness while SimHub has an active game and the selected VR detector reports VR active.

Detection sources:

- `Automatic experimental`: checks OpenXR Toolkit's running app registry value and SteamVR/OpenVR's current scene process.
- `SimHub property`: uses a user-selected SimHub property and compares it with configured ON/OFF values.

Special SimHub property values:

- `<any>`: any non-empty value.
- `<null>`: missing or null.
- `<empty>`: empty text.

SimHub property detection is usually the most reliable choice when another plugin, macro or button workflow can expose the VR state.

VR fixed output uses its own colour and brightness. It does not use each device cockpit brightness range.

## Dark Mode

Dark mode is intended to imitate a steady endurance-style cockpit light, such as a red, cyan, purple or amber cabin glow.

Dark mode can use:

- Plugin local colour/state.
- Lovely Plugin true dark mode properties.
- Daniel Newman Racing true dark mode properties.

When dark mode is active, the selected colour is used as the base. Cockpit light is layered over it when the adaptive cockpit output gets bright enough, so the base colour does not hide modest brightness changes.

The Devices tab decides which endpoints accept dark mode. It can be enabled on selected cockpit lights or on unselected SimHub Ambient Lights endpoints.

`Base colour brightness` scales only the steady dark-mode base colour used by this plugin. Built-in alert effects still have priority while Full control is enabled.

`Cockpit blend start` is based on adaptive cockpit output before each device applies its own min/max range:

- Lower values let cockpit light layer over dark mode sooner.
- The layer ramps in over a short output range instead of waiting for 100% brightness.

`Prioritize cockpit lighting during dark mode` changes priority only while dark mode is active:

- Off: normal SimHub colour effects stay above this plugin.
- On: this plugin moves above normal effects during dark mode so dynamic cockpit lighting remains visible.

If Full control is enabled, cockpit lighting already has priority globally, so the dark-mode-only priority control is disabled.

## Button Bindings And StreamDeck

Normal SimHub actions can be bound through the plugin UI/control picker.

For StreamDeck workflows using SimHub Control Mapper roles, use these role names:

- `Cockpit Ambient - Toggle plugin`
- `Cockpit Ambient - Reset output`
- `Cockpit Ambient - Reset mask`
- `Cockpit Ambient - Hold idle colour`
- `Cockpit Ambient - Toggle dark mode`
- `Cockpit Ambient - Acknowledge low fuel`

The plugin listens to these roles and runs the matching action when they are triggered.

## Diagnostics And Logging

The Diagnostics tab shows:

- Current game and car.
- Mask state and last calibration.
- Overall plus direct sampled zones: left outer, left inner, center-left, center-right, right inner and right outer.
- Lower cockpit plus lower cockpit left, center and right.
- Adaptive exposure, gain and mask block counts.
- SimHub Ambient Lights output state.
- Capture source and capture frame counters.
- Selected light count.
- Runtime, speed and learning gates.
- VR detection state.

Enable debug logging only when troubleshooting. The log is written to:

`Documents\CockpitAmbientLight\Logs\plugin.log`

The log is cleared on startup and focuses on capture, bridge, SimuLight and Govee discovery details.

## Troubleshooting

### Lights Do Not React

Check that the lights are enabled in SimHub Ambient Lights and selected for cockpit output or at least one alert effect in this plugin. Use `Identify` to confirm routing.

### Other Effects Override Cockpit Light

Temporary alert effects are expected to override cockpit light. If another plugin/profile runs a constant effect on the same lamps, disable that continuous effect or use Full control of selected lights.

### Effects-Only Lights Do Not Show Alerts

Effects-only lamps still require Full control and at least one alert effect ticked on the Devices tab. They do not fire during Hold idle colour, pause or idle fallback; those states intentionally keep alerts cleared.

### SimHub Scene Settings Are Reset

When the plugin is enabled, it saves the current SimHub Ambient Lights scene brightness, effect profile brightness and luminosity gamma settings before applying its own 100% brightness and neutral gamma defaults. When the plugin is disabled, those saved settings are restored.

If you change those SimHub settings while this plugin is active, the plugin keeps the new values instead of overwriting them on disable, and saves them as the new restore point.

### Mask Never Becomes Active

Drive above walking speed in cockpit camera. The detector only learns while the car is moving. Disable heavy cockpit camera movement and keep large overlays away from the cockpit area when possible.

### SimHub Capture Freezes In Fullscreen

This plugin uses SimHub's native Ambient Lights screen capture. If that capture freezes when a game enters the track in fullscreen mode, check the game executable Compatibility settings and leave `Disable fullscreen optimizations` off. Borderless/windowed mode is the usual fallback when a game blocks native screen capture.

### Wrong Colours In Triples Or Zones

Lower `Colour amount`. This keeps zone brightness response while moving output closer to grayscale.

### Output Appears Stuck

Use `Reset plugin output`. It restarts SimHub Ambient Lights output and keeps the learned mask.

### Mask Looks Wrong

Use `Reset learned mask`. It clears the stored mask for the current game/car and starts learning again while driving. If the issue repeats, reduce cockpit camera movement and remove large overlays from the cockpit view before learning again.

## Support / Contact

For questions, problems or setup help, contact Bruno Silva:

- Email: bruno.1978@gmail.com
- GitHub issues: https://github.com/BrunoSilva1978PT/Cockpit-Auto-Ambient-Light-public/issues
- Discord: Bruno Silva (username: brunosilva1978)
