# `dgspamguard-data` Format Specification

Every release in `daviante-org/dgspamguard-data` publishes **12 files**: `bundle.tar.gz`, `manifest.json`, `SHA256SUMS.txt`, and **9 GZIP-compressed (`.gz`) dataset files**. All text encodings are UTF-8 without BOM, line endings are `\n` (`LF`), and all domains are normalized to lowercase IDNA 2008 / Punycode ASCII without trailing dots.

---

## 1. `dgspamguard-threat-domains.csv.gz` & `dgspamguard-threat-domains.txt.gz`

- `dgspamguard-threat-domains.csv.gz` (RFC 4180 CSV):
  ```csv
  domain,registered_domain,threat_category,confidence,source_count,sources
  ```
- `dgspamguard-threat-domains.txt.gz`: Sorted lowercase ASCII/Punycode threat domains (`1` per line, no comments or blank lines).

---

## 2. `dgspamguard-threat-urls.csv.gz` (RFC 4180 CSV)

Header row:
```csv
url_sha256,domain,canonical_url,threat_category,confidence,sources
```

- `canonical_url` normalizes scheme (`http`/`https`), lowercases Punycode host (or canonical IPv4/IPv6 literal), strips default ports (`:80`, `:443`) and `#fragment`, and preserves exact path + query string.
- `url_sha256` is the lowercase 64-character hex `SHA-256` digest of `canonical_url`.

---

## 3. `dgspamguard-smtp-ips.csv.gz` (RFC 4180 CSV)

Header row:
```csv
network,threat_category,confidence,source_count,sources
```

- `network`: Canonical IPv4/IPv6 CIDR notation (`198.51.100.77/32`, `203.0.113.0/24`, `2001:db8:abcd::/48`) with host bits masked to network boundary.
- `threat_category`: `hijacked_cidr`, `c2`, `smtp_attack`, `botnet`, `malware`, or `phishing`.

---

## 4. `dgspamguard-email-domains.csv.gz` (RFC 4180 CSV)

Header row:
```csv
domain,registered_domain,email_kind,confidence,sources
```

- `email_kind`: `disposable`, `free_provider`, `trusted`, or `url_redirector`.

---

## 5. `dgspamguard-malware-hashes.csv.gz` (RFC 4180 CSV)

Header row:
```csv
sha256,malware_family,confidence,sources
```

---

## 6. `dgspamguard-tld-risk.csv.gz` (RFC 4180 CSV)

Header row:
```csv
tld,risk_score,default_category,notes
```

---

## 7. `dgspamguard-threat-domains.rpz.gz` (DNS Response Policy Zone)

Standard BIND/RFC DNS RPZ zone file starting with `$TTL 300` and `@ IN SOA localhost. hostmaster.daviante.org. (...)`:
```dns
evil-login-example.com CNAME .
*.evil-login-example.com CNAME .
```
Each RPZ-eligible domain (corroborated threat domain, not Tranco Top 100K, not a shared-hosting root, and not flagged `abused_legit_*` or offline-only) includes both the exact owner name and wildcard subdomain `CNAME .` (`NXDOMAIN`) policy records.

---

## 8. `dgmail-rules.json.gz` (DGMail Spam-Classifier Bundle)

Drop-in compressed JSON ruleset compatible with DGMail's `[spam-filter.updates]` loader:
- Guaranteed `<= 450,560` bytes (`440 KiB`) compressed so DGMail's `512,000`-byte HTTP fetch ceiling is never exceeded.
- `HttpLookup: []` (zero outbound third-party HTTP calls) and all `SpamDnsblServer` entries set to `enable: false`.
- Populates in-memory lookup maps (`dgm_free_domains`, `dgm_disposable_domains`, `dgm_openphish`, `dgspamguard_threat_domains`, `dgspamguard_smtp_threat_ips`) and rule expressions (`DGSPAMGUARD_SMTP_THREAT_IP` matching `remote_ip`, `DGSPAMGUARD_THREAT_URI_DOMAIN`, `DGSPAMGUARD_THREAT_SENDER_DOMAIN`, `DGSPAMGUARD_ABUSED_TLD_SENDER`, `DGSPAMGUARD_ABUSED_TLD_URI`).
