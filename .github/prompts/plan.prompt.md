# Plan a Rust change

Break a complex change into a reviewable plan before editing. Stay inside the one `cloudroom` crate.

## Current behavior

- What the code does today, and which request, session command, or sandbox step exercises it.
- The error type the path already returns (`session::Error` or the module's local `Result`).

## Affected modules

Name only the modules this change touches:

- `api` — HTTP surface
- `session` — session state, journal, history
- `runtime` — agent processes and sandbox execution
- `workspace` — files and storage layout
- `sync` — replication of session data

Call out PostgreSQL statements in `docs/database/` and any Docker or Linux user boundary.

## Boundaries

- Data: which rows, files, or history streams change, and who can read them.
- Sandbox: whether a sandbox user could see the core token, database password, or history files after this change.
- Dependencies: do not add a crate (including `thiserror` or `anyhow`) unless the change cannot be expressed with the existing error enums.

## Tests first

List the failing test you will add before implementation:

- Unit test in `src/` or `tests/` for core logic.
- Python or Node check under `tests/` when the change crosses a process, disk, or database boundary.
- Disposable database only. Do not apply production migrations.

## Checks

```sh
cargo test --locked --all-targets
cargo fmt --check
cargo clippy --locked --all-targets -- -D warnings
```
