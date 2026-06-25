<p align="center">
    <a href="https://github.com/finalverse/nuwamail">
    <img src="./img/nuwamail-logo.svg" height="150">
    </a>
</p>

<h3 align="center">
  NuwaMail — secure, scalable mail &amp; collaboration server with comprehensive protocol support 🛡️ <br/>(IMAP, JMAP, SMTP, CalDAV, CardDAV, WebDAV)
</h3>

<br>

<p align="center">
  <a href="https://github.com/finalverse/nuwamail/actions/workflows/ci.yml"><img src="https://img.shields.io/github/actions/workflow/status/finalverse/nuwamail/ci.yml?style=flat-square" alt="continuous integration"></a>
  &nbsp;
  <a href="https://www.gnu.org/licenses/agpl-3.0"><img src="https://img.shields.io/badge/License-AGPL_v3-blue.svg?label=license&style=flat-square" alt="License: AGPL v3"></a>
</p>

## What is NuwaMail?

**NuwaMail** is a modern, Rust-based mail and collaboration server baseline for
**NuwaMail Cloud**, designed for enterprise email hosting, MSP/reseller
workflows, AI-native inbox features, secure communication, and sovereign
deployment.

NuwaMail Server provides JMAP, IMAP4, POP3, SMTP, CalDAV, CardDAV and WebDAV
support together with a wide range of modern features. It is written in Rust and
designed to be secure, fast, robust and scalable.

> **NuwaMail is derived from [Stalwart Mail Server](https://github.com/stalwartlabs/stalwart).**
> NuwaMail is an independent fork that rebrands and hardens the upstream Stalwart
> baseline; it is **not** an official Stalwart product and is **not** endorsed by
> Stalwart Labs LLC. See [`FORK_NOTICE.md`](./FORK_NOTICE.md) for full attribution
> and licensing details.

## Features

Key features:

- **Email** server with complete protocol support:
  - JMAP:
    * [JMAP for Mail](https://datatracker.ietf.org/doc/html/rfc8621) server.
    * [JMAP for Sieve Scripts](https://www.ietf.org/archive/id/draft-ietf-jmap-sieve-22.html).
    * [WebSocket](https://datatracker.ietf.org/doc/html/rfc8887), [Blob Management](https://www.rfc-editor.org/rfc/rfc9404.html) and [Quotas](https://www.rfc-editor.org/rfc/rfc9425.html) extensions.
  - IMAP:
    * [IMAP4rev2](https://datatracker.ietf.org/doc/html/rfc9051) and [IMAP4rev1](https://datatracker.ietf.org/doc/html/rfc3501) server.
    * [ManageSieve](https://datatracker.ietf.org/doc/html/rfc5804) server.
    * Numerous extensions supported.
  - POP3:
    - [POP3](https://datatracker.ietf.org/doc/html/rfc1939) server.
    - [STLS](https://datatracker.ietf.org/doc/html/rfc2595) and [SASL](https://datatracker.ietf.org/doc/html/rfc5034) support as well as other [extensions](https://datatracker.ietf.org/doc/html/rfc2449).
  - SMTP:
    * SMTP server with built-in [DMARC](https://datatracker.ietf.org/doc/html/rfc7489), [DKIM](https://datatracker.ietf.org/doc/html/rfc6376), [SPF](https://datatracker.ietf.org/doc/html/rfc7208) and [ARC](https://datatracker.ietf.org/doc/html/rfc8617) support for message authentication.
    * Strong transport security through [DANE](https://datatracker.ietf.org/doc/html/rfc6698), [MTA-STS](https://datatracker.ietf.org/doc/html/rfc8461) and [SMTP TLS](https://datatracker.ietf.org/doc/html/rfc8460) reporting.
    * Automated DKIM key rotation and management.
    * Inbound throttling and filtering with granular configuration rules, sieve scripting, MTA hooks and milter integration.
    * Distributed virtual queues with delayed delivery, priority delivery, quotas, routing rules and throttling support.
    * Envelope rewriting and message modification.
- **Collaboration** server:
  - Calendaring and scheduling:
    - [CalDAV](https://datatracker.ietf.org/doc/html/rfc4791) and [CalDAV Scheduling](https://datatracker.ietf.org/doc/html/rfc6638) support.
    - [JMAP for Calendars](https://datatracker.ietf.org/doc/html/draft-ietf-jmap-calendars-24) support.
  - Contact management:
    - [CardDAV](https://datatracker.ietf.org/doc/html/rfc6352) support.
    - [JMAP for Contacts](https://datatracker.ietf.org/doc/html/rfc9610) support.
  - File storage:
    - [WebDAV](https://datatracker.ietf.org/doc/html/rfc4918) support.
    - [JMAP for File Storage](https://datatracker.ietf.org/doc/html/draft-ietf-jmap-filenode-03) support.
  - Sharing with fine-grained access controls:
    - [WebDAV ACL](https://datatracker.ietf.org/doc/html/rfc3744) support.
    - [JMAP Sharing](https://datatracker.ietf.org/doc/html/rfc9670) support.
- **Spam** and **Phishing** built-in filter:
  - Comprehensive set of filtering **rules** on par with popular solutions.
  - LLM-driven spam filtering and message analysis.
  - Statistical **spam classifier** with collaborative filtering, automatic training capabilities and address book integration.
  - DNS Blocklists (**DNSBLs**) checking of IP addresses, domains, and hashes.
  - Collaborative digest-based spam filtering with **Pyzor**.
  - **Phishing** protection against homographic URL attacks, sender spoofing and other techniques.
  - Trusted **reply** tracking to recognize and prioritize genuine e-mail replies.
  - Sender **reputation** monitoring by IP address, ASN, domain and email address.
  - **Greylisting** to temporarily defer unknown senders.
  - **Spam traps** to set up decoy email addresses that catch and analyze spam.
- **Flexible**:
  - Pluggable storage backends with **RocksDB**, **FoundationDB**, **PostgreSQL**, **mySQL**, **SQLite**, **S3-Compatible**, **Azure** and **Redis** support.
  - Full-text search available in 17 languages using the built-in search engine or via **Meilisearch**, **ElasticSearch**, **OpenSearch**, **PostgreSQL** or **mySQL** backends.
  - Sieve scripting language with support for all [registered extensions](https://www.iana.org/assignments/sieve-extensions/sieve-extensions.xhtml).
  - Email aliases, mailing lists, subaddressing and catch-all addresses support.
  - Automated DNS management.
  - Automatic account configuration and discovery with [autoconfig](https://www.ietf.org/id/draft-bucksch-autoconfig-02.html) and [autodiscover](https://learn.microsoft.com/en-us/exchange/architecture/client-access/autodiscover?view=exchserver-2019).
  - Multi-tenancy support with domain and tenant isolation.
  - Disk quotas per user and tenant.
- **Secure and robust**:
  - Encryption at rest with **S/MIME** or **OpenPGP**.
  - Automatic TLS certificate provisioning with [ACME](https://datatracker.ietf.org/doc/html/rfc8555) using `TLS-ALPN-01`, `DNS-01`, `DNS-PERSIST-01` or `HTTP-01` challenges.
  - Automated blocking of IP addresses that attack, abuse or scan the server for exploits.
  - Rate limiting.
  - Memory safe (thanks to Rust).
- **Scalable and fault-tolerant**:
  - Designed to handle growth seamlessly, from small setups to large-scale deployments of thousands of nodes.
  - Built with **fault tolerance** and **high availability** in mind, recovers from hardware or software failures with minimal operational impact.
  - Peer-to-peer cluster coordination or with **Kafka**, **Redpanda**, **NATS** or **Redis**.
  - **Kubernetes**, **Apache Mesos** and **Docker Swarm** support for automated scaling and container orchestration.
  - Read replicas, sharded blob storage and in-memory data stores for high performance and low latency.
- **Authentication and Authorization**:
  - **OpenID Connect** authentication.
  - OAuth 2.0 authorization with [authorization code](https://www.rfc-editor.org/rfc/rfc8628) and [device authorization](https://www.rfc-editor.org/rfc/rfc8628) flows.
  - **LDAP**, **OIDC**, **SQL** or built-in authentication backend support.
  - Two-factor authentication with Time-based One-Time Passwords (`2FA-TOTP`)
  - Application passwords (App Passwords).
  - Roles and permissions.
  - Access Control Lists (ACLs).
- **Observability**:
  - Logging and tracing with **OpenTelemetry**, journald, log files and console support.
  - Metrics with **OpenTelemetry** and **Prometheus** integration.
  - Webhooks for event-driven automation.
  - Alerts with email and webhook notifications.
  - Live tracing and metrics.
- **Web-based administration** (NuwaMail Admin):
  - Dashboard with real-time statistics and monitoring.
  - Account, domain, group and mailing list management.
  - SMTP queue management for messages and outbound DMARC and TLS reports.
  - Report visualization interface for received DMARC, TLS-RPT and Failure (ARF) reports.
  - Configuration of every aspect of the mail server.
  - Log viewer with search and filtering capabilities.
  - Self-service portal for password reset and encryption-at-rest key management.

## NuwaMail Cloud roadmap

This repository is the **baseline mail engine** for NuwaMail Cloud. The
rebranding establishes naming, documentation, and deployment conventions so that
the following layers can be added later **as separate work** (they are **not**
implemented in this repository yet):

| Product | Purpose |
| --- | --- |
| **NuwaMail Cloud** | Hosted, multi-tenant enterprise email & collaboration |
| **NuwaMail Admin** | Web administration console |
| **NuwaMail AI** | AI-native inbox assistant and message analysis |
| **NuwaMail Shield** | AI security / phishing explanation |
| **NuwaMail Archive** | Compliance archive and eDiscovery |
| **NuwaMail Sovereign** | Private / air-gapped sovereign deployment |

## Get Started

> ⚠️ **Baseline status:** this is a rebranded baseline. There is no NuwaMail
> release/download pipeline yet. To run the server today, build it from source
> (see below) or use the Docker image you build locally. The bundled
> [`install.sh`](./install.sh) still downloads upstream **Stalwart** binaries
> and is kept for reference until a NuwaMail release pipeline exists — see
> [`REBRAND_REPORT.md`](./REBRAND_REPORT.md).

### Build from source

```sh
cargo build --release -p stalwart \
  --no-default-features --features "sqlite postgres mysql rocks s3 redis enterprise"
```

> The Rust binary/package is still named `stalwart` internally for upstream
> mergeability and config-path compatibility. See
> [Intentionally preserved names](#intentionally-preserved-names) below.

### Docker

```sh
docker build -t ghcr.io/finalverse/nuwamail:latest .
docker run -d --name nuwamail \
  -p 25:25 -p 143:143 -p 993:993 -p 587:587 -p 465:465 \
  -p 110:110 -p 995:995 -p 4190:4190 -p 443:443 -p 8080:8080 \
  -v nuwamail-etc:/etc/stalwart -v nuwamail-data:/var/lib/stalwart \
  ghcr.io/finalverse/nuwamail:latest
```

Example deployment hostnames:

- Mail / MX server: `mail.nuwamail.com`
- Admin console: `https://admin.nuwamail.com`
- JMAP endpoint: `https://mail.nuwamail.com/jmap`

## Documentation

The protocol behavior, configuration model and administration of the underlying
engine are documented in the upstream **Stalwart** documentation at
[stalw.art/docs](https://stalw.art/docs/install/get-started). NuwaMail-specific
documentation will live in this repository as the Cloud layers are added.

## Derived from Stalwart Mail Server

NuwaMail is a fork/baseline derived from **Stalwart Mail Server**, an
open-source Rust mail and collaboration server by **Stalwart Labs LLC**.

- Upstream project: [github.com/stalwartlabs/stalwart](https://github.com/stalwartlabs/stalwart)
- Upstream website: [stalw.art](https://stalw.art)
- Baseline version: `0.16.11`

Commercial support, priority support, and Enterprise (SELv2) licensing for the
**upstream Stalwart project** are offered directly by **Stalwart Labs LLC** at
[stalw.art/enterprise](https://stalw.art/enterprise). NuwaMail does not resell or
provide Stalwart commercial support.

Part of the development of the **upstream** project was funded through the
[NGI0 Entrust Fund](https://nlnet.nl/entrust) and
[NGI Zero Core](https://nlnet.nl/NGI0/), funds established by
[NLnet](https://nlnet.nl/) with financial support from the European Commission's
[Next Generation Internet](https://ngi.eu/) programme.

## License

This project preserves the upstream dual-licensing **unchanged**, under the
**GNU Affero General Public License v3.0** (AGPL-3.0; as published by the Free
Software Foundation) and the **Stalwart Enterprise License v2 (SELv2)**:

- The [GNU Affero General Public License v3.0](./LICENSES/AGPL-3.0-only.txt) is a
  free software license that ensures your freedom to use, modify, and distribute
  the software, with the condition that any modified versions of the software
  must also be distributed under the same license.
- The [Stalwart Enterprise License v2 (SELv2)](./LICENSES/LicenseRef-SEL.txt) is
  a proprietary license held by Stalwart Labs LLC, designed for commercial use.
  It governs the `enterprise`-gated features. Forking does not grant additional
  rights under SELv2.

Each file in this project contains a license notice at the top, indicating the
applicable license(s). The license notice follows the
[REUSE guidelines](https://reuse.software/). The full text of each license is
available in the [LICENSES](./LICENSES/) directory.

## Copyright

Copyright (C) 2020, Stalwart Labs LLC (upstream Stalwart Mail Server).

NuwaMail fork modifications (C) 2025, Finalverse / NuwaMail.

## Intentionally preserved names

To keep this fork mergeable with upstream and to avoid breaking deployments,
some internal "Stalwart"/"stalwart" identifiers are **intentionally preserved**:
the Rust binary/package name `stalwart`, config paths (`/etc/stalwart`,
`/var/lib/stalwart`), the systemd unit `stalwart-mail.service`, `STALWART_*`
environment variables, the iCalendar/vCard `PRODID`, database schema keys, and
all SPDX license headers. See [`REBRAND_REPORT.md`](./REBRAND_REPORT.md) for the
complete list and rationale.
