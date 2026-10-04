# MUSE Skills

[简体中文](README.md) | **English**

Four MUSE skill packages support original fiction and screenplays, literary analysis, serial writing, and serial analysis. They apply Robert McKee's story principles and retrieve examples from a literary knowledge base to guide outlining, character performance, scene writing, and revision.

[Paper](https://arxiv.org/abs/2609.15188) · [Full project](https://github.com/RoadtoAGI/MUSE) · [Hugging Face resources](https://huggingface.co/datasets/RoadtoASI/MUSE-skills) · [Knowledge base](KNOWLEDGE_BASE.md)

## Latest release: review and writing skill updates

**2026-10-04**. Original writing **v2.19.0**, literary retrieval **v0.10.0**, serial writing **v0.8.1**, and serial analysis **v0.3.1**. The shared runtime remains **muse-runtime v0.1.0**.

This release updates review paths for publication, evaluation, and smoke runs while retaining independent reader review. It streamlines writer instructions and makes the machine revision lane opt-in. Original and serial writing gain dialogue, action, and revision examples; literary retrieval adds short exemplar excerpts around source-text anchors.

## JEV-assisted writing

JEV assesses retrieved scene passages and inspiration cards to help select references for the current task. In fiction and serial brainstorming, it compares developed plot candidates and supports revision, reconstruction, and recommendations. Serial brainstorming uses the existing story, character knowledge, established constraints, and author feedback to explore future directions. Accepted decisions feed into outlines and chapter plans.

Install the runtime in your chosen Python environment:

```bash
python3 -m pip install "git+https://github.com/RoadtoAGI/MUSE-skills.git@muse-runtime-v0.1.0"
python3 -m muse_runtime mode set jev --work-dir /absolute/path/to/work
python3 -m muse_runtime brainstorm guide
```

Standard mode is the default; installing a plugin does not enable JEV. Supply `MUSE_JEV_API_KEY` through the execution environment; `MUSE_JEV_TUZI_API_KEY` optionally enables the Tuzi backup channel. Relevant queries and candidate content are sent to the configured service. Serial brainstorming binds to the series root; chapter reference retrieval binds to the current chapter directory. Set each mode explicitly. See the [runtime guide](src/muse_runtime/brainstorm-guide.md) and package READMEs.

## Packages

| Plugin | Purpose |
|---|---|
| [muse-writing](plugins/muse-writing/README.md) | Original short and medium-length fiction; original and adapted screenplays |
| [muse-canon-distill](plugins/muse-canon-distill/README.md) | Literary analysis and retrieval of structural, character, dialogue, and prose references |
| [muse-serial-writing](plugins/muse-serial-writing/README.md) | Serial fiction, fan fiction, sequels, and spin-offs |
| [muse-serial-distill](plugins/muse-serial-distill/README.md) | Analysis of long or ongoing fiction and preparation of continuation materials |

For original writing, install `muse-writing` and `muse-canon-distill`. For serial writing, install the two serial packages and add `muse-canon-distill` for literary references. Install all four for the complete collection.

## Install in Claude Code

Add the marketplace once, then install the packages you need:

```text
/plugin marketplace add RoadtoAGI/MUSE-skills
/plugin install muse-writing@muse-skills
/plugin install muse-canon-distill@muse-skills
/plugin install muse-serial-writing@muse-skills
/plugin install muse-serial-distill@muse-skills
```

Reload plugins or start a new session as instructed by the installer. For example:

```text
/muse-writing:short-story-writing Write a suspense story set in a railway station at midnight.
```

Plugin namespaces distinguish shared skill names: `muse-writing:writer` and `muse-serial-writing:writer` belong to different workflows. Start with a qualified entry point so its orchestrator can select the appropriate package skills.

## Install in Codex

Run in a terminal:

```bash
codex plugin marketplace add RoadtoAGI/MUSE-skills
codex plugin add muse-writing@muse-skills
codex plugin add muse-canon-distill@muse-skills
codex plugin add muse-serial-writing@muse-skills
codex plugin add muse-serial-distill@muse-skills
```

Start a new session and name the package and entry point in your request, such as “Use short-story-writing from muse-writing to write a suspense story set in a railway station at midnight.”

Run scripts from the installed package root. Claude examples use `${CLAUDE_PLUGIN_ROOT}`; other hosts should locate the package from the loaded `SKILL.md` path. The bundled Claude hooks use Claude Code's hook runtime.

## Knowledge base

The reference packages include literary analyses, scenes, character materials, and retrieval scripts. Install Python dependencies according to each package's instructions. Vector retrieval also requires an embedding service and rebuilt indices; see [configuration commands](KNOWLEDGE_BASE.md#rebuild-retrieval-embeddings).

Story principles guide choices under pressure, scene value changes, and causal plotting. Retrieved examples show how these decisions appear in literary works: designers use structural analysis, character performers use dialogue sequences, and writers use scene passages and style annotations. Selected references accompany design decisions through drafting and revision.

Most instructions and annotations are in Chinese. The [corpus guide](KNOWLEDGE_BASE.md) identifies English source texts.

## Updates

In Claude Code, refresh the marketplace and update installed packages:

```text
/plugin marketplace update muse-skills
/plugin update muse-writing@muse-skills
```

Repeat the update command for other installed packages. In Codex, check updates through plugin management; consult `codex plugin --help` for the commands available in your installed version.

When migrating from standalone repositories, uninstall the old plugin sources before installing from `muse-skills` to avoid enabling duplicate copies.

## Source

This repository distributes skill packages from [MUSE](https://github.com/RoadtoAGI/MUSE), including their scripts, references, knowledge resources, and plugin configuration. [SOURCE.json](SOURCE.json) records the source revision. Development takes place in the main project, with releases synchronized here.

Rights to literary works and translations remain with their respective authors and publishers. See [resource provenance](KNOWLEDGE_BASE.md#resource-provenance) for source information and the removal contact process.

Paper: **[MUSE: A Theory-Harnessed Story Engine for Vibe Narrativizing](https://arxiv.org/abs/2609.15188)**.
