# Text Encoding and Unicode Compatibility

## Purpose

Make text handling deterministic across operating systems, shells, runtimes, editors, generated artifacts, and test environments. Encoding is part of the program's compatibility contract, not an incidental editor setting.

## 1. Declare encoding deliberately

- Follow the repository's documented encoding convention; do not rely on an editor, shell, operating system, or runtime default.
- Prefer UTF-8 for repository text unless a target runtime or file format requires a different encoding.
- Specify the encoding explicitly when reading or writing files at runtime.
- Keep the declared encoding, actual byte encoding, and any format-level declaration consistent (for example, an HTML `<meta charset="utf-8">` declaration).
- Do not convert text through the system's ANSI code page unless that behavior is an explicit external contract.
- Treat console display encoding separately from file encoding. A terminal rendering problem does not by itself prove that file bytes are corrupted.

## 2. Account for runtime-specific source-file rules

- Verify how every supported runtime interprets source files, especially when source contains non-ASCII literals.
- For Windows PowerShell 5.1, a script/module/data file containing non-ASCII source characters must be saved as UTF-8 with BOM, or the source should remain ASCII-only and construct the required characters through code points/escape-safe mechanisms. Windows PowerShell 5.1 can interpret BOM-less UTF-8 source as the active ANSI code page.
- Do not assume PowerShell 7+ encoding behavior applies to Windows PowerShell 5.1.
- When supporting multiple runtime versions, test the oldest supported runtime rather than inferring compatibility from a newer one.

## 3. Handle generated and exchanged text explicitly

- Choose and document the encoding expected by each external file contract.
- For generated HTML, emit valid UTF-8 bytes and include a matching charset declaration.
- Use explicit encodings in file APIs and avoid legacy cmdlet defaults whose behavior differs by PowerShell version.
- Preserve Unicode without lossy conversion when generating reports, logs, exports, or user-visible text.
- If a downstream consumer requires a BOM or a legacy encoding, implement that as an explicit compatibility requirement rather than changing the repository-wide default.

## 4. Make tests encoding-aware

- Test the actual file round trip where encoding is part of the behavior: construct input, write bytes, read with the intended encoding, and assert the resulting text.
- Include representative non-ASCII characters when the feature claims Unicode support; include supplementary-plane characters (such as emoji) when relevant.
- For Windows PowerShell 5.1 tests, avoid fragile non-ASCII literals in BOM-less test source. Prefer code-point construction, or ensure the test source is UTF-8 with BOM and verify that the repository preserves it.
- Read generated artifacts using an explicit encoding. Do not let the test runner's default decoding silently mask or introduce corruption.
- Assert the intended Unicode text, not a mojibake representation produced by the same faulty path under test.
- Keep test fixtures and expected values independent of the implementation's encoding conversion so a round-trip test can detect corruption rather than reproduce it.

## 5. Diagnose before changing production code

When text appears corrupted:

1. Identify the first boundary where the text changes: source parsing, input decoding, in-memory conversion, output encoding, file reading, or terminal rendering.
2. Inspect the source file's actual bytes/BOM and the runtime version/code page.
3. Compare the in-memory string with the bytes written and the string read back using an explicit encoding.
4. Reproduce under the oldest supported runtime and, where useful, a modern runtime.
5. Change the narrowest faulty boundary. Do not alter a correct renderer, serializer, or output contract to compensate for a corrupted test fixture or terminal display.

A passing test on one runtime does not establish encoding compatibility on another runtime.
