# CppKaiConsoleLib

The KAI Console: the REPL engine behind KAI's Console app and kaish. It runs Pi,
Rho and shell commands over one Executor, with history, colour output and, when
built with `KAI_USE_ENET`, a network console.

Builds the `ConsoleLib` library.

| Path | What |
|---|---|
| `Include/KAI/Console/Console.h` | the `Console` class |
| `Include/KAI/Console.h` | convenience include |
| `Source/Console.cpp` | implementation |

## Dependencies

- [CppKaiLanguage](https://github.com/cschladetsch/CppKaiLanguage): PiLang, RhoLang
- [CppKaiCore](https://github.com/cschladetsch/CppKaiCore): Core, CommonLang, Executor, `KAI/Network/Transport.h`,
  and the terminal colours (`KAI/Console/ConsoleColor.h`, `rang.hpp`), which Core uses too

As part of a parent project (CppKAI, CppKaiShell) those targets already exist.
Standalone:

```
cmake -S . -B build -DKAI_LANGUAGE_DIR=../CppKaiLanguage -DKAI_CORE_DIR=../CppKaiCore
cmake --build build
```

Extracted from CppKaiCore with its history.