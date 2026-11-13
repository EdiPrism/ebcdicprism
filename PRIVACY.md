# Privacy & Air-Gapped Security Attestation — EbcdicPrism Copybook Viewer

**Effective Date:** November 2026

## 1. Zero Data Collection & 100% Offline Execution
**EbcdicPrism Copybook Viewer** (`dev.ediprism.ebcdic`) operates **100% locally and offline** inside your JetBrains IDE process (`IntelliJ IDEA`, `DataGrip`, `PyCharm`, `Rider`, `GoLand`, `WebStorm`):

- **Zero Network Calls:** EbcdicPrism contains **no** HTTP clients (`java.net.http`, `HttpURLConnection`, `OkHttp`, `Ktor`), cloud SDKs, or external LLM integrations.
- **Zero Payload Transmission:** COBOL Copybooks (`.cpy`, `.copy`), binary `EBCDIC` mainframe extracts (`.ebcdic`, `.ebc`), `COMP-3` packed fields, SQL DDL schemas, and PCI-scrubbed test fixtures **never leave your workstation**.
- **Zero Telemetry or Analytics:** EbcdicPrism collects no usage telemetry, crash dumps, file names, schema metadata, identifiers, or analytics.

## 2. Offline Licensing Verification
Commercial `[Pro]` subscriptions and 14-day free trials are handled natively by **JetBrains Marketplace (`JetBrains s.r.o.`)** via the IDE's built-in `LicensingFacade` subsystem and verified locally offline using pinned JetBrains X.509 RSA root certificates (`PEBCDICPRISM`). When unlicensed, EbcdicPrism runs silently in Free Tier mode with zero startup nag modals.

## 3. Contact & Support
For enterprise procurement, air-gapped security questionnaires, or technical issue reporting, visit **[https://ediprism.github.io/support.html](https://ediprism.github.io/support.html)** or open an issue on **[GitHub Issues](https://github.com/EdiPrism/ebcdicprism/issues)** (never attach un-scrubbed production PAN/PII data).
