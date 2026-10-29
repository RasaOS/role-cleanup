# role-cleanup

**Version:** 0.1.1 · `rasa.module.role.cleanup` · public

The **Cleanup** role (`cleanup`) as a RasaOS role module. Sweeps every session on its node that is no longer active, whoever started it. Saves their work to a branch and a PR, flags what it cannot save, then ends them so the worker list stays clean.

| File | What it is |
|---|---|
| `role.json` | The role's fields (id, name, summary, version, consumers, knowledge, takes, kinds, grants, powers). galt publishes them to `/tenant_config/roles/cleanup`. |
| `prompt.md` | The role's prompt, byte for byte the body of `roles/cleanup.md` in vsi-tenant. |
| `.claude-plugin/plugin.json` | The plugin manifest, which makes the repo loadable into a session with `--plugin-dir`. |
| `rasa.json` | The Element manifest: `kind: module`, with the role's id and version under `rasa`. |
| `bin/check-role` | The release gate. |

Role version **1**, from `roles/cleanup.md` at vsi-tenant `fd0c872`.

**Powers.** `session_end`, when the role is assigned active.

**Work.** It takes `sweep` through `galt work`, per the tenant's `roles/WORK.md`.

## Loading it

- **As an agent's brain:** `galt agent new <name> --brain RasaOS/role-cleanup@v0.1.1`. galt reads `role.json` and `prompt.md`. There is no role assignment this way, so no grants.
- **As the source of the published role:** the tenant's `roles/cleanup.md` becomes the one line `element: RasaOS/role-cleanup@v0.1.1`. After a person runs Publish roles, every agent with role `cleanup` loads this Element. galt's scheduler supports this; it advertises `role_elements` (galt ADR 0064).
- **Public:** any computer can fetch it. No vault token is needed.

## Changing the role

Until the tenant's `roles/cleanup.md` points here, that file is what Publish roles reads, and this repo is a snapshot of it. Change both together; see `CLAUDE.md`.
