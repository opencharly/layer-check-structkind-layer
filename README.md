# check-structkind-layer

Deployable member of the `check-structkind` foundation bed.

The `check-structkind-layer` candy drops `/etc/check-structkind-marker` on the
host filesystem. It is the deployable member of the `check-structkind` bed
(F5 authored-member input-threading): a `kind: local` layer whose `write:` step
lands a marker asserted by a `check:` authored on the **deploy node's** plan.

A composed candy's `plan:` never reaches a deploy-scope check runner, so a check
authored in this layer would never run at deploy scope — the assertion lives on
the deploy node instead. It uses a dedicated marker path (not `check-local`'s) so
the beds never collide when `/verify-beds` fans them out concurrently.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `check-structkind-layer` |
| Packages | none |
| Artifact | `/etc/check-structkind-marker` (version-stamped marker, mode `0644`) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-bed:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-check-structkind-layer:v2026.239.1620'
```

The layer writes the marker and asserts its presence, its version stamp
(`check-structkind v1`), and — at runtime, on the host — its existence via a
`command:` step.

## Layout

- `charly.yml` — the `check-structkind-layer:` candy entity: the `write:` marker
  step and its `check:` assertions.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-check:check` — the check bed and `plan:` authoring
  reference (this repo declares no `skill:` entity; the gap is tracked in
  [`opencharly/opencharly#291`](https://github.com/opencharly/opencharly/issues/291))
- The deployable member of the `check-structkind` foundation bed (F5
  authored-member input-threading)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
