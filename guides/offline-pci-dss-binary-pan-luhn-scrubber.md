# Offline PCI-DSS Card PAN & PII Scrubbing Inside Binary `EBCDIC` Test Fixtures

## Why Text-Based Masking Corrupts Mainframe Binary Test Files

PCI-DSS Requirement 6.5.3 prohibits live Primary Account Numbers (`PAN`s) and cardholder PII in development and CI/CD test repositories. However, running regex text replacements on a binary `.ebcdic` file fails immediately:

1. **Codepage & Packed Storage:** Card numbers and SSNs may be stored in `IBM037` Zoned EBCDIC (`0xF0..0xF9`) or `COMP-3` Packed Decimal BCD nibbles — invisible to UTF-8/ASCII regex tools.
2. **Strict `LRECL` & `RDW` Framing:** Changing a field's byte count by even 1 byte shifts every subsequent column in the record, triggering `S0C7` abends on downstream `COMP-3` amounts.
3. **ISO/IEC 7812 Luhn Mod-10 Check Digits:** Replacing a 16-digit PAN with `9999999999999999` fails payment-gateway Luhn validation before business logic is ever reached.

---

## Deterministic In-Place Binary Scrubbing (`Alt+Shift+R`)

**EbcdicPrism Copybook Viewer** (`[Pro]`) scrubs `.ebcdic` files **100% offline** in local JVM memory using your `.cpy` copybook layout:

- **Preserves 6-Digit Issuer BIN (`IIN`):** Visa (`453201...`), Mastercard (`542523...`), and Amex routing branches continue to execute identically in integration tests.
- **Recomputes Valid Luhn Mod-10 Check Digits:** Every synthetic PAN passes strict Luhn check-digit verification.
- **Session-Keyed `HmacSHA256` Referential Integrity:** Identical input PANs scrub to identical synthetic PANs across authorization, clearing, settlement, and chargeback batch files.
- **Zero Byte-Offset Drift:** Writes back the exact original byte width in `EBCDIC` or `COMP-3`, leaving `TXN-AMOUNT` packed totals and `RDW` headers 100% intact.

Full interactive reference: **[https://ediprism.github.io/cobol/pci-dss-binary-pan-luhn-scrubber.html](https://ediprism.github.io/cobol/pci-dss-binary-pan-luhn-scrubber.html)**
