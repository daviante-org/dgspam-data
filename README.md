<div align="left">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./assets/logo-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="./assets/logo.svg">
    <img alt="dgspamguard-data" src="./assets/logo.svg" width="336" height="56">
  </picture>
</div>

<br>

**4-Hour Rolling Domain, URL, SMTP IP, Email Taxonomy, Malware Hash, Protective DNS RPZ & Mail Rule Intelligence Releases**

`dgspamguard-data` is the public rolling threat & mail dataset repository maintained by [Daviante](https://daviante.org). Because active phishing campaigns, malicious URLs, hijacked SMTP netblocks, and malware payload hashes rotate within hours rather than weeks, `dgspamguard-data` publishes **4-hour rolling snapshots** with **7-day rolling retention** across **5 open security intelligence layers**:
1. **Canonical Domain Boundaries & Allowlist Gates**: Mozilla Public Suffix List (ICANN vs Private multi-tenant boundaries) and Tranco Top 1M popularity shields.
2. **Corroborated Phishing, Malware & Botnet C2 Domains/URLs**: Verified malicious domains and exact canonical URLs (`SHA-256`) compiled from national CSIRTs and abuse trackers, with multi-tenant shared-hosting isolation (`pages.dev`, `github.io`, `docs.google.com`, `dropbox.com`).
3. **Malicious Sending IPs, Botnets & Hijacked Subnets (SMTP Connection Layer)**: IPv4/IPv6 CIDR netblocks and active botnet/SMTP attacker IPs (`Spamhaus DROP`, `IPsum Level 3+`, `Blocklist.de`, `Feodo Tracker`, `Phishing.Database`).
4. **Email Provider Taxonomy & TLD Risk**: Consolidated MX-validated disposable/burner inbox domains, consumer free mailbox providers, and high-abuse TLD risk multipliers.
5. **Malware Attachment Hashes, Protective DNS RPZ & DGMail Rules**: Cryptographic `SHA-256` malware attachment hashes, standard **DNS Response Policy Zones (`.rpz.gz`)**, and self-hosted **DGMail** spam-classifier rules (`dgmail-rules.json.gz`).

Application source code for the self-hosted compiler and lookup API lives in [`daviante-org/dgspamguard`](https://github.com/daviante-org/dgspamguard).

---

## Versioning, 4-Hour Cadence & 7-Day Rolling Retention

- **Version Tag**: `vYYYYMMDD.HHMM` compact UTC timestamp tags tailored to 4-hour rolling datasets (for example `v20261103.0000`, `v20261103.0400`, `v20261103.1600`, with monotonic intra-minute fallback `v20261103.1600.1` if manually rebuilt within the same UTC minute).
- **4-Hour Rolling Schedule**: Evaluated every **4 hours (`15 */4 * * *` UTC — 6× daily)** right after `dgspamguard`'s `00:00`, `04:00`, `08:00`, `12:00`, `16:00`, and `20:00` UTC snapshot boundaries via [`.github/workflows/rolling-release.yml`](./.github/workflows/rolling-release.yml) against `https://dgspamguard.daviante.org` and published by `mr-vegapunk[bot]` whenever upstream threat feeds have changed.
- **7-Day Rolling Window**: Releases and git tags older than **7 days** are automatically pruned (retaining at most `~42` active snapshots while always keeping the `latest` release intact) so repository storage stays compact and historical stale indicators age out cleanly.

---

## Published Formats in Every Release

Full schema details are documented in [`docs/FORMAT_SPEC.md`](./docs/FORMAT_SPEC.md).

| Artifact | Standard | Primary Use Case |
| :--- | :--- | :--- |
| `bundle.tar.gz` | PAX `tar.gz` | Complete snapshot bundle (`manifest.json`, `SHA256SUMS.txt` + all 9 `.gz` files) for `dgspamguard` cold-start bootstrap. |
| `dgspamguard-threat-domains.rpz.gz` | DNS RPZ Zone | Protective DNS Response Policy Zone (`CNAME .` NXDOMAIN) for `daviante-doh`, Unbound, Knot, PowerDNS, BIND 9. |
| `dgspamguard-threat-domains.csv.gz` | RFC 4180 CSV | Enriched threat domains (`domain,registered_domain,threat_category,confidence,source_count,sources`). |
| `dgspamguard-threat-domains.txt.gz` | UTF-8 Lines | Sorted ASCII/Punycode threat domains (`1` per line) for MTA/proxy blocklists. |
| `dgspamguard-threat-urls.csv.gz` | RFC 4180 CSV | Canonical malicious URLs (`url_sha256,domain,canonical_url,threat_category,confidence,sources`). |
| `dgspamguard-smtp-ips.csv.gz` | RFC 4180 CSV | Malicious SMTP sending IPs, botnet C2s & hijacked CIDRs (`network,threat_category,confidence,source_count,sources`). |
| `dgspamguard-email-domains.csv.gz` | RFC 4180 CSV | Email provider taxonomy (`domain,registered_domain,email_kind,confidence,sources` — disposable, free, trusted, redirector). |
| `dgspamguard-malware-hashes.csv.gz` | RFC 4180 CSV | Malware attachment `SHA-256` indicators (`sha256,malware_family,confidence,sources`). |
| `dgspamguard-tld-risk.csv.gz` | RFC 4180 CSV | High-abuse & `RFC 2606` TLD risk multipliers (`tld,risk_score,default_category,notes`). |
| `dgmail-rules.json.gz` | DGMail JSON | Self-hosted DGMail spam-classifier ruleset (`<= 440 KiB` compressed, zero external `HttpLookup` or DNSBL dependencies). |
| `manifest.json` | RFC 8259 JSON | Structured version metadata, record counts, byte sizes, and `SHA-256` digests. |
| `SHA256SUMS.txt` | `sha256sum` | Checksum file covering `bundle.tar.gz`, `manifest.json`, and all 9 `.gz` datasets. |

---

## Downloading & Verifying a Release

### Latest Release Direct URLs

```bash
BASE="https://github.com/daviante-org/dgspamguard-data/releases/latest/download"

curl -fsSLO "$BASE/bundle.tar.gz"
curl -fsSLO "$BASE/dgmail-rules.json.gz"
curl -fsSLO "$BASE/dgspamguard-threat-domains.rpz.gz"
curl -fsSLO "$BASE/manifest.json"
curl -fsSLO "$BASE/SHA256SUMS.txt"
```

### Integrity & Provenance Verification

1. **Verify `SHA-256` digests**:
   ```bash
   sha256sum --ignore-missing -c SHA256SUMS.txt
   ```
2. **Verify GitHub OIDC SLSA build provenance**:
   ```bash
   gh attestation verify bundle.tar.gz --repo daviante-org/dgspamguard-data
   ```

---

## Upstream Intelligence Attribution

| Category | Feed | Format | License |
| :--- | :--- | :--- | :--- |
| **Allowlist & Domain Boundaries** | `Mozilla Public Suffix List`, `Tranco Top 1M` (`https://tranco-list.eu`) | `.dat`, `.csv.zip` | MPL-2.0 / Open Research |
| **Phishing Domains/URLs** | `CERT Polska (CERT.PL)`, `OpenPhish Community`, `Phishunt`, `Phishing.Database`, `Phishing Army`, `malware-filter` | `.txt` | Open CSIRT / MIT / CC-BY |
| **Malware URLs & Botnets** | `abuse.ch URLhaus`, `ThreatFox`, `MalwareBazaar`, `Feodo Tracker` | `.json`, `.txt`, `.csv` | CC0 1.0 |
| **Bad Subnets & IPs** | `Spamhaus DROP` (`drop_v4.json`, `drop_v6.json`), `IPsum`, `Blocklist.de (mail & bots)` | `.json`, `.txt` | Free w/ Credit / Unlicense |
| **Burner & Free Email Senders** | `disposable-email-domains` (`domains_mx`), `mailchecker`, `burner-email-providers`, `willwhite/freemail` | `.txt`, `.conf` | CC0 / MIT |
| **Malware Attachment Hashes** | `romainmarcoux/malicious-hash`, `Botvrij.eu / CIRCL OSINT` | `.txt` | MIT / Open OSINT |

---

## Repository Structure & Documentation

- [`CREDITS.md`](./CREDITS.md) · [`NOTICE`](./NOTICE) — Complete third-party data ownership, copyright, academic citations, MIT/MPL/CC0 notices, and trademark disclaimers across all 30 feeds and baseline rule assets
- [`docs/FORMAT_SPEC.md`](./docs/FORMAT_SPEC.md) — Technical CSV, TXT, DNS RPZ, and `dgmail-rules.json.gz` schema specification
- [`docs/DATA_POLICY.md`](./docs/DATA_POLICY.md) — Upstream CSIRT/OSINT provenance, shared-hosting subtraction rules, and correction policy
- [`docs/TERMS.md`](./docs/TERMS.md) — Terms of Use and threat-intelligence operational disclaimer
- [`docs/PRIVACY.md`](./docs/PRIVACY.md) — Dataset and `https://dgspamguard.daviante.org` API Privacy Policy
- [`docs/SECURITY.md`](./docs/SECURITY.md) — Vulnerability disclosure and artifact verification guide
- [`LICENSE`](./LICENSE) — Apache-2.0 (workflows, documentation, and Daviante compilation work; third-party upstream rights remain with their respective owners per [`CREDITS.md`](./CREDITS.md) and [`NOTICE`](./NOTICE))

Maintained by **Daviante** (`https://daviante.org`) — Contact: `code@daviante.org`.
