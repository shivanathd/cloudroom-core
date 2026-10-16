# Test-driven development

Write or update the failing test before the implementation.

## Before editing

Run the locked suite so you know what is already red:

```sh
cargo test --locked --all-targets
```

## Loop

1. **Red.** Add or change a unit test for the core logic, or extend an existing integration check for a sandbox or storage workflow. Run it and confirm it fails for the reason you intend.
2. **Green.** Implement the smallest change that makes that test pass. Re-run `cargo test --locked --all-targets`.
3. **Refactor.** Clean the code only while the suite stays green. Then run `cargo fmt --check` and `cargo clippy --locked --all-targets -- -D warnings`.

## Where tests live

- Unit tests for core logic live next to the code in `src/` and in the Rust integration tests under `tests/` (`adapters`, `journal`, `runtime`, `storage`, `terminal`).
- Sandbox, storage, preview, and database workflows are covered by the Python and Node suites under `tests/`, including `core_e2e.py` and `storage_e2e.py`. Extend those when the change crosses a process, disk, or database boundary.

## Databases

Use a disposable database for tests. Do not apply production migrations. SQL files in `docs/database/` are applied by the owner, not by the agent and not by the service.
