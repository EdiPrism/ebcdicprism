# COBOL `COMP-3` Packed Decimal Byte Formula & `S0C7` Abend Diagnostics

## 1. Exact `COMP-3` (`PACKED-DECIMAL`) Byte Length Formula

In IBM Enterprise COBOL and ISO COBOL, `USAGE COMP-3` (`PACKED-DECIMAL`) packs two decimal digits (`0..9` BCD nibbles) into each byte and stores the algebraic sign in the trailing low nibble (`0xC` positive, `0xD` negative, `0xF` unsigned).

For any `PIC` clause with **`N` total decimal digits** (the sum of integer digits before `V` and fractional digits after `V`; implied `S` and `V` consume zero bytes):

```text
packedBytes = floor(N / 2) + 1
```

| COBOL Picture Clause | Total Digits (`N`) | Storage Width | Sample Decimal | Hex Bytes on Disk |
| :--- | :---: | :---: | :---: | :--- |
| `PIC S9(3) COMP-3` | 3 | **2 bytes** | `+125` | `12 5C` |
| `PIC S9(5)V99 COMP-3` | 7 | **4 bytes** | `+450.25` | `00 45 02 5C` |
| `PIC S9(7)V99 COMP-3` | 9 | **5 bytes** | `-120.00` | `00 00 12 00 0D` |
| `PIC 9(8) COMP-3` | 8 | **5 bytes** (high `0` pad) | `20261113` | `02 02 61 11 3F` |
| `PIC S9(13)V99 COMP-3` | 15 | **8 bytes** | `+9850.75` | `00 00 00 00 09 85 07 5C` |

> **Warning — Never Transfer Mixed `EBCDIC + COMP-3` Files in ASCII FTP Mode!**
> In `COMP-3`, byte `0x0D` represents packed digit `0` + negative sign `D`, and `0x15` represents packed digits `15` — **not** carriage-return or EBCDIC newline (`NL`) characters. Running text-mode `iconv` or ASCII FTP corrupts packed monetary fields irreversibly.

---

## 2. Diagnosing z/OS `S0C7` Data Exceptions Offline Before Nightly Batch

A z/OS System Completion Code `0C7` (`S0C7`) hardware trap fires whenever a decimal instruction (`AP`, `SP`, `ZAP`, `CP`, `CVB`) reads:
1. A digit nibble outside `0x0..0x9` (`0xA..0xF`), or
2. A trailing sign nibble outside `0xA..0xF` (`0x0..0x9`).

| Hex Pattern in `.ebcdic` | Typical Root Cause | Why Hardware Traps with `S0C7` |
| :--- | :--- | :--- |
| `40 40 40 40 40` | Group-level `MOVE SPACES` in COBOL | Final byte `0x40` has low sign nibble `0x0` (invalid). |
| `20 20 20 20 20` | Java / Python / C# ETL writer padded with ASCII spaces | Final byte `0x20` has low sign nibble `0x0` (invalid). |
| `00 00 00 00 00` | Uninitialized buffer (`LOW-VALUES`) | Trailing byte `0x00` has sign nibble `0x0` instead of `0x0C`/`0x0F`. |
| `F0 F0 F0 F0 F0` | Group-level `MOVE ZEROS` (alphanumeric move!) | Every high nibble is `0xF` (`> 9`), invalid as a packed digit. |

### Catching `S0C7` Inside Your JetBrains IDE
1. Open your `.cpy` copybook and `.ebcdic` extract locally in **EbcdicPrism Copybook Viewer** (zero network calls).
2. Switch to the **`S0C7`** validator tab to scan every record on a background thread and jump directly to the exact offending record number, field name, 1-based column span, and raw hex bytes.

Full interactive reference: **[https://ediprism.github.io/cobol/fix-s0c7-data-exception-ebcdic.html](https://ediprism.github.io/cobol/fix-s0c7-data-exception-ebcdic.html)**
