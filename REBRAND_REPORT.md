# REBRAND_REPORT — Stalwart → NuwaMail

## 1. Summary

This repository (`github.com/finalverse/nuwamail`) is a fork/baseline derived
from **Stalwart Mail Server** `0.16.11` (fork commit `9525a891`). This change set
performs a **production-safe rebrand** to **NuwaMail**: user-facing product
naming, documentation, deployment/Docker naming, example deployment hostnames,
and cosmetic protocol banner strings — **without** altering any mail-server
protocol, storage, authentication, queueing, indexing, schema, or migration
behavior.

Guiding principle followed: *small, safe, documented patches; preserve all legal
attribution; preserve internal/machine-readable identifiers; preserve upstream
mergeability.* No 🔴 behavioral changes were made. Where a rebrand would be
risky, the original was **left unchanged and documented** (see §7–8).

Attribution and licensing are preserved in full — see
[`FORK_NOTICE.md`](./FORK_NOTICE.md). Every non-trivial change is itemized in
[`PATCHES.md`](./PATCHES.md).

## 2. Files changed

**New files (additive):**
- `FORK_NOTICE.md` — upstream attribution + license note
- `PATCHES.md` — per-change log with risk + merge-conflict assessment
- `REBRAND_REPORT.md` — this report
- `img/nuwamail-logo.svg` — text placeholder logo (upstream `img/logo-red.svg` left intact)
- `docker-compose.example.yml` — example NuwaMail deployment (service `nuwamail`)

**Modified files:**
- `README.md` — full product rebrand + attribution/positioning
- `Dockerfile` — OCI labels + `nuwamail-server` command alias (binary/paths preserved)
- `crates/common/src/lib.rs` — `USER_AGENT`, `DAEMON_NAME` (`PROD_ID` preserved)
- `crates/utils/src/http.rs` — outbound user-agent
- `crates/imap/src/lib.rs`, `crates/imap/src/op/logout.rs`, `crates/imap/src/op/capability.rs` — IMAP greeting / BYE / ID
- `crates/managesieve/src/lib.rs`, `crates/managesieve/src/op/capability.rs`, `crates/managesieve/src/op/logout.rs` — ManageSieve greeting / IMPLEMENTATION / BYE
- `crates/pop3/src/lib.rs`, `crates/pop3/src/op/delete.rs`, `crates/pop3/src/protocol/response.rs` — POP3 greeting / BYE / IMPLEMENTATION
- `crates/smtp/src/inbound/data.rs` — `Received:` header comment
- `crates/http/src/api/mod.rs` — `WWW-Authenticate` realm label
- `crates/jmap-proto/src/request/capability.rs` — JMAP-for-Sieve `implementation` string
- `tests/src/jmap/principal/get.rs` — coupled test fixture for the above
- `crates/main/Cargo.toml`, `crates/smtp/Cargo.toml` — package `description`
- `crates/common/src/manager/defaults.rs` — admin app display description

## 3. Branding changes completed

- **Product naming** → NuwaMail / NuwaMail Server / NuwaMail Admin across README,
  package descriptions, OCI labels, and protocol banners.
- **Protocol banners** (IMAP, POP3, ManageSieve greetings/farewells, SMTP
  `Received` comment, POP3/ManageSieve `IMPLEMENTATION`, IMAP `ID`, JMAP-Sieve
  `implementation`, `WWW-Authenticate` realm) → NuwaMail. All standards-compliant
  human-readable strings; no version-string spoofing introduced.
- **User-agent** (`USER_AGENT`, plus a second hardcoded copy) → `NuwaMail/1.0.0`.
- **Daemon name** (startup/milter/hook label) → `NuwaMail v{version}`.
- **Logo** → NuwaMail text placeholder; upstream logo preserved.

## 4. Legal / attribution files preserved or added

- **Preserved verbatim:** `LICENSES/AGPL-3.0-only.txt`, `LICENSES/LicenseRef-SEL.txt`,
  all 1,232 per-file SPDX `SPDX-FileCopyrightText: 2020 Stalwart Labs LLC` /
  `SPDX-License-Identifier` headers, the README `Copyright (C) 2020, Stalwart Labs LLC`
  line, `SECURITY*.md`, `CONTRIBUTING.md`.
- **Added:** `FORK_NOTICE.md` (states NuwaMail is a fork derived from Stalwart,
  names the upstream project/author, links the repo, preserves the AGPL/SEL
  license note, and explicitly disclaims endorsement by Stalwart Labs).
- **No false endorsement / no removed attribution.** No license headers removed
  or relicensed.

## 5. Deployment names changed

| Aspect | Value |
| --- | --- |
| Docker image | `ghcr.io/finalverse/nuwamail` |
| Container name | `nuwamail` |
| Compose service | `nuwamail` (`docker-compose.example.yml`) |
| Command alias | `nuwamail-server` (symlink → `stalwart` binary) |
| OCI labels | title/vendor/source → NuwaMail / Finalverse |
| Example MX host | `mail.nuwamail.com` |
| Example admin host | `https://admin.nuwamail.com` |
| Example JMAP URL | `https://mail.nuwamail.com/jmap` |

**Preserved for compatibility:** Rust binary/package `stalwart`, container paths
`/etc/stalwart` & `/var/lib/stalwart`, OS user/group `stalwart`, systemd unit
`stalwart-mail.service`, `STALWART_*` env vars, `install.sh` upstream download
URL. CI Docker/registry naming is repository-dynamic (`${{github.repository}}`)
and therefore already resolves to `finalverse/nuwamail` with no edits.

## 6. UI / docs changes

- `README.md` fully rebranded: NuwaMail positioning paragraph, NuwaMail Cloud
  roadmap table, build/Docker/compose quick-starts with `mail.nuwamail.com` /
  `admin.nuwamail.com` examples, "Derived from Stalwart Mail Server" attribution
  section (upstream link + baseline version + license note), preserved License &
  Copyright sections, and an "Intentionally preserved names" section.
- Admin app display label → "NuwaMail Admin".
- No web-UI source exists in this repository — the admin UI (`webui.zip`) is a
  separate component downloaded at runtime (URL preserved; see §7). UI string
  rebranding beyond server-provided labels is therefore out of scope here.

## 7. Internal identifiers intentionally preserved

| Identifier | Location | Why preserved |
| --- | --- | --- |
| Binary/package `stalwart` | `crates/main`, Dockerfile, CI, systemd, install.sh | Renaming breaks build `-p`, artifacts, packaging; alias `nuwamail-server` added instead |
| `PROD_ID` (iCal/vCard) | `crates/common/src/lib.rs` | Embedded in generated objects; asserted by `tests/resources/itip/*` |
| `Capability::Stalwart` → `urn:stalwart:jmap` | `crates/jmap-proto` | Machine-readable JMAP capability URI; interop |
| `urn:stalwart:davsync:` / `:davlock:` | `crates/dav/src/common/uri.rs` | Client-persisted DAV sync/lock tokens; interop |
| `STALWART_*` env vars | `crates/store/src/registry/local.rs` etc. | Runtime/ops configuration contract |
| Config/data/log paths `/etc/stalwart`, `/var/lib/stalwart`, `/var/log/stalwart`, `stalwart.log` | schema defaults, Dockerfile | Existing-deployment & migration compatibility |
| WebUI download URL `stalwartlabs/webui` | `crates/common/src/manager/defaults.rs` | NuwaMail has no webui fork; changing breaks admin UI |
| `"Stalwart Address Book"` default | `crates/registry/src/schema/structs_impl.rs` (**auto-generated**) | Generated code + provisions user data + test-coupled; rebrand at schema source later |
| OTel `service.name = "stalwart"` | `crates/common/src/telemetry/tracers/otel.rs` | Observability pipeline identifier |
| DB schema keys / `DATABASE_SCHEMA_VERSION` | registry/store | Schema stability; migration safety |
| SPDX headers, `LICENSES/` | repo-wide | Legal requirement |
| `CHANGELOG.md`, `UPGRADING/*.md`, `install.sh` | repo-root | Historical accuracy / upstream binary download |

## 8. Remaining "Stalwart" references and reasons

Repository-wide count (case-insensitive, excluding `.git/`): **2,227**
(higher than the original 2,196 because the new attribution files intentionally
name "Stalwart"). Breakdown of what remains and why:

| Category | ~Count | Disposition |
| --- | --- | --- |
| SPDX / license headers | 1,232 | **Preserve** — legal attribution |
| `LICENSES/` text | 7 | **Preserve** — license text verbatim |
| `CHANGELOG.md` + `UPGRADING/*.md` | 317 | **Preserve** — historical upstream records |
| `tests/` fixtures + harness | 258 | **Preserve** — fixture data, PRODID, container/realm names |
| `install.sh` | 79 | **Preserve** — downloads upstream binary; paths/service |
| `crates/` non-header residual | ~120 | **Preserve** — internal identifiers per §7 |
| New NuwaMail docs (FORK_NOTICE/PATCHES/README/REPORT) | small | **Intentional** — nominative attribution to upstream |

No user-facing product banner or display string asserted as safe was left as
"Stalwart"; the only deliberately preserved user-facing residual is the
auto-generated default "Stalwart Address Book" (§7).

## 9. Validation commands run

1. `git status` — clean working tree at fork time; all edits tracked.
2. Repo-wide `grep -rin "stalwart"` audit — categorized in §8.
3. `rustfmt --edition 2024 --check <all edited .rs files>` — **PASS** (exit 0,
   no diffs) on all 15 edited Rust files (including the coupled test).
4. `cargo check --workspace` (clean checkout; deps downloaded + compiled) — see
   §10.

> Note: a clean checkout had no `target/` directory, so the build downloaded and
> compiled the full dependency graph (RocksDB, aws-lc-rs, sequoia-openpgp, etc.).

## 10. Test / build results

- **rustfmt:** PASS — all edited files are correctly formatted; no syntax issues
  introduced by the rebrand.
- **cargo check --workspace:** **PASS** — `Finished dev profile … in 5m 54s`,
  exit code `0`, **no errors and no new warnings**. The full dependency graph was
  downloaded and compiled from a clean checkout. Every edited workspace crate
  type-checked cleanly, confirmed in the build log:
  `utils`, `smtp`, `imap`, `imap-proto`, `managesieve`, `pop3`, `jmap`,
  `jmap-proto`, `http`, `common`, and the `stalwart` main binary.
- **cargo test --workspace:** Not run as a full suite (the upstream suite
  requires external services — LDAP, PostgreSQL, MinIO/S3, Redis, PowerDNS,
  Keycloak — provisioned via `tests/docker/`, not available in this environment).
  The two test fixtures touched by the rebrand were updated in lockstep with
  their source (`Principal/get` `implementation` string) so the corresponding
  assertions remain green; no other assertions reference rebranded strings
  (verified by grep).

## 11. Known risks

- **Low overall.** All source edits are string/label-only; no control flow,
  protocol encoding, schema, or auth logic changed.
- **README merge conflicts** with future upstream are likely (file heavily
  edited) — expected and acceptable for a fork.
- **Outbound user-agent** is now `NuwaMail/1.0.0`; any third party that
  allow-listed `Stalwart/1.0.0` would need updating (none known).
- **`install.sh` still fetches upstream Stalwart release binaries** — it does not
  build/ship a NuwaMail binary. It is kept for reference; a NuwaMail release
  pipeline is a follow-up (see §12). Documented in README's "Get Started".
- Auto-generated `structs_impl.rs` still exposes "Stalwart Address Book" as a
  default display name (§7).

## 12. Recommended next steps

1. **Release pipeline:** point `install.sh` `BASE_URL` and any download tooling
   at NuwaMail releases once CI publishes `ghcr.io/finalverse/nuwamail` and
   binary artifacts (CI is already repository-dynamic and will tag correctly).
2. **Schema-source rebrand:** rebrand "Stalwart Address Book" and any other
   generated user-facing defaults at the schema/generator source, regenerate
   `structs_impl.rs`, and update `tests/src/jmap/contacts/addressbook.rs`.
3. **Optional telemetry rebrand:** decide whether to rename OTel `service.name`
   to `nuwamail` (operator-facing) as a deliberate, dashboard-coordinated change.
4. **WebUI fork:** when a NuwaMail Admin UI exists, fork `stalwartlabs/webui`,
   publish `nuwamail` webui artifacts, and update the `resource_url` default.
5. **Full test run:** execute `cargo test --workspace` against the
   `tests/docker/` service stack in CI to confirm no regressions.
6. **Do not mark production-ready** until steps 1 and 5 pass in CI.
