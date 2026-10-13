<h1 align="center">Mol* Story</h1>

<p align="center">
  <strong>Evidence-backed, interactive stories for structural biology.</strong><br>
  Turn molecular structures and measured results into a traceable visual argument—one scene, comparison, and claim at a time.
</p>

<p align="center">
  <a href="SKILL.md"><img src="https://img.shields.io/badge/Codex-skill-4C6EF5" alt="Codex skill"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="MIT license"></a>
  <a href="https://github.com/Molaison/molstar-story/stargazers"><img src="https://img.shields.io/github/stars/Molaison/molstar-story" alt="GitHub stars"></a>
</p>

<p align="center">
  <a href="#what-it-does">What it does</a> ·
  <a href="#install-the-skill">Install</a> ·
  <a href="#build-a-story">Build a Story</a> ·
  <a href="#documentation">Documentation</a>
</p>

---

## What it does

**Mol* Story** is a [Codex skill](SKILL.md) for answering structural-biology questions with interactive [Mol*](https://molstar.org/) / MolViewStories scenes. It connects **scientific evidence** to **what a reviewer sees**.

| Question | Story workflow |
| --- | --- |
| Where and how does a ligand bind? | Compare pocket geometry, interacting residues, and chemical context. |
| What differs between two states? | Select comparable structures, align them, and visualize supported differences. |
| Which interface or mutation matters? | Highlight mapped residues and experimentally or computationally supported observations. |
| What can the structures *actually* establish? | Link each claim to evidence and make gaps or hypotheses explicit. |

The workflow runs from **question → source selection → quantitative analysis → claim/evidence ledger → interactive scenes → browser acceptance**. Mol* renders the result; it does not replace the research or analysis steps.

## Install the skill

Clone this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/Molaison/molstar-story.git ~/.codex/skills/molstar-story
```

Then request a structure-focused Story in Codex. For example:

> Use molstar-story to compare the apo and ligand-bound states of this protein. Check that the structures are comparable, show the relevant pocket changes, and label claims that the evidence cannot establish.

The skill can start from a scientific question and retrieve candidate evidence, or work from structures and measurements you have already provided. Its detailed operating contract is in [SKILL.md](SKILL.md).

## Build a Story

Two maintained starters cover common review patterns:

| Starter | Best for |
| --- | --- |
| [Ligand pocket](assets/ligand-pocket-story/) | Pocket overview → local interactions → decision |
| [State comparison](assets/state-comparison-story/) | Reference/mobile context → aligned comparison → interpretation |

You can also author custom scenes for interfaces, mutations, density, and other supported MolViewSpec representations.

**Runtime:** Python 3.10+ and NumPy for analysis; Deno and the **pinned** `molstar/mol-view-stories` checkout for building. Browser acceptance uses Playwright, Chromium, and Pillow. Check the exact version and evidence requirements in [SKILL.md](SKILL.md).

Once the story source has been populated with the correct data, evidence, and scene definitions:

```bash
python3 scripts/build_molstar_story.py path/to/story_source path/to/output \
  --mol-view-stories-repo path/to/pinned/mol-view-stories \
  --deno path/to/deno --deno-dir path/to/deno-cache \
  --name state_comparison
```

The build produces:

- `state_comparison.mvstory` — editable Story.
- `state_comparison_file_openable.html` — standalone HTML that can open from a local file.
- A provenance manifest identifying inputs, tool versions, and output checksums.

A successful build **does not establish scientific correctness or browser usability**. Run the viewer acceptance checker:

```bash
python3 scripts/check_story.py path/to/output/state_comparison_file_openable.html \
  --output-dir path/to/output/qa --expected-scenes N \
  --scene-settle-ms 12000 --chromium-software-webgl
```

Replace `N` with the actual number of scenes. Inspect the decision-relevant scenes and test rotate, zoom, and picking interactions. For aligned states, [`scripts/analyze_pair.py`](scripts/analyze_pair.py) supports the documented matched-structure workflow.

## Scientific guardrails

- **Use comparable inputs.** Record structures, chains, assemblies, ligand identity, residue numbering, and exclusions before interpreting an overlay.
- **Measure before illustrating.** Alignment transforms, contacts, RMSD, or residue shifts are computed upstream and recorded with their provenance.
- **Expose uncertainty.** An animation between two structures does not prove the physical pathway between them; unsupported mechanism steps remain visible as gaps.
- **Keep scenes inspectable.** Use a consistent camera, color semantics, and enough structural context to support the claim without overloading the viewer.

## Documentation

| Need | Reference |
| --- | --- |
| End-to-end skill instructions and pinned runtime | [SKILL.md](SKILL.md) |
| Evidence retrieval and candidate selection | [Evidence retrieval](references/evidence-retrieval.md) |
| Matching and analyzing molecular states | [State comparison](references/state-comparison.md) |
| Story layout, semantics, packaging, and acceptance | [Story authoring](references/story-authoring.md) |
| Question patterns and claim coverage | [Story archetypes](references/story-archetypes.md) |
| Current API/source documentation routes | [Documentation router](references/documentation-router.md) |

## License

[MIT](LICENSE) © Molaison.
