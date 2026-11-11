# UI inventory

This is the Phase 0 discovery skeleton. Every geometry, font, color, duration, sound, and input detail is **UNKNOWN** until install extraction or capture measurement fills it. The official PC manual [S09] verifies the named HUD components and default PC controls; it does not verify pixel geometry.

## Per-screen record template

- ID / purpose / entry / exits
- Evidence and capture filenames
- Elements: x, y, width, height at 1280x720 and 1920x1080; anchor/safe-area behavior at 4:3 and 21:9
- Typography: install font asset, size, weight, color, outline, shadow
- Motion: duration in frames/ms, easing, delay, focus pulse, transition order
- States: default, focused, pressed, disabled, loading, error
- Audio cue and trigger frame
- Keyboard, mouse, XInput mapping and focus traversal
- Diff mask and acceptance tolerance (initially **UNKNOWN**)

## Screen flow skeleton

`boot/legal -> intro/attract -> profile/storage -> main`

From `main`: `single player -> career/group -> event -> car/loadout -> loading -> race HUD -> pause/results`; `multiplayer -> playlist/custom/lobby -> car/mods -> loading -> race/results`; `friend challenges`; `stickers/extras`; `options`; `credits`.

This graph is **HYPOTHESIS** assembled from manuals/screenshots and exists to drive capture coverage. Captures must correct it before UI code.

## Inventory

| ID | Screen/state | Source-backed existence | Capture status |
| --- | --- | --- | --- |
| boot.publisher | Activision splash | **VERIFIED (install):** `UI/movies/atvi.bik` | missing |
| boot.developer | Bizarre splash | **VERIFIED (install):** `UI/movies/BizarreSplash.bik` | missing |
| boot.legal-profile | legal, profile, storage, autosave prompts | **UNKNOWN** details | missing |
| boot.attract | attract/intro loop | **VERIFIED (install):** `BlurAttract.bik` | missing |
| main.root | root navigation and focus variants | **SOURCED [S13]** Windows screenshot | missing |
| sp.root | single-player destinations | **SOURCED [S09]** | missing |
| career.map | nine-group map, locks/completion | **SOURCED [S09][S10]** | missing |
| career.event-grid | seven events and boss demands | **SOURCED [S10][S14]** | missing |
| career.event-detail | race/checkpoint/destruction/one-on-one setup | **SOURCED [S10]** | missing |
| garage.showroom | car select/details/upgrades | **SOURCED [S09]** multiplayer showroom named | missing |
| garage.mods | three-slot mod/loadout selection | **SOURCED [S09]** | missing |
| mp.mod-shop | Mod Shop screen, available/locked mods, selection and return flow | **SOURCED [S09]** existence; detailed behavior **UNKNOWN** | missing |
| race.loading | event loading/tutorial hints | **UNKNOWN** variants | missing |
| hud.race | position, health, minimap, lap, fans/combo, scoring, warning, three power-up slots, sticker, lights, next target | **SOURCED [S09]** | missing |
| hud.special | countdown, Fan Demand/Run, wrong way, wreck, boss, checkpoint/destruction | partly **SOURCED [S09]**, details **UNKNOWN** | missing |
| pause.root | pause navigation and confirmation | **UNKNOWN** details | missing |
| results.event | placement, lights, fans, unlocks | **SOURCED [S09]** systems; layout **UNKNOWN** | missing |
| results.boss | boss defeat/reward | **SOURCED [S09]** | missing |
| mp.root | online/local/split-screen destinations | **SOURCED [S09][S12]** | missing |
| mp.playlists | Power-up Racing, Hardcore, Motor Mash, World Tour, Team Racing | **SOURCED [S12]** and secondary manual mirror | missing |
| mp.custom | rules, track/car/power-up/lap choices | **SOURCED [S12]** overview | missing |
| mp.lobby | players, teams, ready, matchmaking/search/errors | **UNKNOWN** details | missing |
| mp.progression | rank, challenges, upgrades, Mod Shop, Legend | **SOURCED [S09]** | missing |
| options.controls | keyboard/controller mapping | **SOURCED [S09]** defaults | missing |
| options.video | resolution/display/graphics | **UNKNOWN** complete list | missing |
| options.audio | mix settings | **UNKNOWN** complete list | missing |
| social.friend-challenge | create/receive/rechallenge | **SOURCED [S09]** | missing |
| extras.stickers | sticker tracker/gallery | **SOURCED [S09]** | missing |
| dialogs.all | confirmation, loading, controller, save, network, share, errors | **UNKNOWN** complete set | missing |

## Verified default PC controls

**SOURCED [S09]:** menu arrows navigate, Space confirms, Escape backs out; race bindings include Q accelerate, A brake/reverse, arrows for steering/handbrake/power-up toggle, Ctrl/Shift power-up actions, P pause, V camera, Delete minimap zoom, and Tab look-back. Exact remapping behavior and displayed glyphs remain **UNKNOWN**.

