# Security

Sandbox isolation, token handling, and database access are the boundaries that matter in this crate.

## Checklist

Review these before finishing a change that touches runtime, API, storage, or SQL:

- Sandbox users are separate Linux users. They have no admin rights and no `sudo`. They cannot read the core token, the database password, or history files.
- Every API request requires the secret token. Parsing and comparison of that token reject missing, empty, and mismatched values. The API listens on localhost unless it is explicitly placed behind HTTPS.
- Database statements live in `docs/database/`. Queries pass values as parameters. Do not interpolate session data, tokens, or paths into SQL text.
- New files that hold tokens, history, or diagnostics are owner-only (`0o600` for files, `0o700` for directories). Do not create world-writable or group-writable paths. Do not "repair" a file that fails an ownership or mode check; refuse it.
- Credentials and internal notes stay in ignored files. Do not commit plaintext tokens, database passwords, or private keys.

## Blocked

- Hardcoded secrets, including tokens and passwords in source, tests, fixtures, or workflow files.
- `unsafe` blocks without an adjacent comment that states the invariant. Existing diagnostic checks in `src/observability/mod.rs` (`geteuid`, mode `0o700` / `0o600`) are the pattern: the comment names why the block is safe, and the surrounding code refuses unsafe files.
- Insecure file permissions, symlink following into another user's files, and widening a sandbox user's access to the core process.

## Reports

Do not open a public issue for a vulnerability. Report it privately as described in `SECURITY.md`.
