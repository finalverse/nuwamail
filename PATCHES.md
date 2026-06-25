# PATCHES — NuwaMail rebrand of Stalwart Mail Server

This file documents every **non-trivial** change made to fork the upstream
**Stalwart Mail Server** into the **NuwaMail** baseline. The goal was a
production-safe rebrand: no protocol, storage, auth, schema, or migration
behavior was changed. See [`FORK_NOTICE.md`](./FORK_NOTICE.md) for attribution
and [`REBRAND_REPORT.md`](./REBRAND_REPORT.md) for validation.

**Baseline:** upstream Stalwart `0.16.11` (fork commit `9525a891`).

**Risk legend:** 🟢 cosmetic/string-only · 🟡 deployment-facing · 🔴 behavioral
(none in this patch set).

**Merge-conflict likelihood:** how likely a future `git merge` of upstream is to
conflict on this line.

---

## 1. New files (additive — no upstream conflict)

| File | Reason | Risk | Merge conflict |
| --- | --- | --- | --- |
| `FORK_NOTICE.md` | Required fork/upstream attribution & license note. | 🟢 | None (new file) |
| `PATCHES.md` | This change log. | 🟢 | None |
| `REBRAND_REPORT.md` | Validation & rebrand report. | 🟢 | None |
| `img/nuwamail-logo.svg` | Text placeholder logo; upstream `img/logo-red.svg` preserved untouched. | 🟢 | None |
| `docker-compose.example.yml` | Deployment example, service `nuwamail`, image `ghcr.io/finalverse/nuwamail`. Not used by build/tests. | 🟡 | None |

## 2. Documentation

| File | Change | Risk | Merge conflict |
| --- | --- | --- | --- |
| `README.md` | Full product rebrand to NuwaMail; added positioning paragraph, Cloud roadmap, "Derived from Stalwart" attribution section, preserved License section and `Copyright (C) 2020 Stalwart Labs LLC`, added NuwaMail fork copyright. Removed Stalwart-owned social/sponsorship CTAs (not NuwaMail's to solicit); kept upstream commercial-support pointer as attribution. | 🟢 | Likely (README heavily edited) |

## 3. Deployment / Docker

| File | Change | Risk | Merge conflict |
| --- | --- | --- | --- |
| `Dockerfile` | Added OCI image labels (`org.opencontainers.image.*` → NuwaMail/Finalverse). Added `nuwamail-server` symlink → `/usr/local/bin/stalwart`. **Binary name, paths `/etc/stalwart` & `/var/lib/stalwart`, user/group `stalwart`, and `STALWART_HEALTHCHECK_URL` preserved** for compatibility. | 🟡 | Low (added lines) |

## 4. Source — user-facing banner strings (string-only, not asserted by tests)

All of the following are human-readable greeting/identification strings sent to
clients. None are parsed by clients for behavior and **none are asserted by the
test suite** (verified by grep before editing).

| File | Change | Risk | Merge conflict |
| --- | --- | --- | --- |
| `crates/common/src/lib.rs` | `USER_AGENT` `Stalwart/1.0.0` → `NuwaMail/1.0.0`; `DAEMON_NAME` `Stalwart v…` → `NuwaMail v…`. `PROD_ID` **preserved** (see §6) with explanatory comment. | 🟢 | Low |
| `crates/utils/src/http.rs` | Second hardcoded outbound user-agent `Stalwart/1.0.0` → `NuwaMail/1.0.0`. | 🟢 | Low |
| `crates/imap/src/lib.rs` | IMAP greeting → `NuwaMail IMAP4rev2 at your service.` | 🟢 | Low |
| `crates/imap/src/op/logout.rs` | IMAP BYE → `NuwaMail IMAP4rev2 bids you farewell.` | 🟢 | Low |
| `crates/imap/src/op/capability.rs` | IMAP `ID` response: name `Stalwart`→`NuwaMail`, vendor `Stalwart Labs LLC`→`Finalverse`, support-url → fork repo. | 🟢 | Low |
| `crates/managesieve/src/lib.rs` | ManageSieve greeting → `NuwaMail ManageSieve at your service.` | 🟢 | Low |
| `crates/managesieve/src/op/capability.rs` | `IMPLEMENTATION` → `NuwaMail ManageSieve`. | 🟢 | Low |
| `crates/managesieve/src/op/logout.rs` | ManageSieve farewell → NuwaMail. | 🟢 | Low |
| `crates/pop3/src/lib.rs` | POP3 greeting → `+OK NuwaMail POP3 at your service.` | 🟢 | Low |
| `crates/pop3/src/op/delete.rs` | 3× POP3 farewell strings → NuwaMail. | 🟢 | Low |
| `crates/pop3/src/protocol/response.rs` | 2× POP3 `CAPA`/`IMPLEMENTATION` → `NuwaMail Server`. | 🟢 | Low |
| `crates/smtp/src/inbound/data.rs` | `Received:` header comment `(Stalwart SMTP)` → `(NuwaMail SMTP)`. Standards-compliant comment token; tests only assert presence of `Received: `. | 🟢 | Low |
| `crates/http/src/api/mod.rs` | `WWW-Authenticate` realm label `Stalwart Server` → `NuwaMail Server` (Bearer + Basic). Realm is a display label, not a protocol key. | 🟢 | Low |
| `crates/jmap-proto/src/request/capability.rs` | JMAP-for-Sieve advertised `implementation` `Stalwart v1.0.0` → `NuwaMail v1.0.0`. Coupled test `tests/src/jmap/principal/get.rs` updated to match. | 🟢 | Low |
| `tests/src/jmap/principal/get.rs` | Expected `implementation` string updated to `NuwaMail v1.0.0` (keeps `Principal/get` test green). | 🟢 | Low |
| `crates/common/src/manager/defaults.rs` | Admin app display description `Stalwart Web Interface` → `NuwaMail Admin`. **Download URL & log paths preserved** (see §6). | 🟢 | Low |

## 5. Source — package metadata

| File | Change | Risk | Merge conflict |
| --- | --- | --- | --- |
| `crates/main/Cargo.toml` | `description` → `NuwaMail Mail & Collaboration Server (derived from Stalwart)`. Package/binary `name = "stalwart"`, `authors`, `repository`, `homepage` **preserved** (authorship attribution + not republished). | 🟢 | Low |
| `crates/smtp/Cargo.toml` | `description` → `NuwaMail SMTP Server (derived from Stalwart)`. | 🟢 | Low |

## 6. Intentionally NOT changed (preserved) — see REBRAND_REPORT §7–8

- **SPDX headers & `LICENSES/`** — preserved verbatim (legal requirement).
- **Rust binary/package name `stalwart`** — renaming breaks `cargo build -p`,
  CI artifacts, install.sh, systemd, Docker ENTRYPOINT. `nuwamail-server`
  provided as a symlink alias instead.
- **`PROD_ID = "-//Stalwart Labs LLC//Stalwart Server//EN"`** — embedded in
  generated iCalendar/vCard objects and asserted by `tests/resources/itip/*`
  fixtures; a stable product identifier.
- **`Capability::Stalwart → "urn:stalwart:jmap"`** (`crates/jmap-proto`) — a
  machine-readable JMAP capability URI; changing it breaks interop.
- **WebUI download URL** `github.com/stalwartlabs/webui` in
  `crates/common/src/manager/defaults.rs` — NuwaMail has no webui fork yet;
  changing it would break the admin UI download.
- **Config/runtime paths** `/etc/stalwart`, `/var/lib/stalwart`,
  `/var/log/stalwart`, log prefix `stalwart.log`, OS user/group `stalwart`,
  `STALWART_*` env vars — changing affects existing/migrating deployments.
- **`resources/systemd/stalwart-mail.service`**, **`install.sh`** — tied to the
  preserved binary name and the upstream release download URL; kept functional.
- **`CHANGELOG.md`, `UPGRADING/*.md`** — historical upstream records; rewriting
  them would falsify history and mislead operators following past upgrade steps.
- **`crates/jmap/src/api/session.rs` `Capability::Stalwart`** match arm,
  schema/registry keys, DB schema version — internal identifiers.
- **WebDAV URN schemes** `urn:stalwart:davsync:`, `urn:stalwart:davlock:`
  (`crates/dav/src/common/uri.rs`) — embedded in sync-tokens/lock-tokens that
  clients persist and replay; changing breaks DAV state and interop.
- **`default_display_name: "Stalwart Address Book"`** in
  `crates/registry/src/schema/structs_impl.rs` — this file is **auto-generated
  ("Do not edit directly")** and the value provisions end-user data (asserted in
  `tests/src/jmap/contacts/addressbook.rs`). Known user-facing residual: rebrand
  at the schema-source/generator level in a follow-up, then regenerate and
  update fixtures. Not edited here to preserve generated-code integrity.
- **OpenTelemetry `service.name = "stalwart"`** (`crates/common/src/telemetry/
  tracers/otel.rs`) — operator-facing observability identifier; preserved to
  avoid breaking telemetry pipelines. Candidate for a future, deliberate rename.
