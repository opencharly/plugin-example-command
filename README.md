# plugin-example-command

The reference **command-class** plugin (`command:examplecommand`) — a plugin that
contributes a real top-level CLI verb to charly.

On `charly examplecommand <args…>`, charly's CLI dispatch resolves this plugin's
binary (host-built from source, or baked into a deployed image) and execs it with
the pass-through tokens in CLI mode. The plugin owns real terminal stdio and does
one observable thing: it prints the joined args to stdout, so a test can assert
the command ran, received its args, and reached a real stdout.

## What it provides

| Capability | Surface |
|---|---|
| `command:examplecommand` | the `charly examplecommand <args…>` CLI verb |

The command class is external-capable: charly prescans `examplecommand` into the
CLI grammar and fork/execs the plugin binary on invocation. It is the
command-class companion of the verb-class `plugin-example-external`.

## How to use it

Compose the plugin candy, then invoke the command:

```
charly examplecommand hello world
# prints: hello world
```

## Layout

- `candy/plugin-example-command/` — the plugin module: `plugin.go` (the provider
  + `NewProvider()`/`NewMeta()` + `CliMain`/`runCommand`),
  `schema/examplecommand.cue` (the served declaration surface),
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:plugin` — the plugin/provider model (incl. the
  `command` class). This candy carries no `skill:` entity of its own; the gap is
  tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
