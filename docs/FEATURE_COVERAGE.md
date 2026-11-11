# Feature coverage checklist

This is a detailed working checklist for the rebuild. It includes major systems and small behaviors that are easy to overlook. It is **not proof that every feature in the original has been discovered**. Add an item whenever original-game captures, the manual, installed assets, traces, or play reveal another screen, state, rule, option, reward, or edge case.

## Status key

- `UNKNOWN`: original behavior or existence has not been verified.
- `KNOWN / NOT BUILT`: the item is supported by a source, but no implementation is recorded.
- `PARTIAL`: some project code or diagnostic exists; original parity is unverified.
- `VERIFIED`: implemented and compared with original evidence using the check named in the row.
- `N/A`: evidence confirms the feature does not apply to this edition or mode.

Most rows are currently `UNKNOWN` or `KNOWN / NOT BUILT`. A format parser, asset preview, menu mockup, or synthetic test alone does not make a game feature `VERIFIED`.

## Maps, routes, and presentation

| ID | Feature and small cases to track | Status | Evidence / detail | Completion check |
| --- | --- | --- | --- | --- |
| map.catalog | Every installed map; race vs showroom; map names; game modes using each map | PARTIAL: 19 folders load structurally; 18 race-labelled directories plus showroom, actual selectable content not fully mapped | [MAP_COVERAGE.md](MAP_COVERAGE.md), [ASSET_INVENTORY.md](ASSET_INVENTORY.md) | Compare a source-backed list of selectable maps to installed and rebuilt catalog. |
| map.variations | Every route/layout variation inside every folder; forward/reverse; short/long routes; alternate branches; event-specific route changes | UNKNOWN: total variation count and mapping are unverified | [MAP_COVERAGE.md](MAP_COVERAGE.md) | Give each observed variation a stable ID and independently verify a full lap. |
| map.geometry | Road, shoulder, sidewalk, barriers, walls, buildings, props, bridges, tunnels, start line, finish line, checkpoint gates | PARTIAL: candidate geometry and regional textured diagnostic renders exist | [MAP_COVERAGE.md](MAP_COVERAGE.md), [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Compare full route coverage against original captures for each variation. |
| map.positioning | Coordinate basis, units/scale, transforms, origin, streamed chunks, visibility/culling and draw order | PARTIAL / UNKNOWN by item | [FORMATS.md](FORMATS.md), [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Overlay source-derived geometry and captured views at multiple route positions. |
| map.course_rules | Start positions, grid order, direction, checkpoints, checkpoint order, lap line, route split/merge, wrong-way detection | UNKNOWN for installed courses | [MAP_COVERAGE.md](MAP_COVERAGE.md), [RACE_LOOP.md](RACE_LOOP.md) | Drive each variation from start through ordered gates to finish and compare with original. |
| map.collision | Drivable surfaces, walls, barriers, height steps, tunnels, vehicle contact and respawn bounds | PARTIAL: candidate collision diagnostic and synthetic resolution exist; original course/contact behavior unknown | [MAP_COVERAGE.md](MAP_COVERAGE.md), [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Compare repeated paths, impacts and recovery against recorded gameplay. |
| map.materials | Correct texture per draw, UV sets, filtering, address modes, mip selection, alpha and material flags | PARTIAL: selected named/stream textures and one road material path | [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Verify representative materials and then coverage across every map/variation. |
| map.lighting | Lightmaps, ambient/dynamic lights, day/night or other lighting variants if present | PARTIAL: one road lightmap path investigated; game-wide behavior unknown | [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Compare matching captures with color and lighting checks. |
| map.reflections | Road reflections, environment cube faces, screen-space filtering, reflection inputs | PARTIAL: one captured road reflection path traced | [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Reproduce the captured pass chain and compare its output. |
| map.effects | Particles, sparks, smoke, dust, water, weather, animation, destruction and post-processing where present | UNKNOWN by effect | [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Inventory original effects by map/event and compare timed captures. |
| map.streaming | Chunk loading/unloading, level-of-detail transitions, pop-in, visibility and memory limits | UNKNOWN | [FORMATS.md](FORMATS.md), [PHASE2_VIEWER.md](PHASE2_VIEWER.md) | Record long runs through each route and verify stable visual coverage and memory use. |

## Cars, driving, and race rules

| ID | Feature and small cases to track | Status | Evidence / detail | Completion check |
| --- | --- | --- | --- | --- |
| car.roster | Every car, class, stats, unlock, selectable state and showroom model | UNKNOWN / PARTIAL asset leads | [ASSET_INVENTORY.md](ASSET_INVENTORY.md), [UI_INVENTORY.md](UI_INVENTORY.md) | Reconcile installed roster with all selectable original cars. |
| car.appearance | Paint, liveries, decals/stickers, damage visuals, lights and wheel/body variants | UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md) | Compare each selectable appearance and saved choice. |
| car.handling | Acceleration, top speed, braking, reverse, steering, grip, drift, handbrake, traction, suspension, airborne control | PARTIAL: project-side vehicle simulation only | [HANDLING.md](HANDLING.md) | Trace-diff matched input sequences and car states. |
| car.physics_edges | Collision response, wall slide, corner stop, jumps, landing, slopes, height steps, tunneling, stuck detection | PARTIAL synthetic collision behavior; original values unknown | [HANDLING.md](HANDLING.md), [MAP_COVERAGE.md](MAP_COVERAGE.md) | Compare controlled test cases against original captures. |
| car.damage | Health, damage levels, critical state, wreck, repair, invulnerability/shield interactions | UNKNOWN | [POWERUPS.md](POWERUPS.md), [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Exercise each damage transition and recovery in original and rebuild. |
| car.camera | Chase camera, look-back, camera toggle, collision/obstruction, smoothing, FOV and reset | PARTIAL free camera only | [PHASE2_VIEWER.md](PHASE2_VIEWER.md), [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Compare camera traces for turns, impacts, jumps and camera-mode changes. |
| race.start | Grid placement, countdown, false start if any, input lockout, launch and initial camera | UNKNOWN | [RACE_LOOP.md](RACE_LOOP.md), [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Capture and reproduce the full start sequence for each applicable mode. |
| race.progress | Checkpoint crossing, order, lap count, lap timing, finish crossing and race completion | PARTIAL: deterministic project tracker and synthetic tests | [RACE_LOOP.md](RACE_LOOP.md) | Compare complete recorded races, including missed/out-of-order gates. |
| race.events | Race, Checkpoint, Destruction, One-on-One/Boss and every other installed event type | PARTIAL / UNKNOWN by type | [RACE_LOOP.md](RACE_LOOP.md), [UI_INVENTORY.md](UI_INVENTORY.md) | Each event can start, follow its own rules, finish and produce matching results. |
| race.scoring | Position, time, points, event objectives, ties, penalties and placement rules | UNKNOWN | [RACE_LOOP.md](RACE_LOOP.md) | Recompute outcomes from captured event scenarios. |
| race.fans | Fan actions, combo, Fan Target, Fan Run, awards, lights, fan totals and unlock contribution | UNKNOWN | [RACE_LOOP.md](RACE_LOOP.md), [UI_INVENTORY.md](UI_INVENTORY.md) | Match awards and thresholds for captured driving sequences. |
| race.pickups | Spawn locations, pickup timing, respawn, replacement rules, slot states and item indicators | UNKNOWN | [POWERUPS.md](POWERUPS.md) | Compare pickup and respawn sequences in recorded races. |
| race.pause | Pause timing, resume, restart, quit, confirmation and race-state restoration | UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md) | Verify every pause action and its state transition. |
| race.results | Placement, times, fans, lights, rewards, unlocks, replay, retry and continue | UNKNOWN / partial UI sources | [UI_INVENTORY.md](UI_INVENTORY.md) | Compare result calculations and every available action. |

## Power-ups (track each independently)

All eight entries below are named in the sourced manual; detailed numbers and original behavior still need capture/verification. Use [POWERUPS.md](POWERUPS.md) as the per-power-up specification.

| ID | Feature and edge cases | Status | Completion check |
| --- | --- | --- | --- |
| powerup.shock | Three domes, placement, duration, target slow, repeated hits, owner interaction | KNOWN / NOT BUILT | Verify placement, timing, radius, speed change, counters, effects/audio. |
| powerup.shunt | Homing, target changes, backward alternate fire, hit/flip, misses, counterplay | KNOWN / NOT BUILT | Compare trajectory, target selection, impact and response. |
| powerup.barge | Close-range activation, radial range/falloff, multiple cars, walls, cancellation | KNOWN / NOT BUILT | Compare affected targets and impulses at controlled distances. |
| powerup.mine | Rear drop, forward throw, arming, trigger, lifetime, multiple targets | KNOWN / NOT BUILT | Compare spawn/trajectory, arming and impact outcomes. |
| powerup.bolt | Three shots, spread, backward firing, cadence, target hit and misses | KNOWN / NOT BUILT | Compare shot timing, aim, hit detection and response. |
| powerup.nitro | Forward speed burst, reverse/alternate airbrake, boost, steering and traction | KNOWN / NOT BUILT | Compare speed and motion traces through full activation. |
| powerup.shield | Duration, impact prevention, damage prevention, stacking and expiry | KNOWN / NOT BUILT | Exercise each attack and collision while active and at expiry. |
| powerup.repair | Repair amount, activation time, critical/wrecked states and repeated use | KNOWN / NOT BUILT | Compare health transitions and invalid-state behavior. |
| powerup.shared | Slot assignment, pickup replacement, activation input, cooldown, HUD, sound, effects and scoring | UNKNOWN | Verify common behavior and interactions for every power-up. |

## AI and opponent behavior

| ID | Feature and small cases to track | Status | Completion check |
| --- | --- | --- | --- |
| ai.route | Route choice, branches, checkpoint awareness, wrong-way recovery, respawn | UNKNOWN | Compare opponent paths across each track variation. |
| ai.driving | Speed, braking, cornering, overtaking, collision avoidance and recovery | UNKNOWN | Measure trajectories and finish positions against captures. |
| ai.combat | Power-up pickup/use, target selection, defense, timing and friendly fire | UNKNOWN | Compare power-up decisions and outcomes. |
| ai.difficulty | Every difficulty's speed, aggression, error rate and rubber-banding if present | UNKNOWN | Run repeatable races and compare distributions. |
| ai.grid | Opponent roster, positions, count, team assignment and event-specific lineup | UNKNOWN | Match event setup and race results. |

## Garage, Mod Shop, career, and progression

| ID | Feature and small cases to track | Status | Evidence / detail | Completion check |
| --- | --- | --- | --- | --- |
| garage.showroom | Entry/exit, car browsing, class filters, locked cars, stats, preview, selection | KNOWN / NOT BUILT | [UI_INVENTORY.md](UI_INVENTORY.md) | Walk every state and verify selected car in race. |
| garage.upgrades | Per-car fan, health, performance, paint rewards/upgrades and effects | KNOWN / NOT BUILT | [UI_INVENTORY.md](UI_INVENTORY.md), [CONTEXT.md](../CONTEXT.md) | Compare unlock conditions, stat changes and persistence. |
| garage.modshop | Mod Shop access, browse, locked/available items, select/equip, three slots, replace/remove, persistence | KNOWN / NOT BUILT; detailed behavior UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md), [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Capture every action and verify chosen loadout affects the race. |
| career.structure | Groups, locations, event grid, completion, boss demands, unlock gates | SOURCED at summary level; complete graph UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md), [CONTEXT.md](../CONTEXT.md) | Complete a fresh profile through every event gate. |
| career.bosses | Boss events, demands, one-on-one rules, defeat state and rewards | SOURCED at summary level; behavior UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md) | Verify qualifying demands, win/loss and rewards. |
| progression.fans | Fan awards, fan status, car unlock thresholds and rank contribution | SOURCED at summary level; exact thresholds UNKNOWN | [RACE_LOOP.md](RACE_LOOP.md) | Match totals and unlocks over repeated events. |
| progression.rewards | Lights, cars, mods, liveries, stickers, upgrades, XP/rank/Legend rewards | SOURCED partly; full reward graph UNKNOWN | [UI_INVENTORY.md](UI_INVENTORY.md) | Reconcile every observed reward and unlock condition. |
| progression.saves | Profile creation, autosave, load, corruption/error, overwrite, persistence and reset | UNKNOWN | [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) | Verify persistence and recovery across relaunches and failures. |

## Menus, HUD, and interaction details

Use [UI_INVENTORY.md](UI_INVENTORY.md) as the screen-level index and [CAPTURE_CHECKLIST.md](CAPTURE_CHECKLIST.md) for capture runs. For **every screen**, separately track: entry source; every exit/back path; each focusable item; default/focused/pressed/disabled/loading/error states; action result; transition timing; sound; keyboard/controller input; screen aspect ratios; persistence; and cancel/confirmation behavior.

| ID | Screen group / small states | Status | Completion check |
| --- | --- | --- | --- |
| ui.boot | Publisher/developer logos, legal, profile/storage, autosave, intro/attract, press start/continue | PARTIAL assets identified; flow UNKNOWN | Capture and reproduce each start-up path and prompt. |
| ui.main | Root navigation, focus order, transitions, quit/return behavior | UNKNOWN | Compare all focus states and paths. |
| ui.singleplayer | Career entry, map, event group, event grid, event details, boss demand | UNKNOWN / sourced destinations | Walk a fresh profile and compare all available/locked states. |
| ui.multiplayer | Local/online entry, playlist, custom rules, lobby, teams, ready, matchmaking, failures | UNKNOWN / sourced mode names | Exercise successful and failed lobby flows. |
| ui.garage | Showroom, upgrades, Mod Shop, car select, mod/loadout, return path | UNKNOWN | Verify every selection, cancel, lock, and persistence state. |
| ui.loading | Map/event loading, hints, boss/tutorial variants, failure/timeout | UNKNOWN | Capture each observed variant and transition. |
| ui.hud | Countdown; position; lap; timer; speed; health; minimap; fans/combo; lights; power-up slots; target/wrong-way; damage | SOURCED components; layout/behavior UNKNOWN | Pixel compare representative states and timed transitions. |
| ui.hud.special | Fan Demand/Run, boss, checkpoint/destruction objectives, wreck, critical damage, power-up warnings | SOURCED partly; details UNKNOWN | Capture each trigger, update and dismissal. |
| ui.pause | Resume, restart, options, quit, confirmations, state restoration | UNKNOWN | Verify actions and resumed race state. |
| ui.results | Event/boss results, placement, fans/lights, XP/rank, unlocks, replay/retry/continue | SOURCED partly; layout/logic UNKNOWN | Verify calculations and every action. |
| ui.options | Controls, remap, video, audio, gameplay, defaults, apply/revert | UNKNOWN / default controls sourced | Change, save, cancel and restore each option. |
| ui.dialogs | Save/load, overwrite, disconnect, controller, invite/share, network, error/confirmation | UNKNOWN complete set | Enumerate reachable dialogs and their actions. |
| ui.extras | Challenges, friend challenges, stickers/gallery, credits, sharing | SOURCED destinations partly; details UNKNOWN | Verify every original destination and state. |
| ui.input | Keyboard, mouse, controller, focus movement, repeat, confirm/back, pause, remapping | Default PC mappings SOURCED; edge cases UNKNOWN | Compare device-by-device, including simultaneous input and focus wrap. |
| ui.accessibility | Subtitles, color/readability options, safe areas or other accessibility settings if present | UNKNOWN | Check original settings and reproduce supported options. |

## Multiplayer, audio, and platform details

| ID | Feature and small cases to track | Status | Completion check |
| --- | --- | --- | --- |
| mp.modes | Race, Checkpoint, Destruction, Power-up Racing, Motor Mash, Hardcore, World Tour, Team Racing, custom games, challenges | SOURCED names; full rules UNKNOWN | Verify playlist availability, rules and scoring per mode. |
| mp.local | Split-screen players, controller assignment, shared screen/camera, joins/leaves | SOURCED existence; details UNKNOWN | Run supported local player counts and edge cases. |
| mp.online | Peer/session setup, invite, search, lobby, ready, disconnect, host exit, reconnect | SOURCED broad architecture; gameplay paths UNKNOWN | Test session lifecycle and failure paths against captures. |
| mp.rank | Fans, rank, XP, unlocks, three mod slots, Legend reset/levels | SOURCED summary; exact thresholds UNKNOWN | Match progression and persistence across sessions. |
| audio.music | Menu/race music, transitions, pause, results and event variants | UNKNOWN | Compare cue timing, selection and mix. |
| audio.vehicle | Engine/revs, tire, collision, damage, boost, wreck and repair sounds | UNKNOWN | Compare audio against controlled action captures. |
| audio.gameplay | Each power-up, pickup, warning, fan award, countdown, checkpoint and result cue | UNKNOWN | Verify cue identity, timing, overlap and attenuation. |
| audio.ui | Focus, confirm, back, locked, error, save and navigation sounds | UNKNOWN | Walk every menu action and compare cue timing. |
| audio.mix | Volume sliders, ducking, spatialization, effects levels and mute behavior | UNKNOWN | Measure representative mix states. |
| platform.video | Resolution, window/fullscreen, aspect ratio, refresh/vsync, graphics settings | UNKNOWN full option list | Verify supported modes and persistence. |
| platform.performance | Frame pacing, loading, memory, long races and supported hardware | UNKNOWN | Meet documented frame-time and stability targets. |
| platform.compat | Install paths, missing/corrupt files, permissions and asset cache behavior | PARTIAL bounded loader checks | Exercise clean install and failure cases. |

## Asset and tool behavior

| ID | Feature and small cases to track | Status | Completion check |
| --- | --- | --- | --- |
| assets.pak | Package listing, extraction, compression/encryption, malformed entries | PARTIAL by format | Round-trip or inspect every supported shape; document unsupported data. |
| assets.ipf | Index/resource table, strings, icons, malformed/outlier entries | PARTIAL by format | Validate all observed entry types against source assets. |
| assets.baf-bin | Audio/config/data payload formats and relationships | PARTIAL / UNKNOWN by format | Identify and parse every required runtime asset type. |
| assets.model | Geometry, skeleton, materials, LOD and animation references | PARTIAL by format | Render representative assets and validate references. |
| assets.texture | DXT formats, mips, alpha, color space, texture tiers and ownership | PARTIAL | Compare decoded pixels to captured uploads and reference output. |
| assets.level | `.level`, `.levelphysics`, `.levelrender`, `.levelstream` companions and stream relationships | PARTIAL | Validate all observed structures and preserve unknown fields safely. |
| tools.viewer | Camera control, selection, screenshots and diagnostic modes | PARTIAL | Regression-check navigation and deterministic captures. |
| tools.import-export | Any supported map/model/audio import/export workflow | UNKNOWN requirements | Define only from original asset workflows and user-facing project scope. |

## Rules for adding and closing checklist items

1. Keep **one row per independently observable behavior or state** when it can fail separately. For example, Mod Shop browsing, equipping, replacing a slot, persistence, and race effect are separate checks.
2. Add variation rows in [MAP_COVERAGE.md](MAP_COVERAGE.md) for each original playable route/layout. Folder count and physics-section count are not variation counts.
3. Link every asserted original behavior to evidence in a detail document. Tag claims there `VERIFIED`, `SOURCED`, `HYPOTHESIS`, or `UNKNOWN`.
4. Split rows when a subsystem has multiple independently testable variants. Do not mark a whole category verified because one example works.
5. Before a release is called feature-complete, revisit every original menu path, event type, map variation, vehicle, power-up, setting, reward, error path and multiplayer mode. Any unverified item must be listed as incomplete.

This ledger is exhaustive relative to the current project prompt, manuals and inventories, but unknown hidden behavior can still be discovered. The final original-game walkthrough is what finds omissions beyond this list.
