# tabula-rasa-preset

> **The ultimate YOLO preset.** No, not even bash is included. Start from nothing and build-a-harness with your DeepSeek Harness. Invent and reinvent everything.

An out-of-tree [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) agent preset: a barebones self-composing agent. Two rows only — a `complete: true` persona and `tool-cordis`. No shell, no filesystem tools, no editor, no nothing. Whatever the agent needs, it designs and registers itself as dynamic Cordis packages (`cordis_define` → `cordis_run`), implemented over the host services the sandbox facade exposes (`ctx.fs`, `ctx.web`, `ctx.bash`, Cordis timers).

It can even rewrite its own prompt: a plugin it runs can add or replace sections of the system prompt it reads. And because everything it invents dies with the process, the durable act is authoring a new preset under `~/.dsh/.agent-presets/` — these very instructions are one row in such a file, and the agent is told to reinvent them freely.

## Requirements

- A dsh host composition that mounts `@deepseek-ai/dsh-cordis-host-runner`. The **web profile** does (`dsh web`); the headless profile does not — there the `cordis_*` tools never activate.
- This is YOLO for real: `cordis_define`/`cordis_run` evaluate model-written JavaScript against the live runtime. Treat a session on this preset as shell access, because it is.

## Install

The directory name under the user preset root becomes the preset id:

```sh
git clone git@github.com:geohotstan/tabula-rasa-preset.git
cp -R tabula-rasa-preset ~/.dsh/.agent-presets/tabula-rasa
```

Discovery re-reads the roots on every roster read, so a running `dsh` process picks it up with no restart. Pick "Tabula Rasa" on the new-session screen, or make it the default in `~/.dsh/settings.yaml`:

```yaml
agent-presets:
  default: tabula-rasa
```

## Notes

- Dynamic packages are session-scoped and vanish on restart; the persona teaches the durability path — authoring a preset directory under `~/.dsh/.agent-presets/` via a self-made file-writing tool over `ctx.fs`.
- A long build session overflows context without compaction. If that bites, copy the compaction group (the `cordis:group` with `compaction-basic`, `command-compact`, `tool-result-pruner`) out of the `standard` shipped preset into this file.
