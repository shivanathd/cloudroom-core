# Security audit

Review the change for sandbox isolation, token handling, and dependency risk. Report findings. Do not write exploit steps, payloads, or reproduction procedures.

## Scope

- Crates in `Cargo.toml` and the lockfile: new or bumped dependencies, features enabled, and crates that parse untrusted input or talk to the network.
- Sandbox boundaries: the agent user is separate from the core, has no admin rights and no `sudo`, and cannot read the core token, database password, or history files.
- Token parsing: missing, empty, and mismatched tokens are rejected. The listener stays on localhost unless the deployment explicitly terminates HTTPS in front of it.
- Database queries: statements in `docs/database/` use parameters. Session data, tokens, and paths are not concatenated into SQL.
- File modes: tokens, history, and diagnostics are owner-only (`0o600` files, `0o700` directories). Unsafe ownership or mode fails closed.
- `unsafe`: every block has an adjacent comment stating the invariant. Flag a new block that lacks one.

## Report

For each finding:

- File and the behavior at risk (isolation, token, query, permission, or dependency).
- Why it matters to a sandbox user or to the core process.
- The change that closes it (a check, a tighter mode, a parameter, or dropping a dependency).

If nothing in scope regressed, say so and list what you reviewed.
