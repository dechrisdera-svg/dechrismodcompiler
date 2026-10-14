# What is verified and what is NOT  (DMC 2.0.0)

Environment used to build and test this release: Linux, Python 3.12, **no tkinter, no Windows, no phone, no GTA**.
Automated tests: `python -m unittest discover -s tests -t .`  (74 tests; build.bat runs them before packaging).

## Verified by automated tests
* Compiler output is **byte-identical to v1.0** for both example scripts and 6 extra snippets (tests/legacy_dmc_v1.py is the old compiler).
* Byte rules you verified earlier: instruction layout, negative absolute label offsets, `00D6` AND/OR counts,
  and the first bytes of `script_name 'BODYGUARD'` / `wait 0` (checked from the facts in your prompt - the real
  Body_Guard.csi was NOT in the upload; drop it into tests/data/ to enable the full 819-instruction test).
* What compiled control flow really does: a small virtual machine executes compiled if/else, while, repeat, for,
  break/continue, nested and multi-condition blocks and checks the results.
* Decompiler: examples round-trip to identical bytes; 60 random library-built scripts and 40 random garbage files never
  crash and recompile identically (odd instructions fall back to raw / hex lines).
* Library loader on a synthetic file with the exact Sanny Builder Library schema (id/name/input/output/source/operator/attrs,
  extensions, is_unsupported), derived operator tables equal the built-in SA tables; download code with a mocked network
  (success, no internet, HTTP 404, garbage file); settings corruption fallback; ADB logic with a mocked adb.
* Formatter is idempotent and keeps compiled bytes; rename/references skip strings and comments; every snippet compiles.
* GUI code: syntax-checked, undefined-name scan, and smoke-run against a **fake tkinter** (start, new tab, compile, errors,
  decompile flow, missing-library prompt, ADB-missing prompt). This only catches typos and logic errors.

## NOT verified - please test and tell me
1. **The entire GUI on real Tk/Windows** (layout, colors, tab drag, folding, minimap, autocomplete popup, hover, drag & drop,
   high-DPI, theme switching). Expect small visual bugs.
2. **build.bat, PyInstaller, Inno Setup** on Windows (never executed). Logic reviewed only.
3. **Real library download**: URL pattern is `raw.githubusercontent.com/sannybuilder/library/master/<dir>/<name>.json`.
   `sa/sa.json` was confirmed to exist; the `sa_mobile` folder name is assumed. Full real files were never loaded
   (only the bundled trimmed Android library and a synthetic file).
4. **PC CLEO 4 / CLEO 5 output in the game.** Same bytecode rules as Android; string quoting (`"..."` -> type 0E) and
   string-variable type bytes (0A/0B/10/11) come from my knowledge of CLEO's data types, not from a test.
5. In the game: **0F vs 0E for long strings**, **global variable index scale (x4)** on Android, **array encoding**,
   the **00 terminator for variadic ("arguments") commands**. DMC prints a warning when a script uses one of these.
6. CLI output from the windowed exe (it attaches to the parent console via Win32 - untested).
7. ADB push with a real phone; copy-to-game on a real PC.
8. GTA III / Vice City support was deliberately **not** added: float encoding and headers differ and I could not verify them.
