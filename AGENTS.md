# Cloudroom core

AI agent execution runtime and sandbox infrastructure for cloudroom-core.

## Project

- Keep one Rust package and one service; app integrations call the API.
- Run `cargo fmt --check`, `cargo clippy --locked --all-targets -- -D warnings`, and `cargo test --locked --all-targets`.
- Use disposable databases for tests. Only the owner applies production migrations.
- SQL and application instructions live in `docs/database/`.
- Keep credentials and internal notes in ignored files. Review changes before publication.

## Stack

Rust (edition 2024, 1.89+), PostgreSQL, Docker and Linux sandboxing, and one API service (`axum` on `tokio`).

## Test-driven development

Write or update a failing test before implementing a feature or fix. Run the test, confirm it fails for the intended reason, then make it pass. Refactor only while the suite stays green.

Baseline the suite with `cargo test --locked --all-targets` before editing so existing failures are visible.

## Rust

- Format with `cargo fmt` and lint with `cargo clippy --locked --all-targets -- -D warnings`.
- Library and service paths return `Result` or `Option`. Do not add unhandled `.unwrap()` or `.expect()` outside tests.
- Propagate errors with the crate's existing enums (for example `session::Error`). Do not add `thiserror` or `anyhow` unless the change already requires a new dependency.
- Use `tokio` for async work. Do not detach tasks that hold sessions, processes, or database work without a shutdown path.

## Security

Do not store plaintext credentials in the tree. Sandbox users stay isolated from the core token, the database password, and history files. See `README.md` and `SECURITY.md`.

Report vulnerabilities privately as described in `SECURITY.md`.
