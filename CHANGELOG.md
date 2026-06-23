# Changelog

## 1.17.1 - 2026-06-23

### Fixed

- Fixed a brief white/base-colour flash that could happen during transient trial lease refresh gaps.
- Improved Direct Hue V2 frame output so duplicate logical endpoints resolve to one colour per physical Entertainment channel.
- Kept Direct Hue V2 base output from competing with selected endpoints that share the same physical channel.
- Fixed multi-segment light labels so Segment 1 is shown when a light has more than one segment.

## 1.17.0 - 2026-06-23

### Added

- Added ten fixed-output theme slots for idle, manual idle hold and VR output.
- Added independent theme-layer scopes for idle animation layers and driving unselected lights.
- Added driving unselected light animations so lights that are not selected for cockpit output can still animate while driving.
- Added live preview support for the selected theme-layer scope while editing.
- Expanded the animation effect list with Sparkle breath, Comet sweep, Scanner sweep, Mirror pulse, Side alert, Aurora wave and Police.

### Changed

- Improved theme layer ordering controls and row headers for order-based and position-based animations.
- Simplified theme layer endpoint rows by removing bridge/IP/channel detail from assigned layer lists.
- Changed layer speed to milliseconds, from 50 ms to 10000 ms.
- Improved effect brightness handling so effects can reach the configured brightness.
- Added optional second-colour support where useful and effect-specific labels for clearer setup.
- Reworked Police into a two-colour emergency pattern with split bursts, ping-pong, in/out and full-sync strobe phases.
- Updated the plugin guide and public README for the current Themes workflow and animation setup.

### Fixed

- Kept selected cockpit-output lights out of driving unselected layers automatically, with ignored lights shown in the layer list.
- Fixed per-game device profile persistence when changing selected lights before live telemetry starts.

## 1.14.2

### Changed

- Documented that Philips Hue Native V1 and Direct V2 can expose the same physical lights with different names and endpoint IDs, so per-endpoint theme colours may need review after switching backend.

### Fixed

- Fixed Direct Hue V2 Identify so disabled Entertainment Areas cannot flash lights through the fallback Hue API path.
- Fixed Direct Hue V2 discovery, theme endpoint lists and fixed-output snapshots so disabled Entertainment Areas are not treated as active endpoints.

## 1.14.0

### Changed

- Improved Philips Hue Direct V2 connection handling and sync recovery.
- Direct V2 can now manage multiple Hue Entertainment Areas, including setups spread across more than one Hue bridge.
- Direct V2 area controls are clearer, with per-area enable, resync, forget and sync status controls.
- Improved Direct V2 bridge and Entertainment Area discovery, including Bridge Pro-compatible local API discovery.
- Improved Hue gradient light handling when several reported members share the same Entertainment channel.
- Improved Devices UI grouping for Hue Direct V2 areas and segments.
- Improved profile copy behaviour for Hue Native V1 and Direct V2 device profiles.
- Improved idle-to-live output arming so supported games can leave idle output from ignition, engine, RPM or movement signals.
- Improved project documentation and troubleshooting notes.

### Fixed

- Fixed cases where old or pending Direct V2 sync sessions could prevent a new sync from becoming active.
- Fixed Direct V2 resync behaviour after reset or bridge ownership changes.
- Fixed duplicate Hue gradient channel rows that could make several UI rows identify or flash together.
- Fixed several Direct V2 output ownership edge cases when SimHub native Hue output and Direct V2 were both present.
- Fixed several small Devices tab layout and state-refresh issues.
