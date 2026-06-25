# FORK_NOTICE

## About this fork

**NuwaMail Server** is a fork/baseline derived from the **Stalwart Mail Server**.

NuwaMail re-uses the upstream Stalwart codebase as the mail and collaboration
engine for **NuwaMail Cloud**, an AI-native enterprise email and collaboration
service. This repository performs a **rebranding and baseline-hardening** of the
upstream project. It is **not** a re-implementation and does not alter the core
mail-server protocol behavior (SMTP, IMAP, JMAP, POP3, CalDAV, CardDAV, WebDAV,
storage, authentication, queueing, indexing).

## Upstream project

- **Original project:** Stalwart Mail Server
- **Original author / copyright holder:** Stalwart Labs LLC
- **Upstream repository:** https://github.com/stalwartlabs/stalwart
- **Upstream website:** https://stalw.art
- **Baseline version at fork time:** `0.16.11`

All original copyright remains with **Stalwart Labs LLC**:

> Copyright (C) 2020, Stalwart Labs LLC

## NuwaMail modifications

- **Maintained by:** Finalverse / NuwaMail (https://github.com/finalverse/nuwamail)
- **Scope of NuwaMail changes:** user-facing product naming, documentation,
  deployment/Docker naming, example configuration domains, and cosmetic
  user-facing banner strings.
- See [`PATCHES.md`](./PATCHES.md) for a per-file record of every non-trivial
  change and [`REBRAND_REPORT.md`](./REBRAND_REPORT.md) for the validation
  report.

## License

This fork preserves the upstream dual-licensing **unchanged**:

- **GNU Affero General Public License v3.0** (`AGPL-3.0-only`) — see
  [`LICENSES/AGPL-3.0-only.txt`](./LICENSES/AGPL-3.0-only.txt).
- **Stalwart Enterprise License v2 (SELv2)** (`LicenseRef-SEL`) — see
  [`LICENSES/LicenseRef-SEL.txt`](./LICENSES/LicenseRef-SEL.txt).

Per-file SPDX license headers and the `LICENSES/` directory are preserved
verbatim. NuwaMail does **not** remove, alter, or relicense any upstream
copyright notice or license header.

> **Note on the Stalwart Enterprise License (SELv2):** the `enterprise`-gated
> features in this repository remain governed by the upstream SELv2 terms held
> by Stalwart Labs LLC. Forking does not grant NuwaMail or its users any
> additional rights under SELv2. Review `LICENSES/LicenseRef-SEL.txt` before
> enabling or distributing those features commercially.

## No endorsement

NuwaMail is an **independent fork**. NuwaMail is **not** an official Stalwart
product and is **not** endorsed by, sponsored by, or affiliated with Stalwart
Labs LLC. References to "Stalwart" in this repository exist solely to provide
accurate attribution to the upstream project as required by its license.

## Trademark notice

"Stalwart" may be a trademark of Stalwart Labs LLC. "NuwaMail" and "Finalverse"
are used by their respective owners. This fork uses the "Stalwart" name only
nominatively, for attribution and to describe the origin of the code.
