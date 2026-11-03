# Terms of Use

1. **As-Is Threat Intelligence**: Datasets in `daviante-org/dgspamguard-data` and responses from `https://dgspamguard.daviante.org` are provided **"AS IS"** without warranty of any kind. Threat intelligence scores and classifications represent automated corroboration of public security feeds at a specific point in time and may contain false positives or false negatives.
2. **Upstream Ownership & Scope of Rights**: Daviante's Apache-2.0 license applies strictly to its own workflows, documentation, and compilation/normalization work, and **does not claim ownership over or grant, transfer, or sublicense any proprietary rights belonging to upstream data providers**. Commercial vendors, ISPs, and managed service providers remain solely responsible for complying with the terms of each underlying upstream source documented in [`CREDITS.md`](../CREDITS.md) and [`NOTICE`](../NOTICE).
3. **Operational Responsibility**: Operators integrating `dgspamguard-threat-domains.rpz.gz`, `dgmail-rules.json.gz`, or CSV/API feeds into mail transfer agents, DNS resolvers, or authentication workflows remain solely responsible for configuring appropriate scoring thresholds, quarantine policies, and local allowlists.
4. **Fair Public API Usage**: Automated high-volume consumers should download the 4-hour rolling snapshots from GitHub Releases or self-host [`daviante-org/dgspamguard`](https://github.com/daviante-org/dgspamguard) rather than sending sustained bulk traffic to the public `https://dgspamguard.daviante.org` instance.

Contact: **`code@daviante.org`**.
