# Readiness verification

Verified locally on 2026-11-09. Public deployments remain pending the user’s merge approval.

## Checks

| Command | Result |
| --- | --- |
| `cargo fmt --all --check` | Passed |
| `cargo clippy --all-targets --all-features -- -D warnings` | Passed |
| `cargo test --all-targets --all-features` | Passed |
| `cargo build --release` | Passed |
| `pnpm docs:build` | Passed |

Spec Kit 1.3.2 manifest hashes, nine Codex skills, Bash syntax and explicit feature prerequisites passed. Disposable setup smoke tests cover legacy TypeScript, modern TypeScript, Rust and the website. Generated tooling is excluded from application lint, not product source.

The VitePress build and internal file/asset references passed. Direct configuration routes and logos were checked in the local browser for all eight documentation sites. Pages PR builds cannot deploy; deployment is guarded to the default branch. Live HTTP/CSS/JS verification remains pending merge.

## Review boundaries

Runtime source, MQTT contracts and container coordinates are unchanged. Existing licenses and vendored notices are preserved. The shared organization audit records dependency advisory counts and limitations; this scoped review is not a complete security or redistribution-license certification.
