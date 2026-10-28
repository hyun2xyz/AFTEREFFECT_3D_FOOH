---
name: aftereffect-3d-fooh
description: Set up a clean macOS/Windows computer and teach a 3D FOOH shot - a camera matched to live footage (static line camera, COLMAP, or planar track), a Tripo character generated once through ComfyUI API nodes, Blender grounding/physics/render passes and a Python-generated After Effects composite. Use in this repository for a student's shot; no AE MCP.
---

# Aftereffect 3D FOOH

This is a repository-bound teaching skill. Run from the repository root that
contains `scripts/setup_environment.py`. Rules live in
[rules](../../../docs/rules.md); lesson order in [workflow](../../../docs/workflow.md).

## First interaction

1. Inspect the actual OS/shell and repository root. If Python or tools are
   missing, start with [START-HERE](../../../START-HERE.md) and read only the matching
   setup: [macOS](../../../docs/setup-macos.md) or [Windows](../../../docs/setup-windows.md).
2. Run `setup_environment.py --check`. Install only what is missing from the
   official sources in the setup document. After Effects and the Comfy account
   (credits) belong to the student, who signs in, pays and accepts terms.
3. Locate the student's clip and create `work/<shot>`. Keep the original intact.
4. Explain the next step in two or three sentences, then do only that step.

## Non-negotiable rules

- Choose one camera route per shot: A static camera from scene lines, B COLMAP
  (use the AFTEREFFECT_COLMAP repository; fix focal from scene lines when free
  focal collapses on forward ground-dominant motion), C planar ground track with
  a fixed-K plane camera and placement limited to that plane.
- Write `contract.json` (placement region, holdout points/lines, gate,
  assumptions) before solving. Score the placement region with observations
  never used for fitting: p95 <= 1.5 px and max <= 3 px at 1920x1080 unless the
  student sets another gate in advance. Keep every frame in the denominator.
- State unknown lens, scale and depth as artist assumptions. Never call an
  interpolated, smoothed or plane-limited camera a measured free-depth solve.
- Pass a fixed gray-object attachment review over the whole shot before any
  character, physics or material work.
- ComfyUI Tripo API nodes cost prepaid credits. Show the API-format workflow and
  the node's estimated credits, get the student's approval, write an attempt
  record, submit once and keep the prompt_id. If the outcome is unclear, inspect
  `/history` and `/queue`; never resubmit blindly.
- Repair generated meshes without moving foot vertices. Check foot contact at
  quarter-frame steps; fix pops with support IK, not by blurring or fading.
- Bake simulations once, verify a fresh-process cache replay, and measure
  penetration, bounces and settling separately. Do not rebake a completed cache.
- Render gray, material, light/shadow and final passes with the same camera and
  frame clock. Match blur, grain and exposure on the CG only.
- Build the AE project with `scripts/ae_comp.py` ([ae-comp](../../../docs/ae-comp.md)):
  it writes JSX the student runs in a new, empty project through
  File > Scripts > Run Script File; then read `build_comp.result.json` with the
  printed `job_id`. No MCP, persistent bridge, overwrite of open
  projects, automatic save or render.
- If the camera gate or gray attachment fails, stop before breakdown or publishing.

## Context and long work

Run Blender as a background process with reviewed Python scripts; never close
the student's open GUI project. Summarize large arrays with Python and read short
JSON reports. Run long bakes and renders with the agent's own background facility,
one at a time. After each lesson write `work/<shot>/next.md`: files, what was
verified, visible problems, one next step.

## Growing the skill

Move a step into `scripts/` only after it worked on a real shot. Keep shot values
(focal, scale, simulation settings, frame ranges) in `contract.json`, not in code.
Add new failure patterns and fixes to `docs/rules.md` with the shot they came from.
