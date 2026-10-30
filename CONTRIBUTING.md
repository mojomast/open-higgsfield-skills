# Contributing

Thank you for helping. New skills, workflow fixes and test reports (especially real GPU runs) are all welcome.

## Adding a skill
1. Create `skills/<name>/` with a `SKILL.md` that follows [docs/SKILL_FORMAT.md](docs/SKILL_FORMAT.md). The folder name must equal `name`.
2. Put ComfyUI graphs in **API format** under `workflows/`. Ship your own copies of `scripts/comfy_client.py` and `scripts/preflight.py` (copy them from `skills/comfyui-connect` and `skills/comfyui-preflight`) so the folder works on its own.
3. Write `references/requirements.md`: node class types with their packs, weight files with their licences, and VRAM. Mark unknowns **unverified**.
4. If `video-director` should use the skill, add **one row** to `skills/video-director/references/routing.md` and run `python3 skills/video-director/scripts/direct.py routes`.
5. Add the skill to the README skills table.
6. Run the validation in `docs/SKILL_FORMAT.md`.

## Licence rules (strict)
- **Open weights only.** Never add a step that calls a closed or hosted generation API.
- **Never commit model weights**, checkpoints, LoRAs or large renders. Keep the repo well under 50 MB.
- You may copy files **only** from MIT, Apache-2.0 or BSD-licensed sources. Check the licence in both the upstream LICENSE file and the GitHub licence field.
- For every copied file:
  - add a provenance entry to `NOTICE` (path, upstream URL pinned to a commit, licence, copyright holder);
  - put the upstream licence text in `licenses/<owner>__<repo>.LICENSE`, plus its NOTICE file if it is Apache-2.0 and has one;
  - update `THIRD_PARTY_LICENSES.md`.
- GPL, AGPL, non-commercial and unlicensed projects may be **studied and credited as reference only**. Don't copy their code, workflows or text.
- Your own contributions are licensed under MIT.

## Honesty rules
- Don't claim a workflow is tested unless you ran it. Say what GPU you used, and add it to the Status section.
- No invented benchmarks. Label VRAM and timing estimates as estimates.
- No secrets, tokens or machine-specific absolute paths.
