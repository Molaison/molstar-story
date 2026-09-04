# Mol* Story

An evidence-backed Codex skill for researching structural-biology questions and building decision-focused interactive Mol* Story/MolViewStories artifacts.

## Contents

- `SKILL.md` — skill instructions
- `references/` — routed evidence, comparison, authoring, and documentation guidance
- `scripts/` — pair analysis, Story build, and offline browser acceptance helpers
- `assets/` — maintained Story starters

## Use

Install the repository as the `molstar-story` skill, or copy this directory into your agent skill collection. Analysis uses Python 3.10+ and NumPy; builds use Deno with the pinned MolViewStories checkout; browser acceptance uses Playwright, Chromium, and Pillow. Follow `SKILL.md` for the pinned runtime and acceptance commands.

The skill separates upstream scientific evidence/analysis from Mol* presentation and keeps unsupported mechanism edges explicit.
