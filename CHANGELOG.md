# Changelog

## 1.23.0 - 2026-10-17

- Separated captured colour from night-time bright-point rejection and exposure. Dashboard highlights remain limited for brightness without suppressing their colour contributions.
- Reworked Surface colour protection to preserve plausible cool, neutral and warm whites, followed by Colour amount and Colour warmth in one shared pipeline.
- Renamed Sun warmth to Colour warmth (with the old name shown in the interface and saved values retained), with stronger warming that also works in bright scenes.
- Preserved RGB channel ratios during brightness boost, lamp gamma and output-range adjustments instead of whitening colours through per-channel clipping.

## 1.22.1 - 2026-08-15

- Added a Surface colour protection toggle to each Global or per-game Light response profile. Protection remains enabled by default; turning it off preserves the captured colours for All, cockpit zones and Track ambience without changing their brightness response.

## 1.22.0 - 2026-07-31

### Govee Direct LAN

- Fixed compatibility with Govee lights using newer firmware that SimHub can no longer detect reliably.
- Added Direct LAN support so the plugin can discover and connect directly to compatible Govee lights.
- Added device scanning, Identify, segment support and automatic hand-off between Native and Direct LAN output.

### Streaming mode and global controls

- Added a global Streaming mode with its own complete driving endpoint profile. Selected endpoints follow cockpit zones; unselected endpoints keep fixed output or the current theme's Driving unselected animations.
- Added optional OBS WebSocket automation that activates Streaming mode only while OBS is actually live.
- Added Streaming mode, idle-brightness and driving-brightness actions to SimHub bindings, Control Mapper, Control Bridge, Elgato Stream Deck, Stream Dock / MiraBox and Ulanzi, including live state and encoder support.
- Added one global Driving brightness control on Home, shared by normal and Streaming driving output and independent from the selected per-game or Global profile.
- Removed the Logitech / Loupedeck integration because its controller actions could not be maintained reliably across the supported hardware and software.

### Hardware, profiles and interface

- Separated global physical Hardware configuration from per-game Lights setup behaviour. Devices, backends and physical LED layouts remain global, while endpoint selection, cockpit routing, fixed output, dark mode and alert permissions remain profile-based.
- Kept available hardware endpoints consistent across normal profiles, Streaming and themes without duplicating physical configuration per game.
- Added Select all and Unselect all controls to endpoint groups and improved the organisation of native and Direct LAN Govee device cards.
- Updated profile names, navigation labels, the built-in Guide and public documentation to match the current interface.

### Racing flags and capture reliability

- Added a recommended Driver's zone trigger for yellow flags. It uses player-local or current-sector telemetry where supported and falls back to SimHub's standard yellow signal; All reported yellows keeps the previous behaviour.
- Added recovery for stalled SimHub Ambient Lights capture and safer output restart handling without clearing learned cockpit masks or plugin settings.

## 1.21.2 - 2026-07-27

- Stopped controller integration updates from targeting software that is not installed. Residual plugin files and configuration folders left behind by Stream Deck, Stream Dock, Ulanzi Studio or Logitech / Loupedeck no longer trigger downloads, installers or file-association prompts during SimHub startup.
- Revalidate the corresponding controller application immediately before installation, so an application removed after the update check is safely skipped too.

## 1.21.1 - 2026-07-26

- Fixed Stream Dock not reopening after its integration was installed or updated. The updater now resolves the application from the running process or its standard install locations, starts it from its own working directory and confirms that it is running; rollback uses the same verified restart path.
- Made controller integration checks read the public `component-versions.json` release asset directly and download known assets through GitHub's `releases/latest/download` URLs, avoiding unauthenticated GitHub Releases API rate limits.

## 1.21.0 - 2026-07-26

### Cockpit response, Track ambience and colour

- Replaced the old cascading adaptive-exposure windows with stable per-zone brightness controllers. Steady daylight no longer makes the exposure range close and fade the lights, while rapid mask changes through trees, tunnels and track lighting still drive immediate transitions according to Response smoothing.
- Renamed Light sensitivity to **Brightness boost** and clarified that it controls cockpit-zone gain, not transition speed. The same mask percentage now has a stable relationship with output while the controller still adapts to the usable brightness range of each zone.
- Added **Track ambience**, an optional independent lamp zone learned from moving track/world areas across the full capture. It works as one zone on single or triple screens, normally excludes the cockpit, combines several screen regions so one bright or dark area cannot dominate, uses its own fixed response curve and has an independent Track ambience boost.
- Added Track ambience learning and output diagnostics, plus a compact amber full-capture map on the Lights tab that clearly separates it from the combined cockpit **All** zone.
- Unified natural-illumination colour filtering across All, every cockpit zone and Track ambience. Coloured surfaces such as grass, walls, kerbs, dashboards and cockpit materials are pulled towards neutral light without changing brightness, while spatially supported warm sunlight is preserved.
- Hardened frame capture hand-off and Track ambience learning for day and night so temporary capture contention, dark scenery or a limited set of active screen regions no longer leaves the new zone unnecessarily unavailable.

### Controller integrations

- Added the **Controllers** tab and Bruno Silva SimHub Control Bridge integration for Elgato Stream Deck, Stream Dock / MiraBox, Ulanzi Studio and Logitech / Loupedeck. Stream Dock, Ulanzi and Logitech / Loupedeck support remains experimental until validated on physical hardware.
- Added ready-made controller actions for plugin power, selecting or displaying the current idle theme, previous/next theme, idle brightness, dark mode, idle hold, car-mask reset, output reset and low-fuel acknowledgement, with live names, percentages and on/off state.
- Added encoder support where the controller provides it: rotate to change idle theme or brightness, with fine brightness adjustment while held. Stream Deck + receives native dial/touch-strip feedback.
- Added per-action text sizing where supported, automatic icons and plugin-specific action groups so users do not need to prepare images or manually create commands.
- Added guided installation and component updates from inside Cockpit Auto Ambient Light. Controller-only updates restart the relevant controller software where possible; shared bridge updates always request permission before restarting SimHub.

### Light ownership and physical strip layouts

- Made how the Adalight and WLED strips are split into zones — the zone count, each zone's LED range and its name — shared by every game instead of stored per game. That split describes where the strip is physically mounted, so keeping a separate copy per game only meant re-drawing the same ranges for every sim. What each zone then does (cockpit zone, fixed colour, excluded, brightness limits, alerts, dark mode) still follows the per-game or Default device profile as before. On upgrade the split is taken from the Default profile, which is both the template new games are created from and what the Default profile mode already used; if it was never set up, the first configured game profile is used instead, and the plugin log records which one it adopted.
- Added an "Excluded lights" list: lights picked there are left entirely to the user's own SimHub effects. Every endpoint the plugin reached through the SimHub effects profile was written on every frame, and lights that were not selected for cockpit tracking were held at an opaque colour, so a light driven by the user's own effects container (for example from a Stream Deck script) was overwritten by the plugin. Excluded lights now receive a fully transparent colour instead, so whatever the user's own effects put on them comes through, and they are hidden from cockpit zones, themes, animation layers and alerts. The list is global rather than per game, and the light's other settings are kept, so removing it from the list restores its previous setup. WLED and Hue V2 direct lights are not listed because they are driven over the network and never reach the effects profile.
- Made Adalight LEDs outside every zone stay untouched when the outside brightness is 0%, instead of being held at opaque black, leaving them free for the user's own effects. Setups with an outside brightness above 0 are unchanged.
- Renamed the Adalight zone output mode "Disabled" to "Excluded" and made it write those LEDs fully transparent. It previously skipped the zone, which left its LEDs showing the outside-zone colour, so a zone switched off to free its LEDs stayed lit whenever an outside colour was set — and the LED range map showed them as free while the plugin was still painting them. Adalight zones can also be excluded on the new Excluded lights tab, which frees them on every game rather than only the current one; a zone excluded that way stays visible but locked on the Lights tab so the LED range it covers is still readable. WLED zones keep the name "Disabled" and their previous behaviour, because WLED is driven directly over the network and has no SimHub effect underneath to reveal.

### Brightness ranges, WLED and fixes

- Added a single navigable message centre for simultaneous plugin notices. Users can move between unread messages, mark only the current message or all of them as read, or close the window and keep the remaining notices for the next SimHub session.
- Fixed the plugin's effects containers accumulating as duplicates in a SimHub profile across sessions. The plugin injects a base-layer container (and an Adalight-zones container) into the active effects profile and removed it by object reference on shutdown; if a crash or a mid-session SimHub save persisted it, the next launch created a fresh container that never matched the stale copy, leaving a new duplicate each such session. The plugin now removes any leftover copies of its own containers (matched by their description) before placing the current one.

- Reworked WLED colour rendering around a single tone curve (the WLED firmware's default 2.2 gamma with a linear toe) applied to every frame the plugin sends. Previously themes/idle went out through a 2.8 gamma, live driving went out linear and previews got neither, so the same colour rendered differently per path: mid colours washed out while driving (WLED never gamma-corrects realtime UDP, only colours set on the device itself) and the brightness previews looked wrong. Colours now match the device's own picker while driving, and dim moving content stays on the linear part of the curve, so the low-brightness flicker and red/green colour shifts that gamma used to cause at night cannot return.
- Made the WLED per-zone brightness range map output duty directly, after the tone curve: a zone minimum now sits at exactly the value set and the light responds immediately as the cockpit brightens (before, the floor rendered well below the set value and the rise out of it felt stuck), and the zone brightness preview shows exactly what the range produces while driving.
- Restored the per-light minimum output brightness, alongside the existing maximum and luminosity gamma. It raises the bottom of the cockpit/dynamic output range for lights that only become visible above a certain level, such as diffused or softbox fixtures, and it lives under "Advanced & effects" so it stays out of the way. Left at 0 (the default) nothing changes. Adalight and WLED zones keep their maximum and gamma only for now.
- Made a dark scene drive each light to the bottom of its own output range instead of always to black, so the Dark Cutoff no longer overrides a light's configured minimum. With the default minimum of 0 the bottom is still black, so lights without a minimum behave exactly as before.
- Kept a light with a minimum lit through a fully black cockpit, where it previously dropped out: such a scene now holds the light at its minimum in a neutral tone, since a black signal carries no colour to follow. Lights with no minimum still go fully dark, and no light is ever lit while the plugin is not driving it.
- Fixed the Dark Cutoff judging darkness from a light's already-limited output rather than from the cockpit signal, which made two lights in the same zone cut out at different scene brightness purely because their maximums differed. Only affects setups with a Dark Cutoff above 0 and a maximum below 100%.
- Added live brightness previews while dragging lamp and strip-zone controls.

## 1.20.0 - 2026-07-07

- Added WLED as a direct network output: WLED controllers (ESP8266/ESP32) are driven over realtime UDP (DRGB/DRGBW), independent of SimHub Ambient Lights, with no Adalight, app, cloud or Effects index.
- Added WLED device discovery by IP or `wled-xxxx.local`, plus a Scan that uses mDNS with a local subnet fallback (and a retry) so it still finds devices on networks that block mDNS, showing a pick list of what it found.
- Added WLED virtual zones mapped to LED ranges (cockpit zone, fixed colour or disabled), an outside-zone colour, per-zone alert effects, and WLED zones as fixed-output theme endpoints.
- Added per-device WLED controls: editable/auto-unique names, Identify, Open web page, LED count and realtime UDP port.
- Gamma-corrected WLED colour automatically to match the other outputs and the WLED colour picker: a fixed 2.8 curve for idle, themes and the dark-mode base, and a linear curve while driving to avoid low-brightness flicker, plus the same per-zone luminosity gamma control as Adalight.
- Made WLED behave like the other outputs across all states: cockpit while driving, idle otherwise (including while the mask is still learning), and off when output is disabled.
- Made the alert-effect previews drive WLED zones, not only Hue and Adalight.
- Made WLED consistent with the other output types: the master enable and per-device zone routing are saved per game (honouring the global all-games profile mode and Copy device profile), while the device hardware and its Enabled switch stay global.
- Redesigned the plugin interface, and moved license activation and status into their own License tab with the API status shown in the top bar.
- Added colour palettes to animation layers: a layer can cycle or randomise through a palette of colours over time or along the lights, instead of only fixed colours.
- Added a global (all-games) profile mode so devices and light response can be shared across every game instead of kept per game, alongside the existing per-game and Copy profile options.

## 1.17.1 - 2026-06-23

- Added a short trial lease renewal grace period so idle animations do not flash back to static base colours during transient refresh gaps.
- Hardened Direct Hue V2 frame output so duplicate logical endpoints resolve to one colour per Entertainment channel.
- Kept Direct Hue V2 unselected/base output from competing with selected endpoints that share the same physical channel.
- Fixed Direct Hue V2 segment labels so multi-segment lights show Segment 1 as well as later segments.
- Removed the incorrect optional deploy copy path for `SimHub\Plugins`; SimHub plugin DLL deployment uses the SimHub root folder.

## 1.17.0 - 2026-06-23

- Added ten fixed-output theme slots for idle, manual idle hold and VR output.
- Added independent theme-layer scopes for idle animation layers and driving unselected lights.
- Added driving unselected light animations so lights that are not selected for cockpit output can still animate while driving.
- Kept selected cockpit-output lights out of driving unselected layers automatically, with ignored lights shown in the layer list.
- Added live preview support for the selected theme-layer scope while editing.
- Added theme layer ordering controls and clearer row headers for order-based and position-based animations.
- Simplified theme layer endpoint rows by removing bridge/IP/channel detail from assigned layer lists.
- Added Identify buttons in assigned and available theme layer light lists.
- Kept saved animation layers intact when activation is required, while applying only static theme base output until activated.
- Expanded the animation effect list with Sparkle breath, Comet sweep, Scanner sweep, Mirror pulse, Side alert, Aurora wave and Police.
- Reworked overlapping/unclear effects: Sweep fill now replaces Wipe, Center out and Edge in cover middle/edge motion, and Mirror sweep combines both directions.
- Changed layer speed to milliseconds, from 50 ms to 10000 ms.
- Improved effect brightness handling so effects can reach the configured brightness.
- Added optional second-colour support where useful and effect-specific labels for clearer setup.
- Reworked Police into a two-colour emergency pattern with split bursts, ping-pong, in/out and full-sync strobe phases.
- Updated the plugin guide and public README for the current Themes workflow and animation setup.
- Fixed per-game device profile persistence when changing selected lights before live telemetry starts.
