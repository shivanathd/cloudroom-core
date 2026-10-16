# Rust

Cloudroom core is one crate (`cloudroom`, edition 2024, Rust 1.89+). Keep that boundary.

## Format and lint

Before finishing a change, run:

```sh
cargo fmt --check
cargo clippy --locked --all-targets -- -D warnings
```

Fix warnings. Do not silence a lint with `#[allow]` unless the comment next to it states why the warning is a false positive.

## Errors

Service and library paths return `Result` or `Option`. Match the existing handwritten enums, such as `session::Error` (`NotFound`, `Conflict`, `Storage`) with `From<io::Error>` and a module-local `Result`.

Do not add `thiserror` or `anyhow` unless the change already needs a new dependency. Prefer propagating with `?` over mapping into a string at every call site.

`.unwrap()` and `.expect()` belong in tests. Do not add them on service paths.

## Async and ownership

Use `tokio` for async work. The runtime features already enabled in `Cargo.toml` are the ones to use.

Do not spawn a task that holds a session, a child process, a socket, or database work without a shutdown path that joins or aborts it when the session or process stops.

Keep ownership of buffers, file handles, and child processes explicit. Drop or close them on the path that created them. Do not clone large session or replay state to satisfy a borrow when a reference or an index is enough.
