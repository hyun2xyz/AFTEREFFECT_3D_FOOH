# AFTEREFFECT_3D_FOOH — classroom starter

Read `.agents/skills/aftereffect-3d-fooh/SKILL.md` for this repository's workflow
and `docs/rules.md` for the 3D FOOH rules. For a computer without prerequisites,
start with `START-HERE.md` and only the current OS setup. Do not attempt Python
commands before locating Python. This repository is the source of its project
skill; do not install or change global agent rules.

- Teach one next step at a time. Inspect the student's actual footage, files and
  installed tools before choosing commands. Preserve student edits and originals.
- One camera route per shot, scored on the placement region with observations
  never used for fitting. A failed gate stops the shot before character work.
- ComfyUI Partner (API) nodes spend the student's prepaid credits: show the
  estimate, get approval, submit once, keep the prompt_id, never resubmit blindly.
- Run Blender in the background with reviewed Python; never close the student's
  open project. Use Python and generated JSX for AE, never an AE MCP server.
- The student works in their own fork: `origin` is the fork, `upstream` is
  `hyun2xyz/AFTEREFFECT_3D_FOOH`. Follow `docs/fork-and-update.md`. Update with
  `git fetch upstream` + `git merge upstream/main`; never reset, force-push or
  discard the student's commits. Before a commit, check `git status` and stop if
  footage, AEP, generated media, weights or `tools.local.json` would be included.
- Codex asks for approval before network commands (git, gh, pip, brew, winget)
  and writes outside this folder. Request it and explain why; never change
  sandbox or security settings to avoid it.
- Keep footage, models, caches, renders and AEP files in ignored `work/` or a
  student-selected folder. Never commit private data, weights or app binaries.
- Missing frames, unmeasured frames and unknown app results are not passes.
- Add a script only after it worked on a real shot. Keep shot values in
  `work/<shot>/contract.json`, not in code.
