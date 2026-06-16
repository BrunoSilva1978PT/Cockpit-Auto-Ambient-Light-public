# Changelog

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
