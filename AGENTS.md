# AGENTS.md

## Project
This repository contains the community firmware for the Renkforce RF2000v2 / RF2000 family.
The current debugging focus is the RF2000v2 only.

## Current goal
Find and fix the root cause of incorrect first-layer Z behavior on RF2000v2.

Observed behavior:
- Prime line prints correctly at the commanded Z height.
- The actual model start prints too low and scratches/plows the printed material.
- Large Z offset changes in slicer and machine UI had little or no visible effect.
- Heat bed / PLA scan can complete, but the real print Z still appears wrong.
- The generated model-start G-code contains suspicious early Z moves before the first real extrusion path.

## Board / machine target
- Machine: Renkforce RF2000v2
- Firmware target: RF2000v2 community firmware
- The machine uses dual extruders, but current debugging focuses on single-material printing.
- Both nozzles are at the same mechanical height.
- Scan behavior and first-layer behavior are more important than dual-extruder production features.

## Priorities
1. Preserve buildability and flashability.
2. Do not make broad refactors unrelated to RF2000v2 Z/scan behavior.
3. Prefer small, reviewable patches.
4. Add instrumentation / logging before making large behavior changes.
5. Keep behavior changes RF2000v2-specific when possible.

## Areas of interest
Search first in files related to:
- Z reference / Z offset / compensation
- heat bed scan / PLA / ABS scan
- sense offset / digit / DMS logic
- first layer handling
- model-start / print-start motion paths
- RF2000v2-specific conditionals and configuration
- EEPROM-backed Z values and matrix handling

Likely relevant concepts / names:
- heat bed compensation
- zMatrix
- SenseOffset
- DMS / digit / pressure
- first layer
- scan
- RF2000v2
- static Z offset
- compensation matrix
- print start motion
- homing and post-homing Z logic

## What NOT to do
- Do not change unrelated UI/menu strings unless required.
- Do not touch RF1000-only behavior unless necessary to isolate shared logic.
- Do not modify build settings or pin mappings unless the change is directly justified.
- Do not remove safety checks without explicitly documenting why.
- Do not introduce “magic” constants without naming and commenting them.

## Preferred workflow
For any bugfix task:
1. Identify the exact code path for RF2000v2.
2. Explain which runtime path differs between:
   - manual moves / prime line
   - actual first model layer
3. Add minimal logging or debug output if needed.
4. Propose the smallest plausible patch.
5. Keep the patch focused and easy to revert.

## Debugging guidance
When investigating first-layer Z problems, compare:
- prime-line Z path
- first real model-layer Z path
- scan/compensation application path
- any post-homing or post-scan Z transformations
- any EEPROM-loaded offsets or matrices applied at print start

## Code quality
- Keep changes minimal and local.
- Comment non-obvious RF2000v2-specific fixes.
- Avoid unnecessary formatting-only edits.
- Prefer explicit names over clever code.

## Validation
After code changes, always:
1. Build the firmware for the intended RF2000v2 target.
2. Report compile success/failure clearly.
3. If debug logging is added, mention exactly where the new logs appear.
4. Summarize which hypothesis the patch is testing.

## Output expectations
For each task, provide:
- files changed
- the exact hypothesis being tested
- why the patch should affect RF2000v2 first-layer behavior
- any risks or rollback notes
