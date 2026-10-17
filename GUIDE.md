# Cockpit Auto Ambient Light Guide

This guide mirrors the built-in plugin Guide. Use the Guide tab in SimHub for the same instructions.

## What the plugin does

Cockpit Auto Ambient Light watches SimHub's screen capture, learns the stable cockpit area, and drives selected Ambient Light devices from the cockpit brightness and colour.

The goal is to make physical cockpit lights react to sun, shadow and night lighting inside the car. The plugin owns the lights it drives, and its built-in alert effects can override cockpit and dark-mode output per lamp. For devices driven through SimHub, hand a light back to your own effects by ticking it on the Excluded lights tab.

Alert effects can also be assigned to unticked endpoints. That lets some lights follow the cockpit while other lights stay effects-only.

The plugin uses SimHub's native Ambient Lights capture engine instead of opening a second capture pipeline.

Native SimHub Ambient Lights is still the default device source. Configure and enable Hue V1, Govee, Bluetooth and Adalight devices in SimHub Ambient Lights first, then pick which endpoints this plugin should control.

Direct Hue V2 is the exception. It pairs directly with the Hue bridge, including Bridge Pro installations that expose the same local Hue API, discovers Hue V2 Entertainment lights and gradient segments, and sends output through Hue Entertainment sync instead of SimHub's native Hue V1 output.

Govee Direct LAN also bypasses SimHub output. It discovers and drives chosen LAN Control devices itself, and automatically disables their native SimHub entries to prevent UDP conflicts. Because there is no SimHub effect underneath, Direct LAN lights are not offered on the Excluded lights tab.

WLED is the other direct path. WLED network LED controllers (ESP8266/ESP32) are driven straight over your local network using WLED's realtime UDP protocol, without SimHub Adalight, an app, the cloud or a SimHub Effects index. You add them on the Hardware tab by IP or by scanning, then split each controller into zones the same way as Adalight.

It also does not rewrite SimHub Ambient Lights effect indexes. Each lamp or segment should have its own unique Effects index in SimHub, otherwise multiple endpoints can share one output slot and appear to ignore their cockpit zone or alert assignment.

Each output type on the Lights setup tab has its own Enabled / Disabled for this profile switch. Turning a type off suspends this plugin's control for that type without deleting saved selections, zones, brightness limits, fixed outputs or alert permissions.

## First setup

1. Update SimHub to the latest available version before troubleshooting or reporting setup issues. Older builds can expose different Ambient Lights capture, effects or device behaviour.

2. For native output, configure Philips Hue V1, Govee, Bluetooth or Adalight devices in SimHub's Ambient Lights plugin. Check that every lamp or segment has a different Effects index in SimHub Ambient Lights.

3. For Direct Hue V2, open Hardware, select Direct V2, pair the bridge, then choose a Hue Entertainment Area. Hue lights controlled by Direct V2 should not also be active in SimHub's native Hue V1 group at the same time.

4. For Govee Direct LAN, enable LAN Control in Govee Home, close Govee Desktop, open Hardware, select Direct LAN, then use Add devices to scan and choose the devices the plugin should own.

5. Make sure the SimuLight / Ambient Lights master output is enabled.

6. Keep Enable plugin on in Home. On Lights setup, choose the Global output layout: Global for one shared cockpit value, or Cockpit zones / triples for per-light zone routing.

7. Open the Lights setup tab, enable the needed output types for this profile, refresh the list, tick the lights this plugin should control, then adjust each Hue lamp, Hue V2 segment or Govee device cockpit max brightness limit if needed. Also choose which alert effects each endpoint accepts, including unticked effects-only lights.

8. For WLED, add each controller on Hardware by IP (for example 192.168.1.50) or with Scan, then set its LED count and physical zones. On Lights setup, enable WLED for this profile and choose what each zone does. Set that device's brightness to 100% in its own WLED web page.

9. Optional: open the Themes tab to create the fixed-output themes used for idle output, manual idle hold and VR output.

10. Optional: open the Effects tab to tune built-in alerts such as flags, spotter, Custom 1, Custom 2, low fuel, redline, pit limiter, engine start, pedals, wheel lock and wheel spin.

11. Start driving in cockpit camera. The mask learns automatically while the car moves.

## Activation and trial

Cockpit Auto Ambient Light is paid software with an online live-output trial per PC. During the trial, discovery, pairing, setup and output all work normally.

The trial uses short online leases from the activation API. If the internet or API is unavailable, live output keeps running only until the current lease expires; after that, all configuration pages stay available, but live cockpit output and alert effects stop. Selected lights are held on the configured idle theme until the plugin can confirm remaining trial time or is activated.

Buy lifetime access through the Ko-fi product page, then enter the same Ko-fi checkout email on the License page. Use Send activation code, check that email inbox, then enter the 6-digit code to activate this PC. The email is locked after activation so the local activation stays tied to the purchase account.

A license is valid for one active PC at a time. To move it, use Deactivate this PC on the currently activated PC, then activate the new PC with the same Ko-fi checkout email and a fresh activation code.

If Windows was formatted, the PC was replaced, or the old PC is no longer available, contact support so the old PC activation can be deactivated for you.

First activation requires the activation API to be online. After that, the plugin keeps a local offline activation and refreshes it periodically, so a temporary server or internet problem will not immediately stop an already activated PC.

The License page shows the current activation status and refreshes it automatically. Use Deactivate this PC before moving the activation away from this PC.

If activation fails, the wrong email was used, Windows was reinstalled, the one-PC activation limit was reached, or a PC was deactivated by mistake, use Contact support or Copy support info from the License page, or email bruno.1978@gmail.com.

## Cockpit mask learning

The last accepted mask is stored per game and car, then reused as a startup seed. While you drive, the plugin keeps learning a candidate mask in the background.

If the candidate stays different for long enough, the plugin promotes it automatically. This covers FOV changes, seat position changes and different cockpit views.

A stable game plus car change saves the previous mask, loads the new car's stored mask when available, or starts a fresh learn after a short debounce.

### Best results

Use a stable cockpit camera while the mask learns. Camera shake, look-to-apex/head movement and large HUD or overlay elements over the cockpit view can confuse the detector because they change what appears stable inside the lower screen area.

### Manual reset

Use Reset learned mask only when the brightness response is clearly wrong. It resets the detector and restarts the capture.

## Lighting modes

### Global

Every selected lamp follows the combined cockpit signal. This is the safest mode for most rigs.

### Cockpit zones / triples

Wide/triple captures split the learned mask into cockpit zones for multi-light rigs. Single-screen captures use adaptive mask zones instead of raw screen columns. Lamps can follow All, Left outer, Left inner, Center-left, Center-right, Right inner, Right outer, Lower cockpit, or the lower cockpit left/center/right subzones. Left, Center and Right remain available as combined outputs for simpler rigs. Assign lamps to zones on the Lights setup tab. One shared detector estimates illumination colour for All, every cockpit zone and Track ambience. Surface colour protection is enabled by default and keeps plausible cool, neutral and warm white illumination while suppressing strong material colours. Turn it off in Light response to use the captured mask colour. Colour amount then controls that colour: 0% removes it and 100% preserves it. Colour warmth (previously Sun warmth) applies afterwards: 0% adds no warmth and 100% gives whites a strong warm tone, even in bright scenes or with Colour amount at 0%. Saved Sun warmth values are retained. Night-time bright-point rejection and exposure calculate brightness separately from colour. RGB channels are scaled together; strong colour or warmth may reach the hardware limit before the requested brightness. These controls do not recolour alerts, output themes, dark mode base colour or fixed outputs.

### Track ambience (optional)

In Cockpit zones / triples mode, Track ambience is one independent lamp zone, not another left/centre/right cockpit split. The amber outline on the Lights map shows its full-capture source; it remains one selectable zone in both single-screen and triple-screen layouts. It learns the parts of the full capture that repeatedly change structure while driving, and combines six hidden screen regions so one bright area cannot dominate the result. Learning works progressively by day or night: darkness or temporarily static scenery keeps previous evidence instead of erasing it. Blocks already classified as cockpit are accepted only when stronger repeated motion and nearby support indicate that they are really track scenery. Assign it explicitly to a lamp or LED zone on the Lights setup tab; All never includes it, and leaving it unassigned has no effect on existing cockpit output. Its brightness uses a fixed response curve rather than adaptive exposure. Track ambience boost in Light response controls that curve independently: 0% sends the raw Track mask brightness, 100% keeps the default response, and higher values strengthen it. Its colour follows the same Surface colour protection choice as All and the cockpit zones; it uses the same cool/neutral/warm-white filter and the same Colour amount and Colour warmth controls. Because it only sees pixels, it can still react to headlights, fog, illuminated signs and track floodlights, especially at night.

### Light response profiles

Brightness boost (previously called Light sensitivity), Track ambience boost, response smoothing, Surface colour protection, Colour amount, Colour warmth (previously Sun warmth), Dark-mode cockpit blend start and Dark cutoff are configured on the Light response page. The current game profile loads from the game selected by SimHub, so these values can be edited before live telemetry starts. When a game is selected for the first time, the plugin creates its Light response profile from the Global Light response settings in the background. Keep Brightness boost at 100% and smoothing at 50% for the default response. Smoothing 50% matches the previous 0% default; old profiles are migrated to preserve their existing response. Lower smoothing now means a faster, more aggressive response, while higher smoothing slows transitions for trucks, open cockpits or games with rapid shadow changes. Brightness boost controls the adaptive exposure gain of the cockpit zones only, not its response speed. Lower values keep cockpit output closer to the raw mask brightness; higher values make the cockpit lights reach maximum brightness with a lower mask percentage. Track ambience boost is independent: 0% sends raw Track mask brightness, 100% keeps its default fixed-curve response, and higher values strengthen only Track ambience. It never enables adaptive min/max exposure for Track ambience. Response smoothing controls how quickly transitions occur for both outputs. Dark-mode cockpit blend start controls when that game's adaptive cockpit brightness starts layering over the dark-mode base colour. Dark cutoff turns very low cockpit output fully off and fades back in through a short soft ramp above the cutoff. Mask learning is unchanged by these sliders. Use Copy from on the Light response row to apply another game's response profile, or the Global Light response profile, to the current game only. The confirmation dialog lists the values that will be replaced and Lights setup selections are left unchanged.

### Lights setup profiles

Light selections, cockpit zones, cockpit max brightness limits, unselected fixed outputs, per-lamp dark-mode routing and per-lamp alert permissions can be saved per game on the Lights setup tab. Driving brightness is on Home and is shared by every game and Streaming, regardless of the selected profile. The current game profile loads from the game selected by SimHub. When a game is selected for the first time, the plugin automatically creates that game's Lights setup profile from the Global settings in the background. With no active game, changes on the Lights setup tab edit those Global settings for future games. This lets truck, car and other simulator layouts use different lamp routing without creating separate settings per vehicle. Use Copy from on the Lights setup row to apply another game's profile, or the Global Lights setup profile, to the current game only. The copy replaces selected endpoints, zones, output ranges, fixed unselected output, dark-mode routing and alert permissions; Light response, global driving brightness and cockpit masks are left unchanged. If Hue backend IDs differ between Native V1 and Direct V2, the plugin translates matching endpoint settings when the current bridge/area map is available.

One thing is never per game: how the Adalight and WLED strips are split into zones — the zone count, each zone's LED range and its name. Those describe where the strip is physically mounted, so they are configured on Hardware, shared by every game and by both profile modes, and copying a Lights setup profile does not change them. Everything each zone then does is part of the profile as usual.

### Output type switches

Hue, Govee, Bluetooth, Adalight and WLED each have an Enabled / Disabled for this profile switch on the Lights setup tab. Disabling one type stops this plugin from controlling that type in the current Lights setup profile, but keeps all saved selections and tuning. Re-enable it later to restore the same routing. Disabling Hue also releases Direct Hue V2 Entertainment sync and restores the matching SimHub native Hue group if this plugin had disabled it.

### Philips Hue backends

Native V1 uses SimHub Ambient Lights as the Hue backend. Configure the Hue bridge and entertainment group in SimHub, then select the Hue endpoints in this plugin. This is the safest default path and behaves like other SimHub Ambient Lights devices.

Direct V2 uses this plugin's saved Hue bridge pairing and selected Hue Entertainment Area. It discovers Hue V2 lights directly and sends identify, previews, idle output, cockpit output and built-in alert effects through Hue Entertainment sync. Bridge discovery uses Hue cloud discovery, Hue mDNS discovery and local HTTPS probing, so normal Hue bridges and Bridge Pro setups can be found when they expose the same local Hue API. A Hue bridge only allows one Entertainment sync owner at a time, so the matching SimHub native Hue group is disabled while Direct V2 owns it and restored when Direct V2 is released, disabled or forgotten.

Direct V2 can only control lights and channels that belong to the selected Hue Entertainment Area. If a light is on the bridge but not in that area, it will not appear as a controllable endpoint here. Disabled Direct V2 areas stay saved, but their endpoints are not used by Identify, themes or live output until that area is enabled again.

Native V1 and Direct V2 can expose the same physical Hue lights with different names and endpoint IDs. V1 usually follows SimHub's group order, while Direct V2 uses Hue's real light names and Entertainment channel IDs. The plugin copies matching theme and device settings when a safe V1/V2 map is available, but switching backend can still require reviewing Themes.

Use Resync if the Hue app or SimHub native Hue takes over the same Entertainment Area. Use Forget Direct V2 bridge to remove only this plugin's saved Hue pairing and return to SimHub native Hue V1. Forget does not delete anything from the Hue bridge itself.

### Hue gradient lights

Hue V2 gradient-capable lights are represented by their Entertainment channels. A gradient strip can therefore appear as multiple segment rows in Direct V2, and each segment can have its own cockpit zone, max brightness limit, dark-mode permission and alert permissions. Identify and output also target the selected segment/channel instead of treating the whole gradient light as one lamp when the bridge exposes separate channels.

### Cockpit max brightness limit

Cockpit max brightness is configured on the Lights setup tab and applies only to cockpit/dynamic output. It caps brightness but does not force lamps to stay on. Idle and VR output use themes edited on the Themes tab. Built-in alert effects use the Alert brightness control from the Effects tab. These limits are part of the current game's Lights setup profile. Philips Hue rows, Direct V2 segment rows and Bluetooth rows use their own cockpit max limit per endpoint; Govee rows share one cockpit max limit per device group while each segment still keeps its own zone selection.

### Unselected lamps

Lights enabled in SimHub Ambient Light but not ticked in this plugin can use their own fixed colour and brightness on the Lights setup tab. 0% brightness keeps that unticked endpoint dark. Unticked endpoints can still accept built-in alert effects, so you can reserve lamps for alerts only. These choices are saved per game. To leave an endpoint to your own SimHub effects instead of holding it at a fixed colour, use the Excluded lights tab.

### Excluded lights

The Excluded lights tab lists the lights the plugin drives through the SimHub Ambient Lights effects profile: native Hue V1, Govee and generic Bluetooth endpoints. Tick Exclude on a light to hand it back to your own SimHub effects profile: the plugin then sends that light a fully transparent colour, so whatever your own containers put on it comes through untouched. Use it for a light you drive yourself, for example from a Stream Deck script or a computed-property colour in the base ambient plugin.

An excluded light disappears from cockpit zone selection, themes, animation layers and alert effects, because the plugin no longer drives it at all. Its other settings are kept, so removing it from the list restores exactly how it was set up before.

This list is global: it is not saved per game, unlike Lights setup profiles and Light response. WLED, Direct Hue V2 and Govee Direct LAN lights are not listed because the plugin drives them over the network instead of through the effects profile, so there is no SimHub effect underneath to show through. A stored native Govee exclusion is ignored while Direct LAN is active and becomes effective again if that device returns to Native.

Adalight zones are listed too. Excluding a zone here frees its LEDs on every game; to free it for the current game only, set that zone's output mode to Excluded on the Lights setup tab. Either way the plugin writes those LEDs fully transparent. A zone excluded globally stays visible on the Lights setup tab, locked, so you can still see which LEDs it covers. LEDs outside every zone are left alone as well while the outside brightness is 0%.

### Effects priority

Cockpit Auto Ambient Light writes its own layer through SimHub's colour effects profile, on top of the other containers, so effects from the current Ambient Lights profile do not run on the lights it drives. This is what keeps the cockpit light visible instead of being hidden by a continuous effect. Built-in alert effects from the Effects tab can override cockpit or dark-mode output per lamp, and can also drive unticked effects-only endpoints.

Lights you would rather keep for your own effects are set on the Excluded lights tab: the plugin sends those a fully transparent colour and stops driving them altogether.

Live driving uses cockpit or VR output. After pause, replay, menus, no game, waiting for data or no learned mask, the plugin keeps the idle theme until the first ignition, engine, RPM or movement signal arms live cockpit output. Once armed, stopping the car or switching ignition off does not by itself return to idle; the cycle resets when runtime becomes unavailable again.

Hold idle theme can be bound to a button on the Controls page. It temporarily keeps available theme endpoints on the idle theme until pressed again. It needs at least one light ticked for cockpit output.

### Streaming mode

Streaming mode is one global device override, independent from the normal Global or per-game Lights setup profiles. Configure every available endpoint on the Streaming mode tab. Ticked endpoints follow their cockpit zone while driving; unticked endpoints keep their fixed output and can still use the current general theme's Driving unselected animation layers. Alert and dark-mode permissions also come from the Streaming profile.

Streaming changes output only while driving. In menus, idle and before live driving is armed, the normal global idle theme runs exactly as it does with Streaming off. Hold idle stays above Streaming. One global driving master brightness is shared by normal and Streaming driving output and is applied last to all cockpit, fixed, animation and alert output. Copy current Lights setup replaces only the Streaming endpoint profile after confirmation; it never changes themes or any normal game profile.

OBS automatic control is optional. In OBS open Tools > WebSocket Server Settings, enable the WebSocket server, then enter its host and port in the plugin. Port 4455 is the OBS default. Enable Automatically enable while OBS is live after applying the connection settings. The plugin watches OBS's real streaming output state, so merely opening OBS does not activate Streaming mode.

When Enable Authentication is active in OBS, use Show Connect Info in the same OBS dialog and copy its Server Password into WebSocket password (optional) in the plugin. This is an OBS WebSocket password, not an OBS, Twitch or YouTube account password. The plugin protects it for the current Windows user. Authentication may instead be disabled in OBS; in that case leave the plugin password empty. Keeping authentication enabled is recommended, especially when OBS is reachable from the LAN.

### VR mode

Automatic experimental VR detection checks OpenXR Toolkit's running app and SteamVR/OpenVR's current scene process while SimHub has an active game. If either signal is active, available theme endpoints use the VR theme selected on the VR mode page. SimHub property remains the most reliable source for custom VR detectors or button/plugin workflows.

### Govee devices

Govee devices are grouped by SimHub controller/device, but each segment is listed as its own selectable light endpoint. Use Identify per segment to map the physical layout.

### Govee Direct LAN compatibility

Known realtime cockpit models: H6047, H6056, H6061, H6062, H6065, H6066, H6067, H6072, H619A, H619B, H619C, H619D, H619E, H619Z, H61A0, H61A1, H61A2, H61A3, H61A5, H61C2, H61C3, H61E0, H61E1.

Other LAN-enabled Govee models may still appear in Add devices and work as a single zone, but their realtime cockpit response is not guaranteed; they are best suited to idle themes and animations. Direct LAN also bypasses SimHub native Govee compatibility issues seen with some newer firmware revisions. Enable LAN Control in Govee Home and close Govee Desktop before scanning.

Govee Direct LAN devices do not appear under Excluded lights: Direct LAN owns their UDP output, so a transparent SimHub effect cannot hand them to another container. Untick segments to stop using them for cockpit output, remove the device from Direct LAN, or switch the Govee backend to Native before assigning that physical light to your own SimHub effects.

### Bluetooth and Adalight

Generic Bluetooth lights are treated as single-zone endpoints from SimHub Ambient Lights. Adalight uses SimHub's configured physical Adalight output and maps this plugin's virtual zones to LED ranges. The outside-zone colour and brightness, and what each zone does, are saved in the current Lights setup profile and are not deleted when Adalight is disabled.

How the strip is split is shared by every game. The number of zones, each zone's LED range and its name are not saved per game — they describe where the strip is physically mounted, which does not change from one sim to the next. Editing them on the Hardware tab changes them everywhere, in Per game and Global mode alike, and copying a Lights setup profile leaves them untouched. This applies to WLED strips exactly the same way.

What each zone does is a different matter and stays with the Lights setup profile: output mode, cockpit zone, brightness limits, fixed colour, alert permissions and dark-mode routing all follow Per game or Global like every other Lights setup setting. So the same physical zone can drive a different cockpit area in a truck than in a car.

Each Adalight zone outputs Ambient, a Fixed colour, or Excluded. Excluded writes those LEDs fully transparent, leaving them to your own SimHub effects, and the LED range map marks them so you can see which stretch was handed over. LEDs not covered by any zone take the outside-zone colour, or are left alone too while its brightness is 0%. WLED zones offer the same choice as Disabled: WLED is driven directly over the network, so there is no SimHub effect underneath and those LEDs simply take the outside colour.

### WLED (network LEDs)

WLED controllers are driven directly over your network with WLED's realtime UDP protocol, not through SimHub Ambient Lights, so they need no SimHub Adalight, app, cloud or Effects index. Add a device by IP or wled-xxxx.local, or press Scan. Scan uses mDNS and also sweeps the local subnet, so it still finds devices on networks that block mDNS; it shows what it found and lets you pick which to add. If nothing appears on the first scan, run it once more.

Each device shows its IP, LED count and reachability, with Rename (duplicate WLED names are also numbered automatically), Identify and Open (its web page). Set the LED count and, if needed, the realtime UDP port. A device is split into virtual zones mapped to LED ranges just like Adalight: each zone can follow a cockpit zone, show a fixed colour or be disabled, and can accept alert effects and appear in fixed-output themes. LEDs outside any zone use a separate outside colour and brightness.

How a strip is split — the number of zones, their LED ranges and their names — is shared by every game, for WLED and Adalight alike, because it describes where the strip is physically mounted. What each zone then does (cockpit zone, fixed colour, excluded, brightness limits, alerts, dark mode) is part of the Lights setup profile, so it follows Per game or Global like every other Lights setup setting.

Set each device's brightness to 100% in its WLED web page, because WLED scales the plugin's output by its own brightness. WLED colour is gamma-corrected automatically: WLED does not apply gamma to realtime data, so the plugin does it here to match the WLED colour picker — a fixed 2.8 curve for idle, themes and the dark-mode base, and a linear curve while driving so low-brightness colours do not flicker. Leave the gamma options in the WLED web page as they are. Reserving a static IP per device in your router keeps it from changing.

Like the other direct output types, each WLED device and its Device enabled switch are global (shared by all games), while the WLED Enabled for this profile switch and each zone's behaviour are saved per game. Choosing Global for Lights setup shares them across games instead, and copying a Lights setup profile brings the WLED zone behaviour along without changing the physical layout.

### Ambient Lights effect indexes

SimHub routes Ambient Lights colour effects through indexes. Keep one unique Effects index per physical lamp, Hue segment, Govee segment or other endpoint. If two endpoints share the same index, they can mirror the same output even when this plugin assigns them to different cockpit zones. The plugin warns about this setup requirement but leaves the SimHub indexes untouched.

## Output themes

Themes are fixed-output presets edited on the Themes tab. They are used when the plugin needs steady output instead of live cockpit capture, such as idle output, manual idle hold and VR output. Idle theme is selected on Home or the Themes page; VR theme is selected on the VR mode page.

There are ten theme slots. Each slot can be renamed and set to Single colour or Custom per endpoint. Single colour applies one colour and brightness to every available endpoint. Custom per endpoint lets each endpoint keep its own colour and brightness, with Use default returning an endpoint to the theme default.

Each theme can also use up to eight animation layers. A layer overrides the static base only for the endpoints assigned to it, and each endpoint can belong to one layer at a time. Endpoints not assigned to a layer keep the theme's single-colour or custom-per-endpoint base output. See the Animations: idle and driving topic for how the idle and driving-unselected layer sets work and when each one plays.

Layer speed is set in milliseconds from 50 ms to 10000 ms. The brightness controls set the base/resting brightness and the effect brightness. Effects that support two colours show an effect colour plus a base colour; on some effects the base colour is optional. Police uses a single Strobe brightness instead, with the lights off between flashes.

### Animation layer effects

Whole-layer effects: Breathing, Sparkle breath, Colour fade, Heartbeat, Twinkle, Confetti, Random burst, Flash and Strobe burst.

Ordered effects: Marquee, Runner, Comet, Comet sweep, Scanner, Scanner sweep, Sweep fill, Center out, Edge in, Mirror sweep, Mirror pulse and Aurora wave. Move rows in Light order in this layer until the animation follows the physical strip or lamp layout. Center out, Edge in, Mirror sweep, Mirror pulse, Aurora wave and the sequence effects expect the order to run from one physical end to the other.

Position effects: Side pulse, Side alert and Police use the Left, Center and Right assignment on each light instead of row order, so they are best for left/right rigs. Police runs split bursts, ping-pong, in/out and full-sync strobe phases.

The Effect help panel changes with the selected effect. It shows the best setup, whether the layer needs order or positions, and which colours or speed ranges make the effect easier to tune.

Press Edit before changing a theme. Edit mode previews changes live on the lights, Identify can be used to confirm the physical endpoint, Save stores the theme and returns the lights to normal output, and Cancel restores the previous theme values.

The endpoint lists are grouped by output type: Philips Hue, Govee, Bluetooth, Adalight and WLED. Themes use available endpoints for enabled output types, not only endpoints ticked for cockpit output on the Lights setup tab. Adalight shows no endpoints until a real SimHub Adalight output is configured, enabled and connected. WLED zones appear once a WLED device is added.

Hue theme colours are stored per endpoint ID. Because native V1 and Direct V2 expose different Hue endpoint IDs and display names, changing backend is treated like changing the Hue source. Review custom per-endpoint theme colours after switching backend.

The Controls page exposes Previous idle theme and Next idle theme bindings. These cycle the idle theme selected on Home or the Themes page and provide StreamDeck Control Mapper roles named Cockpit Ambient - Previous idle theme and Cockpit Ambient - Next idle theme.

Global idle master brightness scales the completed idle theme from 0 to 100%. It is applied after per-light brightness and idle animation frames are calculated, so dimmer lights stay proportionally dimmer. It does not change live cockpit, VR or alert-effect brightness.

## Animations: idle and driving (theme layers)

A theme layer is a moving effect - pulse, sweep, breathing and more - played on top of a theme's base colour. A theme can hold several layers, and each light belongs to one layer at a time. Lights not added to any layer simply show the theme's base colour.

### Two kinds of layers - and when each one plays

On the Themes tab, under Theme layers, the Edit layers for: tabs switch between two separate sets of layers. Both sets are saved at the same time - the tabs only change which set you are editing right now.

Idle animation layers play while this theme is the active idle output (the idle theme is chosen on the Themes tab). Use them for a calm resting effect with no game, in menus, while paused/replaying, or while waiting for live data. Stopping or parking the car does not itself enter idle; the current cockpit output remains active.

Driving unselected lights play while you are driving, on the lights you did not tick on the Lights setup tab. Ticked lights follow the live cockpit brightness; unticked lights would otherwise sit on a fixed colour, so this lets them run an animation instead - for example a slow breathing on rear or side lamps while the cockpit lamps react to the track.

### Building a layer

1. Open the Themes tab and press Edit on the theme.

2. Under Theme layers, pick the tab you want: Idle animation layers, or Driving unselected lights.

3. Press Add layer.

4. Add lights to the layer from the available list on the right. Identify confirms which physical lamp is which.

5. Choose an Effect, then set the speed, brightness and colours.

6. Press Save. The lights return to normal output.

The available light lists differ per tab: the Idle tab offers the theme's base lights, while the Driving tab offers the lights left unticked on the Lights setup tab. The Effect help panel on the right explains each effect and whether it needs a particular light order or left/center/right positions.

Tip: idle animations look best slow and gentle. Driving animations work well on lamps that are not tracking the cockpit, so they add motion without fighting the live reaction.

## Built-in alert effects

Each endpoint decides which alerts it accepts on the Lights setup tab, even when it is not ticked for cockpit output. Those endpoint permissions are saved in the current game's device profile. Lights on the Excluded lights tab never show alerts, because the plugin does not drive them at all. Direct network outputs are not part of that list; their alert permission is controlled directly on the Lights setup tab.

### Per-lamp priority

Priority is resolved per lamp, so different lamps can show different alerts at the same time. Pit limiter and engine start are always highest priority. The Effects tab lets you reorder spotter, racing flags, Custom 1, Custom 2, wheel lock, wheel spin, low fuel, redline and pedals. Normal cockpit, VR, dark-mode or idle output stays underneath alert effects.

### Racing flags

Configure each flag separately. You can disable a noisy flag, choose Static, Blink or Pulse, set its blink speed, and optionally show it only for a fixed duration. Duration hides a flag until the sim clears it and raises it again. Yellow also has a trigger shared across games: Driver's zone uses the local player flag where the sim provides one, or matches the current sector to that sector's yellow state. Games without local or sector telemetry fall back to SimHub's standard yellow signal. All reported yellows keeps the previous behaviour. If multiple flags are active at once, the flag priority list on the Effects tab decides which one wins.

### Spotter

Spotter uses the same colour and blink speed, but Lights setup lets you decide which lamps accept left-side and right-side spotter alerts.

### Low fuel

Low fuel can use percent or liters. There are separate low and critical thresholds, and each stage can be Static or Blink. The acknowledge button hides the current low or critical alert until the fuel stage clears or changes.

### Redline, pit limiter and engine start

Redline uses SimHub's current car/gear redline automatically. Pit limiter alternates between two user colours and can either flash all accepted lamps together, which is the most compatible option, or use cockpit zones so left-side zones start on Colour 1 while right-side zones start on Colour 2. Engine start plays a short Warm afterglow, Hard flash or Ignition burst animation when SimHub reports the engine starting.

### Pedals

Pedals can show throttle and brake as static colours or as brightness scaled from pedal travel. Brake has priority over throttle. Use the throttle and brake enable switches when a lamp should react to only one pedal.

### Wheel lock and wheel spin

Wheel lock uses ABS active when the sim reports it, then calculated wheel lock while braking as a fallback. Wheel spin uses TC active when available, then calculated wheel spin while accelerating. Each effect stays active only while the signal is active.

### Custom 1 and Custom 2

Custom 1 and Custom 2 are user-defined alert slots. Each slot has its own optional display name, formula, mode, colours, blink speed and duration limit. The display name is shown in the Effects tab header and alert priority list as a hint, while the Lights setup tab keeps the compact Custom 1 / Custom 2 labels.

The Edit button opens SimHub's native JavaScript/NCalc formula editor and the effect is active when the formula returns a truthy value. This is useful for simulator-specific properties such as iRacing off-track or incident logic, without adding a separate fixed effect for every possible game property.

Each custom slot supports Blink, Fixed and Fade modes. Blink uses primary and secondary colours; Fixed and Fade use the primary colour. If Limit duration is off, a truthy formula keeps the custom effect active while it stays true. If Limit duration is on, a rising true result starts the effect for the selected number of seconds and it can trigger again after the condition clears.

### Preview

Preview buttons play the selected effect on endpoints that accept that effect. Alert brightness preview uses all endpoints with at least one alert effect because it is the shared brightness scale for the Effects tab.

## Dark mode

Dark mode can follow this plugin's local toggle, Lovely Plugin true dark mode, or Daniel Newman Racing true dark mode. The Dark mode page source selector chooses which state and colour are used. There is no separate enable switch because external dark-mode effects can still run through SimHub even when this plugin is not the source.

The Dark mode control and Stream Deck action toggle immediately when Plugin local colour is selected, or when an unavailable or disabled external source has fallen back to the plugin's local state. While an enabled Lovely Plugin or Daniel Newman Racing source owns the state, the control remains a live indicator and cannot override that external plugin.

When dark mode is active, the lights stay on the selected dark mode colour until the cockpit brightness passes that game's Dark-mode cockpit blend start from Light response. The plugin still uses the selected dark mode colour as the base and layers cockpit light over it when the car passes brighter zones, so the base colour does not hide modest brightness changes.

The Lights setup tab controls which endpoints accept dark mode. This can be enabled on selected cockpit lights or on unticked SimHub Ambient Lights endpoints, so only the lamps you choose receive dark-mode output.

### Base colour brightness

This scales only the steady dark-mode base colour used by this plugin. Cockpit highlights, whites and brighter areas are still added by the capture blend, then each device max brightness limit is applied.

### Dark-mode cockpit blend start

This Light response value is based on the adaptive cockpit light output before each device applies its own max brightness limit, not raw mask percentage. Lower values let real cockpit light start layering over the dark mode colour sooner for that game. The layer ramps in over a short output range instead of waiting for 100% brightness.

### Dark cutoff

Turns very low cockpit output fully off for the current game. Output just above the cutoff fades back in through a short soft ramp, avoiding a visible jump from black. Idle, VR, alerts and the steady dark-mode base colour are not affected.

The Dark mode page shows the colour currently detected from the selected source for quick diagnosis.

## Controller integrations

Open the Controllers tab to install or update Bruno Silva SimHub Control Bridge and the Cockpit Auto Ambient Light integration for Elgato Stream Deck, Stream Dock / MiraBox or Ulanzi Studio devices. The bridge is a separate headless SimHub plugin shared with other Bruno Silva products; it has no settings page of its own.

After installation, restart SimHub once and add the Cockpit Auto Ambient Light actions from your controller application. All button artwork is included. Plugin power, Dark mode, Streaming mode and Idle hold show their real current state. Select idle theme lists the saved themes; its button gets a green border and check when that specific theme is active. Theme buttons use a neutral centre because a theme can contain several colours and animations; the theme name is the identity shown on the key. Current idle theme always follows and displays the active theme. On a supported model with knobs, place Current idle theme on a knob and rotate it for previous / next. Changes made in this UI, with Next/Previous or from another controller are reflected on every visible controller button. Theme actions that show text or numbers have their own size slider in Stream Deck, Stream Dock and Ulanzi, stored independently for each key or encoder. New controls start at their largest supported size and can be reduced for longer names.

Idle brightness down and Idle brightness up adjust the idle master by 5% on normal keys. Driving brightness down and Driving brightness up adjust the one global driving master shared by normal and Streaming output. On a supported model with knobs, use the matching brightness dial: rotate for 5% steps, or hold the knob while rotating for 1% fine adjustment. Elgato Stream Deck + also shows the real percentage and progress.

Experimental - Stream Dock integration: its normal keys and knob actions are implemented against the official Stream Dock SDK, but have only been tested in the desktop application. The entire integration still requires validation on real Stream Dock or MiraBox hardware. Knob actions additionally require a real knob-equipped model because the application cannot emulate one.

Experimental - Ulanzi integration: buttons, live states, per-key text sizing and D200X encoder actions are implemented against the official Ulanzi SDK and have only been validated with its simulator. Support for physical D200, D200H, Dial and D200X hardware still requires user validation. Ulanzi Studio does not load controller plugins without compatible hardware, so Disconnected is expected when no physical device is attached and does not indicate an installation failure. Encoder rotation changes themes, idle brightness or driving brightness; held rotation uses the same 1% fine brightness adjustment as Stream Deck +.

The Ulanzi adapter writes a low-volume diagnostic log containing startup, Ulanzi Studio and bridge connections, added actions, user commands and errors. It deliberately omits routine image and live-state refreshes. When physical testing fails, send %USERPROFILE%\Documents\CockpitAmbientLight\Logs\ulanzi-controller.log.

Reset car mask and Reset plugin output are momentary actions: a successful command shows a short confirmation rather than remaining lit. If SimHub, the bridge or this plugin is unavailable, the button shows an offline state and reconnects automatically when the provider returns.

Elgato Stream Deck uses its official package installer and may show its own confirmation. Stream Dock and Ulanzi integrations are installed directly after your confirmation in Cockpit Auto Ambient Light. Later controller-only updates are automatic where the controller platform permits it. You never need to download or assign icon files manually; each controller application provides its own runtime.

When supported controller software is installed but these controls are missing, Cockpit Auto Ambient Light offers their first installation during SimHub startup. Once the components are installed, the plugin checks them automatically when SimHub starts. The bridge and each controller integration are versioned independently. SimHub restarts only when the shared bridge DLL itself needs an update, and a confirmation window is always shown before that bridge update is installed.

## Troubleshooting

### Start with latest SimHub

Update SimHub first, especially for Ambient Lights, Govee, Bluetooth, capture or effects routing problems. This plugin reuses SimHub's internal Ambient Lights capture and effects pipeline, so older SimHub builds can cause behaviour this plugin cannot reliably fix.

### Lights do not react

Check that the lights are ticked in the Lights setup tab and also enabled in SimHub's Ambient Lights plugin. Also check that the output type switch for Hue, Govee, Bluetooth or Adalight is active in the Lights setup tab. Use Identify to confirm the bridge/container path is working.

### Direct Hue V2 sync stays Starting

A Hue bridge can run only one Entertainment sync owner for the same area at a time. If Direct V2 stays on Starting, first make sure the matching SimHub native Hue group is not manually enabled while Direct V2 owns it. Then click Resync once. The plugin stops stale V2 sessions, disables the matching native group before starting Direct V2, and ignores any old connection attempt that finishes after the backend has been disabled or changed. If the bridge was replaced or migrated to Bridge Pro, use Forget Direct V2 bridge and pair again so the saved bridge ID, IP and Entertainment Area match the new bridge.

### Switching Hue back to SimHub native V1

Turn Direct V2 off on Hardware or disable Hue for the profile on the Lights setup tab to release the Hue Entertainment sync. If this plugin disabled the matching SimHub native Hue group, it restores that group after the V2 sync has been stopped. Use Forget Direct V2 bridge only when you want this plugin to remove its saved Hue bridge pairing and return to the native V1 flow.

### No Hue V2 lights after pairing

Pairing only authorizes the bridge. Direct V2 also needs a Hue Entertainment Area. Use Pair again to select the correct Entertainment Area, then Refresh or Resync. Direct V2 builds its endpoint rows from the Entertainment channels reported by that area. Gradient strips can expose different channel counts by model and area layout, and after Resync existing Direct V2 selections or theme endpoint assignments may need to be set again.

### WLED does not react or looks wrong

Check that WLED is enabled for the profile on the Lights setup tab and the device shows Reachable. Set that device's brightness to 100% in its WLED web page, or a lower value dims everything. If Scan finds nothing, add the device by IP; some networks block mDNS. Colour is gamma-corrected automatically to match the WLED colour picker, so do not change the WLED web page gamma options. If red and green look swapped, fix the Color Order in the WLED web page. Reserve a static IP per device so it does not change.

### Themes tab shows no endpoints

Themes list available endpoints for enabled output types. If a group is empty, check that the output type is enabled for the profile on the Lights setup tab and that SimHub or Direct Hue V2 can currently see those endpoints. Direct Hue V2 requires a paired bridge and selected Entertainment Area. Adalight requires a configured, enabled and connected physical SimHub Adalight output.

### Theme preview does not change lights

Theme editing previews through this plugin's fixed-output layer. Make sure the plugin is enabled, the output type is enabled, and no other active Hue Entertainment session or SimHub effect is taking priority on the same lights. Save or Cancel exits preview and returns the lights to normal output.

### Other effects override the cockpit light

Temporary alert effects are expected to override this plugin. The plugin's own layer sits on top of the other containers in the effects profile, so a constant ambient-light effect from another plugin or profile should not hide the cockpit light. If one still does, check that the lamp has a unique Effects index in SimHub Ambient Lights.

### A light I control myself is overwritten by the plugin

The plugin writes every endpoint it finds in the SimHub effects profile, including lights not ticked for cockpit output. If you drive one of those lights yourself, for example from a Stream Deck script or a computed property, tick it on the Excluded lights tab. The plugin then sends it a fully transparent colour and your own effect comes through.

### Lights stay faintly on in dark scenes

Use Dark cutoff on the Light response page for that game. Start low, such as 3-6%. This only affects live cockpit output, so idle themes, VR themes, alert effects and the dark-mode base colour keep their own brightness controls.

### Effects-only lights do not show alerts

Effects-only lamps need at least one alert effect ticked on the Lights setup tab, and must not be on the Excluded lights tab. They do not fire during Hold idle theme, pause or idle fallback; those states intentionally keep alerts cleared.

### SimHub scene settings restore

When the plugin is enabled, it saves the current SimHub Ambient Lights scene brightness, effect profile brightness and luminosity gamma settings before applying its own 100% brightness and neutral gamma defaults. When the plugin is disabled, those saved settings are restored. If you change those SimHub settings while this plugin is active, the plugin keeps the new values instead of overwriting them on disable, and saves them as the new restore point.

### Mask never becomes active

Drive above walking speed in cockpit camera. The detector only learns while the car is moving so it can separate stable cockpit surfaces from the moving outside world. Disable heavy cockpit camera movement and move large overlays away from the cockpit area when possible.

### SimHub capture freezes in fullscreen

This plugin uses SimHub's native Ambient Lights screen capture. If that capture freezes when a game enters the track in fullscreen mode, check the game executable Compatibility settings and leave Disable fullscreen optimizations off. Borderless/windowed mode is the usual fallback when a game blocks native screen capture.

### Wrong colours in triples

Surface colour protection is the optional cool/neutral/warm-white filter shared by All, cockpit zones and Track ambience. For captured colours, turn protection off and set Colour amount to 100%. Set Colour warmth to 0% to avoid adding a warm bias, or raise it for a warmer result. Night-time bright-point protection still controls brightness without attenuating coloured samples. Lamp brightness ranges preserve channel ratios rather than whitening the signal.

### Govee debug

Enable the debug log on the Diagnostics tab only when troubleshooting. The plugin writes one startup-cleared log file focused on SimuLight bridge and Govee discovery details.

### Factory reset plugin settings

Use Factory reset on the Maintenance page only as a last resort for corrupted or legacy plugin configuration. It resets this plugin's settings and backups, but keeps SimHub Ambient Lights devices, profiles and Effects indexes untouched.

### Diagnostics zones

The Diagnostics brightness table shows Overall plus direct sampled zones: Left outer, Left inner, Center-left, Center-right, Right inner and Right outer. Left, Center and Right are still valid lamp selections, but they are combined outputs rather than direct diagnostic zones. Lower cockpit is shown separately with its left/center/right subzones. Track ambience is shown as one independent zone with a plain-language learning status, raw mask brightness, fixed-curve light output, accepted mask blocks, coverage across its six internal regions and current gain. Detailed per-region learning evidence remains in the diagnostic CSV for troubleshooting rather than being shown in the normal UI.

### Need help?

For setup questions, problems or bug reports, email bruno.1978@gmail.com, open an issue on the GitHub repository, or send a Discord message to Bruno Silva (brunosilva1978).
