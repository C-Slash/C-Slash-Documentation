# C-Slash Documentation

Documentation for **C/** (C-slash), a minimalist, security-focused orchestration language, and for the **C-Slash SDK**, the command-line tool that runs C/ programs.

- Language specification and examples: [CSlash](https://github.com/C-Slash/CSlash)
- SDK and runtime: [C-Slash](https://github.com/C-Slash/C-Slash)

This documentation covers **SDK v1.0.0**.

```text
#print: "Hello from C/";
```

## Contents

| Page | What's in it |
|------|--------------|
| [Installation](docs/installation.md) | Install, update and uninstall on Windows, Android (Termux), Linux and macOS |
| [Getting started](docs/getting-started.md) | A step-by-step tutorial, from "Hello" to converting files |
| [Language reference](docs/language-reference.md) | Syntax: statements, strings, comments, chaining, conditions |
| [Directives](docs/directives.md) | Every directive in detail, with examples and output |
| [Runtimes and pr.cache](docs/pr-cache.md) | The Private Runtime, the `pr.cache` file and how paths are resolved |
| [Command line](docs/cli.md) | The `cslash` command, options and exit codes |
| [Sandbox](docs/sandbox.md) | What C/ programs are allowed to touch |
| [Troubleshooting](docs/troubleshooting.md) | Every error message and what to do about it |
| [Limitations](docs/limitations.md) | What v1.0.0 does not do yet |

## What is C/?

C/ is a small orchestration language. A C/ program is a list of directives, each starting with `#` and ending with `;`. Directives create files, check whether files exist, convert files, import other files and run them. The native core is deliberately bare-minimum, which keeps it fast and keeps its behaviour easy to reason about.

All file access happens inside a sandbox. A C/ program can only touch the folders it has been given, and it has no network access.

## License

See the `LICENSE` file in this repository.
