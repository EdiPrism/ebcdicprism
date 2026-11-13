# Changelog — EbcdicPrism Copybook Viewer

All notable changes to **EbcdicPrism Copybook Viewer** (`dev.ediprism.ebcdic`) are documented in this file.

## [1.0.1] - 2026-11-13

### Added
- **Air-Gap-Safe Feedback Menu & Sanitized Crash Diagnostics (`?` ToolWindow Header & `IDE Fatal Errors`):** Added a zero-socket `?` Feedback & Support menu on the `EbcdicPrism` ToolWindow header bar (`Report Bug`, `Suggest Feature`, and `Copy Air-Gapped Diagnostic Template` for restricted banking VDIs) plus a native IntelliJ `<errorHandler>` that strips local file paths and redacts raw exception messages so PCI-DSS PANs or copybook lines can never leak.

## [1.0.0] - 2026-11-13

### Added
- **Free Tier — COBOL Copybook (`.cpy`, `.copy`, `.cobcopy`) Highlighting, Inline Offset Pills & Hover Cards:** Instant 1-based (Mainframe JCL/`DFSORT`) and 0-based (Java/Python) start/end byte offsets, inline byte-range pills (`52..56 • 5B COMP-3`), total `01` `LRECL`, `COMP-3` packed lengths, `OCCURS` array strides, and `REDEFINES` overlay targets on hover and `Ctrl+Q`.
- **Free Tier — Searchable (`Ctrl+F`) Multi-Codepage EBCDIC & Hex Viewer:** Native read-only paged editor for `20–50+ MB` binary mainframe extracts across `IBM037` (US/Canada), `IBM1047` (z/OS USS), `IBM500`, `IBM273`, `IBM285`, `IBM297`, and `IBM01140` (Euro), including top-of-dump `# Copybook PIC:` summary headers and Fixed-Block (`FB`) / Variable-Blocked 4-byte `RDW` (`VB`) auto-detection.
- **`[Pro]` Copybook-Over-Binary Record Decoder & Hex Hover Cards:** Decodes `PIC X`, signed/unsigned Zoned Decimal (`{A-I` / `}J-R` trailing overpunch), `COMP-3` Packed Decimal, Big-Endian `COMP`/`BINARY`, `REDEFINES` branches, and active `88`-level condition flags per record with multi-row Hex + EBCDIC byte-span hover highlighting and click-to-sync.
- **`[Pro]` Batch `S0C7` & `RDW` Corruption Validator:** Background non-blocking scanner pinpointing exact record numbers and 1-based byte columns containing invalid `COMP-3` digit/sign nibbles (such as ASCII `0x20` or EBCDIC `0x40` spaces inside packed fields) and truncated `FB`/`RDW` frames.
- **`[Pro]` Semantic Binary Record Diff (`Alt+Shift+B` & Native `Ctrl+D`):** Side-by-side decoded record comparison through a shared `.cpy` copybook with automatic masking of volatile batch header timestamps and sequence numbers.
- **`[Pro]` Deterministic Offline PCI-DSS / PII Binary Scrubber (`Alt+Shift+R`) & SQL/JSON/CSV Exporter:** In-place keyed `HmacSHA256` binary scrubber preserving 6-digit issuer BINs, valid ISO/IEC 7812 Luhn Mod-10 check digits, `COMP-3` nibbles, and exact `LRECL`, plus 1-click SQL `CREATE TABLE` DDL, JSON, and CSV generation.
