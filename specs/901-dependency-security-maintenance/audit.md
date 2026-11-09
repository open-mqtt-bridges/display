# Maintenance evidence

Issue: https://github.com/open-mqtt-bridges/display/issues/22
Base: `145758f1df81675228664dee251e6fb840603290` on `main`. Runtime used for local Node checks: Node 26.13.0; Cargo checks: Rust/Cargo 1.99.

## Baseline and selected targets

Original declared versions, aliases, engines and overrides are preserved in `baseline.json`. Registry metadata and release notes supplied target versions and peer/engine constraints.

| Dependency                   | Before    | After     |
| ---------------------------- | --------- | --------- |
| `anyhow` (Cargo)             | `1.0.100` | `1.0.104` |
| `clap` (Cargo)               | `4.5.53`  | `4.6.7`   |
| `serde` (Cargo)              | `1.0.228` | `1.0.229` |
| `serde_json` (Cargo)         | `1.0.145` | `1.0.154` |
| `tracing` (Cargo)            | `0.1.41`  | `0.1.44`  |
| `tracing-subscriber` (Cargo) | `0.3.20`  | `0.3.23`  |
| `tokio` (Cargo)              | `1.48.0`  | `1.54.1`  |
| `uuid` (Cargo)               | `1.19.0`  | `1.29.1`  |

## Compatibility decisions

- Cargo dependencies and the documentation lockfile are independently updated. Existing local nom compatibility patch, notices, native-TLS transport and device-access boundaries remain unchanged.

## Release-age policy and deferred updates

The user explicitly selected versions published at least 24 hours before update selection, following a fresh-container pnpm policy rejection. NestJS 12.4.2, dotenv 18.0.10, eventsource 5.1.5, eslint-plugin-import-x 4.18.1 and ws 8.22.2 are deferred where applicable. Further fresh Rollup/Vite/follow-redirects and Cargo fixedbitset/itoa/syn/uuid versions were checked and reverted to aged stable resolutions. No new minimumReleaseAgeExclude entries, peer/engine bypass, registry changes or build-script trust expansions are retained. Existing TypeScript aliases are preserved where tooling excludes TypeScript 7.

## Security coverage

GitHub API readback confirms Dependabot alerts, secret scanning and secret-scanning push protection enabled. Public repository dependency graphs support the enabled alerts. Available Dependabot security updates were enabled for dependency-bearing repositories. Secret-scanning open-alert lists were empty after activation. CodeQL configuration is added or retained for the repository's supported source language; successful PR/default-branch analysis must be verified separately. Dependabot entries match actual manifests, with coupled NestJS updates grouped and pnpm workspaces tracked at their lockfile root. Action major references were checked against official releases; release/deployment trigger behavior is preserved.

## Lockfile audits

- `corepack pnpm audit --json`: exit 0; {"info": 0, "low": 0, "moderate": 0, "high": 0, "critical": 0}.
- `cargo audit --json`: exit 1; {"vulnerabilities": 1, "warnings": ["unmaintained"]}.

### Residual findings

RustSec still reports [RUSTSEC-2018-0005](https://rustsec.org/advisories/RUSTSEC-2018-0005.html) for `ddc-hi@0.4.1 → mccs-db@0.1.3 → serde_yaml@0.7.5`. Both parent crates are already on their latest stable versions. Source inspection confirms that ddc-hi calls `Database::from_version`, which deserializes bundled `include_bytes!("../data/mccs.yml")`; the bridge does not expose `Database::from_database` to MQTT input. No externally supplied YAML path to this old parser was found. The finding is retained rather than ignored or silently vendoring a dependency fork. RustSec also warns that `unsafe-libyaml` and `yaml-rust` are unmaintained; replacing the YAML implementation is deferred to a separately specified compatibility migration.

## Validation

- `corepack pnpm docs:build`: passed.
- `cargo fmt --all --check`: passed.
- `cargo clippy --all-targets --all-features -- -D warnings`: passed.
- `cargo test --all-targets --all-features`: passed.
- `cargo build --release`: passed.
- Clean isolated immutable install (empty pnpm store where applicable), lockfile unchanged: passed.
- Container build with base-image pull: passed.
- Synthetic container smoke: MQTT connection/status with synthetic broker and HTTP fixtures; device instances disabled, no host devices mapped: passed.

## Delivery

PR: https://github.com/open-mqtt-bridges/display/pull/23; closes #22. Existing matching Dependabot PRs remain open and are linked as superseded. Required CI must pass on the PR current head before any separately authorized merge. No release, deployment or release-PR merge was performed.

## Hosted CI evidence

All reported checks on implementation head `aa68628b44d1982a923c7f58dfe63585ea0e4ee7` completed without failures at 2026-11-09T13:23:15.934296+00:00. Deployment jobs were intentionally skipped on the PR. CodeQL PR analysis completed for the supported source language. This final documentation-only task update follows that successful implementation verification; the current PR checks remain authoritative before merge.
