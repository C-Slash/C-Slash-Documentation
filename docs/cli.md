# Command line

The SDK is one program, `cslash`.

```text
cslash run <file.cslash> [--cache <pr.cache>] [--strict]
cslash check <file.cslash>
cslash version
cslash help
```

## cslash run

Runs a C/ program.

```text
cslash run hello.cslash
```

Options:

| Option | Meaning |
|--------|---------|
| `--cache <file>` | Load this `pr.cache` before the program starts. See [Runtimes and pr.cache](pr-cache.md#loading-a-cache) |
| `--strict` | Turn warnings into errors: a missing `#import` file or a missing `pr.cache` stops the program |

Without `--strict`, a missing import is only a warning:

```text
warning: import not found, skipped: pr.txt
after
```

With `--strict`:

```text
error: import not found: pr.txt
```

## cslash check

Checks that a file is valid C/ without running it.

```text
cslash check hello.cslash
```

Output when it is valid:

```text
ok: hello.cslash
```

Output when it is not (or the file cannot be found):

```text
invalid: bad.cslash
```

`check` only reads the file. To see what is wrong with a file, run it, and the error message gives the line.

## cslash version

```text
cslash 1.0.0
```

## cslash help

Prints the usage text. Running `cslash` with no arguments also prints it, with exit status 2.

## Where output goes

- Program output (`#print`) and progress notes starting with `[cslash]` go to standard output.
- Warnings and errors go to standard error.

## Exit codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | The program failed, or `check` found an invalid file |
| 2 | The command was used incorrectly (unknown command or option) |

You can use these in scripts:

```bash
cslash check build.cslash && cslash run build.cslash
```
