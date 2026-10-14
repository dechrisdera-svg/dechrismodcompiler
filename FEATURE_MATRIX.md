# Sanny Builder vs DMC 2.0 - feature matrix

Source of the Sanny list: the official docs index (docs.sannybuilder.com: Language, Edit Modes, Editor, Options,
CLI, Language Service, Debugger sections) and the Sanny Builder Library repo. Only pages whose content I actually read
are described in detail; the rest are judged by their titles. This is **not** a claim that DMC is better overall -
Sanny has a game debugger and memory tools that DMC does not have.

Legend: YES = implemented and unit-tested here | PART = partly | NO = not in 2.0 | UNTESTED = written but never run on Windows

## Language
| Sanny feature | DMC 2.0 |
|---|---|
| Comments `//` and `/* */` | YES |
| Directives `{$CLEO}`, `{$INCLUDE}`, `{$INCLUDE_ONCE}`, `{$USE}` (accepted) | YES (`{$USE}` is accepted, not acted on) |
| Other directives (`{$NOSOURCE}`, `{$VERSION}` ...) | PART (ignored) |
| Constants (`const`) | YES |
| Variables: `0@`, `$12`, named `int/float/Class x`, `var` blocks | YES (named globals need `{$GLOBALS n}`) |
| String variables (`@s`, `@v`) | PART (PC targets only, UNTESTED in game) |
| Arrays (`x[i]`, `5@(6@,10i)`) | PART (encoding UNTESTED in game) |
| Class syntax `Class.Member(args)`, outputs on the left, `not` | YES |
| Instance calls `ped.IsDead()` | YES |
| Assignment, `+= -= *= /=`, `=#`, timed `+=@` | YES |
| Bitwise / modulo operators | PART (works if the loaded library defines them) |
| Expressions `a = b + c` | PART (one operator) |
| Conditions `if / and / or / not`, `if ... then ... else ... end` | YES |
| Loops `while`, `repeat-until`, `for` (+`step`, `downto`), `break`, `continue` | YES |
| `switch / case` | NO |
| Functions (`function`, return values) and Import/Export | NO (use `gosub`/`return`) |
| Enums (`Enums.txt`) and model names `#NAME` | NO (numbers only; clear error message) |
| `hex ... end` | YES |
| Labels, `gosub`, `jump`, mission/external script labels | YES for normal labels; mission/external NO |
| Built-in commands (Alloc/Free etc.) | NO |

## Edit modes / output
| Sanny feature | DMC 2.0 |
|---|---|
| SA mobile (Android) mode | YES (verified byte rules, see VERIFIED.md) |
| SA PC with CLEO 4 / CLEO 5 | PART (compiles; never run in the game) |
| GTA III / Vice City / LCS / VCS modes | NO (float and header formats differ; cannot be verified here) |
| Library JSON from Sanny Builder Library, extensions on/off | YES (loader tested on a synthetic file; real files: UNTESTED) |
| main.scm / script.img modes, missions, external scripts | NO (DMC targets CLEO scripts only) |
| CustomLabels.ini, SCM.INI opcode list, Code Templates | NO (DMC has JSON snippets instead) |
| Custom CLEO headers | NO (not found in a released CLEO) |

## Editor
| Sanny feature | DMC 2.0 |
|---|---|
| Syntax highlighting, themes, color editor | YES (3 themes, per-token colors) UNTESTED on Windows |
| Autocomplete, parameter hints, hover docs, opcode search | YES UNTESTED on Windows |
| Language service (diagnostics while typing) | YES (live check, squiggles, problems list) |
| Find / replace, find in files | YES |
| Go to definition, references, rename | YES (labels, named variables/constants) |
| Code folding, minimap, bracket matching | YES UNTESTED on Windows |
| Tabs, session restore, drag & drop | YES UNTESTED on Windows |
| Disassembler / decompiler | YES (self-verifying; byte round trip) |
| Hex view of output | YES |
| CLI | YES (`DMC script.txt -o out`, `--decompile`, `--disasm`) |
| Hotkey editor | NO (fixed shortcuts) |
| SCM debugger, debug options, memory/variable watch, run-game actions | NO |

## Extras in DMC
One-click ADB push (Android) / copy to game (PC), "did you mean" suggestions, all errors at once, per-variable
type inspector (hover + Outline), auto-format, snippets with Tab expansion, compile-all-tabs, library download with
progress and friendly offline errors, build script that leaves the project folder clean.
