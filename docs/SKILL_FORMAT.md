# SKILL.md format

Skills here follow the open [Agent Skills](https://agentskills.io) format, so any compatible agent can load them.

## Folder

```
skills/<name>/
  SKILL.md                      # required
  scripts/                      # optional; Python 3 standard library only
  workflows/*.api.json          # optional; ComfyUI API-format graphs
  references/requirements.md    # required if the skill renders anything
  references/...                # optional docs, presets, upstream notes
  assets/...                    # optional bundled custom-node packs (keep their LICENSE)
```

## Frontmatter

`SKILL.md` starts with YAML frontmatter that has exactly two keys:

```yaml
---
name: video-upscale
description: Upscale and restore generated video or stills with open-weights SeedVR2 in ComfyUI. Use after ...
---
```

- `name`: lowercase letters, digits and single hyphens (`^[a-z0-9]+(-[a-z0-9]+)*$`), at most 64 characters, and **identical to the folder name**.
- `description`: one line, 1 to 1024 characters. Say what the skill does, which open-weights models it uses and **when to use it** ("Use when ..."), because agents pick skills by this text.

## Body

Write plain Markdown instructions for an agent:
1. What the skill does and when to use it (or not).
2. Setup: custom nodes to install, where bundled `assets/` go, and weight files (point to `references/requirements.md`).
3. How to run it: the exact `python scripts/comfy_client.py --url $COMFY_URL run workflows/<file> --set NODE.INPUT=VALUE --out out/` commands and the node inputs to set.
4. Model-specific prompting rules, failure modes and fixes.
5. Licence caveats for the weights.

## references/requirements.md

List every node `class_type` with the pack that provides it, every weight file with where it goes and its licence, and a VRAM note. Mark anything not confirmed as **unverified**.

## Validation

```bash
python3 - <<'PY'
import glob,re
for f in sorted(glob.glob('skills/*/SKILL.md')):
    m=re.match(r'---\n(.*?)\n---\n',open(f).read(),re.S); d=dict(l.split(': ',1) for l in m.group(1).splitlines())
    assert set(d)=={'name','description'} and d['name']==f.split('/')[1], f
    assert re.fullmatch(r'[a-z0-9]+(-[a-z0-9]+)*',d['name']) and len(d['name'])<=64 and 0<len(d['description'])<=1024, f
print('ok')
PY
python3 skills/video-director/scripts/direct.py routes
```
