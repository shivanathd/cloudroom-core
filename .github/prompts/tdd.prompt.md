# Rust TDD loop

Use red-green-refactor. The failing test comes before the production change.

## 1. Baseline

Run the locked suite and note anything already failing:

```sh
cargo test --locked --all-targets
```

## 2. Red

Write or update one test that expresses the new behavior:

- Core logic: a unit test in `src/` or a Rust test in `tests/`.
- Sandbox, storage, preview, or database workflow: extend the matching script under `tests/` (`core_e2e.py`, `storage_e2e.py`, or the disposable check CI already runs).

Run that test alone, then the full suite if the filter is ambiguous. Stop when it fails for the reason the change is supposed to fix. Do not weaken an existing assertion to make the run green.

## 3. Green

Implement the smallest change that makes the new test pass. Library and service paths return `Result` or `Option`. Propagate with the existing error enum. Do not add `.unwrap()` or `.expect()` outside tests.

Re-run:

```sh
cargo test --locked --all-targets
```

## 4. Refactor

Simplify names, ownership, and control flow while the suite stays green. Then:

```sh
cargo fmt --check
cargo clippy --locked --all-targets -- -D warnings
```

The test and the implementation ship together. Do not leave the new test uncommitted relative to the fix.
