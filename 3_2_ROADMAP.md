# BE 3.2 Roadmap

Updated: 2026-11-24

Focus: reliable flight transitions, Odyle gathering, FP recovery and route playback.

## Status guide

- **Implemented in source:** code exists; this does not mean the public download contains it.
- **Local test build:** included in a locally verified package; gameplay acceptance is separate.
- **Pending implementation:** requirements are recorded, but the feature is not complete.
- **Pending live validation:** still requires an in-game reproduction or acceptance test.
- **Out of scope:** not planned for BE 3.2.

BE and BA2 are separate products. A correction in BA2 is not automatically present in BE.

## Implemented in the BE 3.1 source baseline

### Skip a resource that disappears during approach

- [x] Keep F9 running if the selected node disappears during approach or immediately before F.
- [x] Defer that node for 30 seconds and select another without counting a gathering attempt.
- [x] Retain flight cleanup and stop conditions for input failures or unavailable player state.
- [ ] Reproduce another player taking the node during approach in game.
- [ ] Include and verify the correction in the next distributed executable.

**Status:** source implementation and controller/syntax checks completed. Inclusion in the current published 3.1 archive is not confirmed; do not describe it as a shipped 3.2 release.

### Existing FP controls

- [x] Separate minimum takeoff, low-FP landing and Resume FP settings.
- [x] After low-FP or node-batch recovery, wait for Resume FP; batch recovery also respects the timer.
- [x] Keep these F9 recovery rules separate from recorded route actions.

**Status:** existing baseline behavior, not a new 3.2 feature. It does not yet cover the distinct no-target landing policy below.

## P0 — Pending implementation

### Record and replay mid-air V cancellation

The reported freeze has been investigated, but the current recorder still relies on confirmed flight-state transitions; observing V is not yet a separately replayable cancellation action.

- [ ] Record V cancellation separately from confirmed touchdown.
- [ ] Distinguish falling segments from powered flight; do not save them as ordinary Fly waypoints.
- [ ] Resume ground movement after confirmed touchdown without another unintended takeoff.
- [ ] Preserve normal descent when the player flies all the way down to the ground.
- [ ] Check old route compatibility; never invent a missing V action from coordinates alone.

**Acceptance:** Walk -> Takeoff -> Fly -> V mid-air -> Descend -> Land -> Walk completes without freezing.

### Correct Odyle interaction distance

- [ ] Port the relevant BA2 close-approach correction into BE.
- [ ] Verify approaches from below, from the side and after FP recovery.
- [ ] Leave ground-resource interaction distances unchanged.

**Status:** pending. The inspected BE baseline still uses the shared ground interaction distance for Odyle; the BA2 correction must not be marked complete here.

**Acceptance:** the character reaches the actual interaction prompt rather than stopping beneath the node.

### Land when F9 has no eligible next target

- [ ] Allow a short bounded repeat-search window after gathering.
- [ ] Request landing if no next eligible node is found.
- [ ] Confirm touchdown and continue searching from the ground.
- [ ] Bound failed landing attempts and show the reason for waiting or stopping.

**Acceptance:** no indefinite hovering until FP is exhausted when no target is available.

## P1 — Pending implementation and investigation

### Separate no-target landing from low-FP recovery

- [ ] After no-target landing, allow departure when a node appears and minimum takeoff FP is available.
- [ ] After low-FP landing, keep waiting for Resume FP.
- [ ] After a node-batch pause, require both the timer and applicable FP conditions.
- [ ] Display which condition is blocking departure.
- [ ] Preserve the separation between F9 recovery and recorded route flight actions.

**Boundary:** minimum takeoff FP is a departure condition, not a replacement for Resume FP after energy depletion.

### Investigate inconsistent F9 selection

- [ ] Compare initial target selection with sustained operation under the same filters and area.
- [ ] Explain rejections by range, resource selection, area, allowed locations, height, FP, temporary deferral or stale data.
- [ ] Change selection rules only after a discrepancy has been reproduced.

**Status:** unresolved feedback, not a confirmed single-cause detection defect.

### Support Odyle in Gather Along Route

- [ ] Extend the existing ground-oriented corridor selection to airborne resources.
- [ ] Respect XYZ positions, corridor limits, forward route order and FP.
- [ ] Rejoin the route without skipping required recorded actions.
- [ ] Verify mixed ground/air routes and cancellation.

### Odyle-to-ground-resource transition

- [ ] Approach the next selected ground resource.
- [ ] Request V at a full 3D XYZ distance of 4 meters or less.
- [ ] Apply no separate safe-height check, as requested.
- [ ] Confirm touchdown before gathering and bound failed transitions.

**Boundary:** the 4-meter trigger is the total 3D distance, not a horizontal-only or vertical-only measurement.

## Release acceptance — pending

- [ ] Mid-air cancellation and normal descent to ground.
- [ ] Ground gathering, Odyle and mixed routes.
- [ ] Area/allowed-location filters and disappearing/reappearing targets.
- [ ] Low FP, no-target landing, batch pauses, recovery and failed landing.
- [ ] Characters with different FP reserves under comparable conditions.
- [ ] Focus loss, pause, resume, stop and unavailable player/input state.
- [ ] MOBFARM and counterattack regressions.

## Documentation and distribution

- [x] Prepare an illustrated English BE 3.1 reference manual using supplied screenshots.
- [x] Choose the full 3.1 installation as the base for future compact updates.
- [ ] Update the manual, USAGE text, changelog and translations for actual 3.2 changes.
- [ ] Prepare and verify the compact 3.2 update against that base installation.
- [ ] Include changed executable/support files and the matching PDB; omit unchanged assets.
- [ ] Verify preservation of settings, routes and format compatibility.
- [ ] Publish only after acceptance and a separate release decision.

**Status:** the distribution approach is agreed; a verified 3.2 update package has not been completed.

## Not planned for BE 3.2

- Mandatory walking between every pair of nodes.
- Immediate takeoff for every target regardless of FP or route state.
- A complete new terrain pathfinder.
- Underwater gathering.
- New maps.

These exclusions are release scope decisions, not claims that the features are technically impossible. Other pending items above remain planned; they have not been silently cancelled.
