# Ground-truth capture checklist

Do not commit captures. Put them under `reference/captures/` using the naming rule below. Record game version, executable SHA-256, OS, GPU/driver, input device, graphics settings, and whether patch 1.2 is installed in `reference/captures/session.json`.

## Capture standard

- Native 60 fps, lossless or visually lossless, no overlays, post-processing injectors, scaling, or frame interpolation.
- Capture 1280x720 and 1920x1080; add 1024x768 (4:3) and 2560x1080 (21:9) for representative menu and HUD screens.
- Hold every selectable UI item focused for at least two seconds. For transitions, start two seconds before input and end two seconds after settling.
- Still name: `<resolution>/<screen-id>__<state>__f<frame>.png`.
- Sequence name: `<resolution>/<screen-id>__<transition>__60fps.mkv`.
- Do not crop. Preserve whole-frame pixels and audio.

## UI captures

- Boot: publisher, developer, legal/profile/storage prompts, intro/attract, press-start/continue states.
- **VERIFIED (user report, 2026-10-11):** leaving Blur without input switches to a sample/attract video; arrow-key behavior in that video is not a valid gameplay-control check. For Phase 2 driving-scene captures, navigate into and load the race before evaluating input or camera behavior.
- **VERIFIED (user report, 2026-10-11):** the user cannot interact with Blur through remote access. Continue offline using the existing Tokyo race capture and local assets; do not wait for remote game input. Original-game visual/input parity may need a later user-provided capture if the existing frame is insufficient.
- Main: every top-level item, signed-out/offline/errors, single-player, multiplayer, friend challenge, stickers/extras.
- Career: group map locked/unlocked/completed, event grid, race/checkpoint/destruction/one-on-one detail, boss demands, fan target/run state.
- Garage: showroom, car class/list/details, locked/unlocked, livery/color, mod selection, loadout.
- Loading: each event type and at least one boss/tutorial loading state.
- Race HUD: countdown, 20/10/3/1 positions where available, every lap state, each of eight power-ups in slots 1/2/3 and active, targeted warning, damage levels, critical/wreck/repair/shield, fans/combo 1–10, scoring message, Fan Demand, Fan Run, wrong way, pause.
- Results: win/podium/non-podium/wreck, lights, fans, unlock, boss defeat, XP/rank/Legend, replay/retry/continue.
- Multiplayer: online landing, playlists, World Tour, Power-up Racing, Hardcore Racing, Motor Mash, Team Racing, custom setup, lobby states, matchmaking/search/failure, showroom/upgrades, Mod Shop/challenges/rank.
- Mod Shop: capture its entry and exit paths, any tabs and focus states, locked and available mod states, the action taken when selecting a mod, the three-slot loadout, and whether the choice persists after leaving and re-entering. Record actual behavior before specifying purchase or unlock rules.
- Options: controls for keyboard/controller, video, audio, gameplay, confirmation/revert, reset defaults.
- Popups: save/load/network/controller/disconnect/invite/share/overwrite/error/confirmation variants.

## Animation measurement

When FFmpeg becomes available, extract exact frames with:

```text
ffmpeg -i input.mkv -vsync 0 reference/frames/<screen-id>/%06d.png
```

Measure first changed frame, last moving frame, delays, focus pulse period, easing samples, and audio cue frame. Record values in `UI_INVENTORY.md` with `VERIFIED (frame measurement)`.

## Image-diff shots

Start with: main menu default/focused; career map; event detail; showroom; load screen; clean race HUD; each power-up active; pause; results. Use exact fixed states and record allowed masked regions. Initial tolerance is **UNKNOWN** until capture noise and codec effects are measured; do not invent a percentage.

## Handling trace scenarios

Use one stock car and one flat, repeatable straight selected by the human after Phase 0 review. For every run, record inputs and per-update time, position, orientation, linear/angular velocity, steering, throttle, brake, handbrake, wheel contacts, current gear/RPM if accessible, damage, and surface.

1. Idle for 10 s, then full-throttle standing start for 20 s.
2. Hold steady speeds at several reproducible points; full brake to rest.
3. Steering sweep left-to-right at low, medium, and high speed.
4. Constant-radius steering at three speeds; repeat with handbrake drift entry.
5. Wall impacts at shallow/medium/perpendicular angles; sustained wall grind.
6. Car-to-car front/rear/side impacts with stationary and moving targets.
7. Straight jump with no input, throttle, brake, and steering in air.
8. Repeat every scenario three times from reset to test determinism/noise.

Trace acquisition order: inspect Amax plugin SDK/logging first; if insufficient, develop an external read-only memory logger from verified structures. Exact operator instructions cannot be honest until that tool exists.

