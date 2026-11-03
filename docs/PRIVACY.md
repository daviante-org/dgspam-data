# Privacy Policy

1. **Dataset Contents**: `dgspamguard-data` contains only infrastructure-level indicators (domain names, canonical malicious URLs, public routable hijacked/botnet/SMTP-attack IPv4/IPv6 CIDRs, public email provider domains, TLD metadata, and cryptographic `SHA-256` hashes of malware binaries) published by public CSIRTs and open threat repositories. No personal email addresses, user accounts, or private message contents are collected or published.
2. **Self-Hosted Deployments**: Running [`daviante-org/dgspamguard`](https://github.com/daviante-org/dgspamguard) or consuming `dgspamguard-data` artifacts transmits zero telemetry, analytics, or lookup queries to Daviante.
3. **Public API (`https://dgspamguard.daviante.org`)**: Client IP addresses connecting to the hosted public API are held transiently in volatile process memory strictly for per-minute rate limiting (`600 req/min` sliding window) and are never sold, profiled, or persisted to analytical databases.

Questions regarding privacy may be sent to **`code@daviante.org`**.
