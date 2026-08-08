> This page location: APIs & SDKs > CLI > Configuration commands > deploy
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: The Neon CLI `neon deploy` command applies a neon.ts policy to a branch. It is a top-level alias of `neon config apply` with the same options, so you can reconcile a branch without the `config` prefix.

# Neon CLI command: deploy

Apply a neon.ts policy to a branch

The `deploy` command applies a `neon.ts` policy to a branch. It is a top-level alias of [`neon config apply`](https://neon.com/docs/cli/config#apply), with the same options.

```bash
neon deploy [options]
```

| Option              | Description                                                                                                                                                                                                    | Type    | Default | Required |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------- | :------: |
| `--allow-protected` | Auto-confirm applying to a branch marked protected on Neon                                                                                                                                                     | boolean | `false` |    No    |
| `--branch`          | Branch ID or name                                                                                                                                                                                              | string  | —       |    No    |
| `--config`          | Path to a neon.ts policy (defaults to walking up from cwd)                                                                                                                                                     | string  | —       |    No    |
| `--env`             | Path to a .env file to load into the environment before evaluating neon.ts (so function env values resolve from it). Existing env vars are not overridden.                                                     | string  | —       |    No    |
| `--env-pull`        | Pull the branch's Neon env vars (DATABASE\_URL, …) into a local .env after a successful apply. On by default; use --no-env-pull to skip (e.g. when injecting env at runtime with `neon-env run` / `neon dev`). | boolean | `true`  |    No    |
| `--project-id`      | Project ID                                                                                                                                                                                                     | string  | —       |    No    |
| `--update-existing` | Auto-confirm overriding existing remote settings on the branch                                                                                                                                                 | boolean | `false` |    No    |

For non-interactive use (scripts, CI, agents), pass `--update-existing` and `--allow-protected` to auto-confirm the corresponding prompts.

```bash
neon deploy --branch feature/auth --update-existing
```

---

## Related docs (Configuration commands)

- [config](https://neon.com/docs/cli/config)
- [status](https://neon.com/docs/cli/status)
- [dev](https://neon.com/docs/cli/dev)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/deploy"}` to https://neon.com/api/docs-feedback — no auth required.
