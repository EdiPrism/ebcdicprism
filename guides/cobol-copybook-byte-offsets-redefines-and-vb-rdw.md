# Calculating COBOL Copybook Byte Offsets Across `REDEFINES`, `OCCURS`, and `VB RDW`

## 1. Mainframe 1-Based Columns vs. JVM / Python 0-Based Slices

One of the most frequent off-by-one bugs in mainframe modernization pipelines is mixing JCL `DFSORT` / `ICETOOL` 1-based positions with JVM/Python 0-based byte slices:

- **Mainframe 1-Based Inclusive (`Cols start..end`):** `Cols 52..56` (`5` bytes: `INCLUDE COND=(52,5,PD,GT,0)`).
- **Java / Kotlin / Python 0-Based Half-Open (`bytes[start..<end]`):** `record.copyOfRange(51, 56)` or `record[51:56]` (`56 - 51 == 5` bytes).

Hovering any field in **EbcdicPrism Copybook Viewer** (Free Tier) displays both coordinate systems side by side alongside inline ghost-text pills.

---

## 2. `REDEFINES` Union High-Water-Mark Rule

When sibling item `B` declares `REDEFINES A`:
1. `B`'s start offset rewinds to `A`'s exact start byte offset.
2. The parent group's running cursor advances to `max(endOffset(A), endOffset(B))` so union overlays never double-count storage in the `01` record's total `LRECL`.

---

## 3. Fixed-Block (`FB`) vs. Variable-Blocked 4-Byte `RDW` (`VB`)

- **Fixed-Block (`FB`):** Every record occupies exactly `LRECL` bytes back-to-back with **no** `0x0A` or `0x0D0A` delimiters (`fileSize % LRECL == 0`).
- **Variable-Blocked (`VB` with `RDW`):** Each logical record is prefixed by a 4-byte Record Descriptor Word `[LL_hi, LL_lo, 0x00, 0x00]`, where Big-Endian unsigned 16-bit `LL` equals `4 + payloadBytes`. If your parser forgets to strip or account for the 4-byte `RDW`, every copybook field shifts right by 4 bytes!

Full interactive reference: **[https://ediprism.github.io/cobol/cobol-copybook-redefines-occurs-offsets.html](https://ediprism.github.io/cobol/cobol-copybook-redefines-occurs-offsets.html)**
