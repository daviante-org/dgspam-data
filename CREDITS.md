# Data Sources, Ownership, Credits & Legal Attribution

**Repository:** `daviante-org/dgspamguard-data`  
**Compiler Engine:** [`daviante-org/dgspamguard`](https://github.com/daviante-org/dgspamguard) (`https://dgspamguard.daviante.org`)  
**Maintainer:** Daviante (`https://daviante.org`)  
**Legal, False-Positive & Provenance Contact:** `code@daviante.org`

---

## 1. Ownership Statement & Scope of License

1. **Scope of Daviante's License (`Apache-2.0`)**: Workflows, schema specifications, documentation, and Daviante's own compilation work (IDNA/PSL boundary normalization, multi-tenant shared-hosting subtraction, multi-feed corroboration scoring, and artifact formatting) in this repository are licensed under the [Apache License, Version 2.0](./LICENSE) (see [`NOTICE`](./NOTICE)).
2. **No Claim of Ownership Over Upstream Data**: **Daviante does not claim copyright, sui generis database ownership, or proprietary title over any third-party CSIRT warning lists, OSINT threat indicators, domain suffix registries, popularity rankings, SMTP IP blocklists, email provider taxonomies, or malware hash corpora**, and **does not grant, transfer, or sublicense any proprietary rights belonging to upstream data providers**.
3. **Downstream Responsibility**: Organizations, ISPs, managed security providers, or commercial vendors consuming `dgspamguard-data` releases remain solely responsible for reviewing and complying with the licensing and usage terms of each underlying upstream provider listed below.
4. **Nominative Attribution & Non-Endorsement**: All third-party names, logos, and trademarks belong to their respective owners and are cited strictly for data-provenance attribution, with zero implied affiliation, sponsorship, or endorsement.

---

## 2. Complete Upstream Data Credits (30 Feeds Across 5 Layers)

### 2.1 Layer 0 — Domain Boundaries & False-Positive Subtraction Gates

| Source | Upstream Owner / Authors | Canonical Origin | License & Redistribution Note |
| :--- | :--- | :--- | :--- |
| **Mozilla Public Suffix List** | **Mozilla Foundation** & contributors | `https://publicsuffix.org/list/public_suffix_list.dat` | **MPL-2.0** ([Mozilla Public License 2.0](https://mozilla.org/MPL/2.0/)) — used to compute `registered_domain` boundaries. |
| **Tranco Top Sites Ranking** | **Victor Le Pochat, Tom Van Goethem, Samaneh Tajalizadehkhoob, Maciej Korczyński, Wouter Joosen** (KU Leuven / UGA) | `https://tranco-list.eu/top-1m.csv.zip` | **Academic Research Ranking** ([`tranco-list.eu`](https://tranco-list.eu/)). Used **strictly internally during compilation** as a false-positive subtraction shield (`Top-100K` / `Top-10K`); **Tranco rankings are never redistributed** in `dgspamguard-data` files. |

**Academic Citation for Tranco:**
> Victor Le Pochat, Tom Van Goethem, Samaneh Tajalizadehkhoob, Maciej Korczyński, and Wouter Joosen. *"Tranco: A Research-Oriented Top Sites Ranking Hardened Against Manipulation."* Proceedings of the 26th Annual Network and Distributed System Security Symposium (NDSS), 2019. `https://doi.org/10.14722/ndss.2019.23386`

### 2.2 Layer 1 — Phishing, Malware Distribution & Botnet C2 Domains/URLs

| Source | Upstream Owner / Maintainer | Canonical Origin | License / Terms |
| :--- | :--- | :--- | :--- |
| **URLhaus** | **abuse.ch** (Institute for Cybersecurity and Engineering ICE, Bern University of Applied Sciences) | `https://urlhaus.abuse.ch/` | **CC0 1.0 Universal (Public Domain)** ([URLhaus API Terms](https://urlhaus.abuse.ch/api/)) |
| **ThreatFox** | **abuse.ch** (ICE, Bern University of Applied Sciences) | `https://threatfox.abuse.ch/` | **CC0 1.0 Universal (Public Domain)** |
| **CERT Polska Warning List** | **CERT Polska / NASK** (National Research Institute, Poland) | `https://hole.cert.pl/domains/v2/domains.txt` | **Public CSIRT Warning Feed** ([CERT Polska](https://cert.pl/posts/2020/03/ostrzezenia_phishing/)) |
| **`malware-filter` (`phishing-filter` & `urlhaus-filter`)** | **curben & `malware-filter` contributors** | `https://gitlab.com/malware-filter` | **MIT License / CC0 1.0** |
| **OpenPhish Community Feed** | **OpenPhish** | `https://openphish.com/feed.txt` | **OpenPhish Community Feed** (subject to [OpenPhish Terms of Use](https://openphish.com/terms.html)) |
| **Phishunt** | **Phishunt** | `https://phishunt.io/feed.txt` | **Public Community Phishing Feed** ([`phishunt.io`](https://phishunt.io/)) |
| **Phishing Army** | **Andrea Draghetti** | `https://phishing.army/` | **Free Public Blocklist** ([Phishing Army License](https://phishing.army/) — provided "AS IS" without transferring upstream rights) |
| **`Phishing.Database`** (`ACTIVE` & `NEW-today` domains/links) | **Mitchell Krog, Nissar Chababy (`@funilrys`), Funceble & contributors** | `https://github.com/Phishing-Database/Phishing.Database` | **MIT License** |

### 2.3 Layer 2 — Malicious Sending IPs, Botnets & Hijacked Subnets (`dgspamguard-smtp-ips.csv.gz`)

| Source | Upstream Owner / Maintainer | Canonical Origin | License / Attribution Notice |
| :--- | :--- | :--- | :--- |
| **Spamhaus DROP (`IPv4` & `IPv6`)** | **The Spamhaus Project** (`© The Spamhaus Project`) | `https://www.spamhaus.org/drop/drop_v4.json` · `drop_v6.json` | **Free with Attribution**: Provided free of charge by **The Spamhaus Project** ([Spamhaus DROP](https://www.spamhaus.org/drop/)). All underlying copyright and database rights remain with The Spamhaus Project. |
| **Feodo Tracker** | **abuse.ch** (ICE, Bern University of Applied Sciences) | `https://feodotracker.abuse.ch/` | **CC0 1.0 Universal (Public Domain)** |
| **IPsum (`Level 3+`)** | **Miroslav Stampar (`@stamparm`)** | `https://github.com/stamparm/ipsum` | **The Unlicense (Public Domain)** |
| **Blocklist.de (`mail` & `bots`)** | **Martin Schurig & Blocklist.de contributors** | `https://www.blocklist.de/` | **Free Community Abuse Feed** |
| **`Phishing.Database` Active IPs** | **Mitchell Krog, Nissar Chababy (`@funilrys`) & contributors** | `https://github.com/Phishing-Database/Phishing.Database` | **MIT License** |

### 2.4 Layer 3 — Disposable, Burner & Free Email Provider Taxonomies (`dgspamguard-email-domains.csv.gz`)

| Source | Upstream Owner / Maintainer | Canonical Origin | License |
| :--- | :--- | :--- | :--- |
| **`disposable/disposable-email-domains` (`domains_mx.txt`)** | **`disposable` contributors** | `https://github.com/disposable/disposable-email-domains` | **CC0 1.0 / Open Domain List** |
| **`disposable-email-domains`** | **Martenson & contributors** | `https://github.com/disposable-email-domains/disposable-email-domains` | **CC0 1.0 Universal (Public Domain)** |
| **`burner-email-providers`** | **Wes Bos (`@wesbos`) & contributors** | `https://github.com/wesbos/burner-email-providers` | **MIT License** |
| **`mailchecker`** | **Francois-Guillaume Ribreau (`@FGRibreau`) & contributors** | `https://github.com/FGRibreau/mailchecker` | **MIT License** |
| **`freemail`** | **Will White (`@willwhite`) & contributors** | `https://github.com/willwhite/freemail` | **MIT License** |

### 2.5 Layer 4 — Malware Attachment Hashes & Baseline Mail Rules (`dgspamguard-malware-hashes.csv.gz` · `dgmail-rules.json.gz`)

| Source | Upstream Owner / Maintainer | Canonical Origin | License |
| :--- | :--- | :--- | :--- |
| **MalwareBazaar & URLhaus Payloads** | **abuse.ch** (ICE, Bern University of Applied Sciences) | `https://bazaar.abuse.ch/` · `https://urlhaus.abuse.ch/` | **CC0 1.0 Universal (Public Domain)** |
| **`malicious-hash`** | **Romain Marcoux (`@romainmarcoux`)** | `https://github.com/romainmarcoux/malicious-hash` | **MIT License** |
| **Botvrij.eu OSINT IOCs** | **Botvrij.eu / CIRCL OSINT** | `https://www.botvrij.eu/data/ioclist.sha256.raw` | **Open OSINT Indicator Feed** |
| **Baseline Spam-Filter Rule Grammar** | **Stalwart Labs LLC** (adapted by Daviante) | `https://github.com/stalwartlabs/spam-filter` | **Apache-2.0 OR MIT** (sanitized by Daviante to remove all third-party runtime `HttpLookup` and live external DNSBL queries) |

---

## 3. Rights-Holder, Attribution & Delisting Contact

- **Upstream Maintainers & Rights Holders**: To request an attribution update, usage clarification, or removal of a source from future snapshots, email **`code@daviante.org`**.
- **False-Positive / Delisting Requests**: Email **`code@daviante.org`** with the exact indicator and snapshot version tag (`vYYYYMMDD.HHMM`), plus any upstream remediation status. Every snapshot ages out automatically after **7 days** under `dgspamguard-data`'s rolling retention window.
