# Data Provenance & Compilation Policy

`dgspamguard-data` snapshots are compiled by [`daviante-org/dgspamguard`](https://github.com/daviante-org/dgspamguard) across **5 intelligence layers** and **30 authoritative feeds**:

- **Layer 0 — Domain Boundaries & Allowlist Gates**: Mozilla Public Suffix List (`MPL-2.0`) and the Tranco Top 1M Research Ranking (`https://tranco-list.eu`).
- **Layer 1 — Active Phishing, Malware & C2 Domains/URLs**: CERT Polska (`hole.cert.pl`), OpenPhish Community Feed, Abuse.ch URLhaus (`CC0`), Abuse.ch ThreatFox (`CC0`), Phishunt (`phishunt.io`), Phishing Army Extended (`phishing.army`), `malware-filter`, and `Phishing.Database` (`ACTIVE` domains, `NEW-today` domains, and `ACTIVE` links, `MIT`).
- **Layer 2 — Malicious Sending IPs, Botnets & Hijacked Subnets**: Spamhaus DROP (`drop_v4.json` & `drop_v6.json`, attributed to The Spamhaus Project), IPsum Level 3+ (`stamparm/ipsum`, `Unlicense`), Blocklist.de (`mail.txt` & `bots.txt`), Abuse.ch Feodo Tracker (`CC0`), and `Phishing.Database` Active IPs (`MIT`).
- **Layer 3 — Email Provider Taxonomy**: `disposable-email-domains` (`domains_mx.txt` & blocklist, `CC0`), `FGRibreau/mailchecker` (`MIT`), `wesbos/burner-email-providers` (`MIT`), and `willwhite/freemail` (`MIT`).
- **Layer 4 — Malware Attachment Hashes**: Abuse.ch MalwareBazaar (`CC0`), Abuse.ch URLhaus Payloads (`CC0`), `romainmarcoux/malicious-hash` (`MIT`), and Botvrij.eu / CIRCL OSINT (`SHA-256` indicators).

---

## Subtraction & False-Positive Safeguards

1. **Trusted Apex Allowlist & Tranco Shields**: Exact domains in Tranco Top 100K and enterprise roots in Tranco Top 10K (or curated `trusted` entries) are never emitted as malicious domains or RPZ blocks.
2. **URLhaus RPZ Safeguards**: Offline URLs and compromised legitimate sites flagged `abused_legit_*` are matched strictly at exact-URL granularity (`dgspamguard-threat-urls.csv.gz`), never promoted to whole-domain RPZ blocks.
3. **Multi-Tenant Shared-Hosting Separation**:
   - Subdomain-isolated platforms (`pages.dev`, `github.io`, `vercel.app`, `netlify.app`, `web.app`, `duckdns.org`, `blogspot.com`) protect the platform root while allowing dedicated malicious tenant subdomains to be blocked.
   - Path-isolated shared platforms (`docs.google.com`, `drive.google.com`, `dropbox.com`, `onedrive.live.com`, `raw.githubusercontent.com`, `discord.com`, `t.me`) are never blocked at the hostname level; only exact canonical URLs (`dgspamguard-threat-urls.csv.gz`) are flagged.
4. **Bogon & Public DNS Resolver IP Shield**: Private RFC 1918 ranges, loopback, CGNAT, overly broad prefixes, and public DNS resolvers (`1.1.1.1`, `8.8.8.8`, `9.9.9.9`, `208.67.222.222`) are automatically excluded from `dgspamguard-smtp-ips.csv.gz`.
5. **7-Day Rolling Expiration**: GitHub Releases older than 7 days are automatically pruned so remediated domains, IPs, and expired ephemeral infrastructure age out cleanly.

---

## Reporting a False Positive

To request review or delisting of an indicator in `dgspamguard-data`, email **`code@daviante.org`** with the exact indicator, snapshot version tag (`vYYYYMMDD.HHMM`), and remediation details.
