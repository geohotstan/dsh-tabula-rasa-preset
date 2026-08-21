# tabula-rasa-preset

> **The ultimate YOLO presets.** No, not even bash is included. Start from nothing and build-a-harness with your DeepSeek Harness. Invent and reinvent everything.

Two out-of-tree [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) agent presets. Both hold `tool-cordis` — the self-referential Cordis toolset (`cordis_inspect_*`, `cordis_define`, `cordis_run`, `cordis_stop`, `cordis_undefine`) — and nothing that gives the agent a capability. No shell, no filesystem tools, no editor. Whatever the agent needs, it designs and registers itself as dynamic Cordis packages, implemented over the host services the sandbox facade exposes (`ctx.fs`, `ctx.web`, `ctx.bash`, Cordis timers).

| | `void/` | `tabula-rasa/` |
|---|---|---|
| System prompt | **Empty.** An erasure persona (`text: ''` + `complete: true`) suppresses every section — the model sees only tool schemas. | The blank-slate doctrine: one faculty (read the runtime, write plugins), what a live plugin may change, and how change outlives the process. |
| Support rows | None | Workspace instruction files (AGENTS.md/CLAUDE.md) + context compaction (fold + tool-result pruning) |
| Tools | cordis toolset only | cordis toolset only |

In both, the agent can even rewrite its own prompt: a plugin it runs can add or replace sections of the system prompt it reads. And because everything it invents dies with the process, the durable act is authoring a new preset under `~/.dsh/.agent-presets/` — the tabula-rasa persona says so outright, and tells the agent these very instructions are one row in such a file, to reinvent freely.

## Requirements

- A dsh host composition that mounts `@deepseek-ai/dsh-cordis-host-runner`. The **web profile** does (`dsh web`); the headless profile does not — there the `cordis_*` tools never activate.
- This is YOLO for real: `cordis_define`/`cordis_run` evaluate model-written JavaScript against the live runtime. Treat a session on either preset as shell access, because it is.

## Install

The directory name under the user preset root becomes the preset id:

```sh
git clone git@github.com:geohotstan/tabula-rasa-preset.git
cp -R tabula-rasa-preset/tabula-rasa ~/.dsh/.agent-presets/tabula-rasa
cp -R tabula-rasa-preset/void ~/.dsh/.agent-presets/void
```

Discovery re-reads the roots on every roster read, so a running `dsh` process picks them up with no restart. Pick one on the new-session screen, or make it the default in `~/.dsh/settings.yaml`:

```yaml
agent-presets:
  default: void
```

## Notes

- Dynamic packages are session-scoped and vanish on restart; the tabula-rasa persona teaches the durability path — authoring a preset directory under `~/.dsh/.agent-presets/` via a self-made file-writing tool over `ctx.fs`. Void teaches nothing; that is the point.
- The empty-system-prompt trick in `void` works because empty persona text drops the persona section at render while `complete: true` suppresses every other section. Delete the erasure row and the deployment's default persona renders instead — less nothing, more default.
