# Troubleshooting

Find your message below. In messages with a line number, the line is where the problem is in your `.cslash` file.

## Errors

| Message | Cause | Fix |
|---------|-------|-----|
| `error: cannot read file: <path>` | The file does not exist at that path | Check the spelling, and run the command from the folder that holds the file, or give the full path |
| `syntax error (line N): <file>: unknown directive 'x'` | The directive name is misspelled, is not capitalized correctly, or your SDK build is older than the docs | Directive names are lowercase. Update the SDK if the directive is in [Directives](directives.md) |
| `stray '*' (comments are written '* //')` | A comment line starts with `*` but not `* //` | Write comments as `* // text` |
| `unterminated string` | A quote was opened and never closed | Add the closing quote |
| `'<name>:' has no directive to continue` (for example `'from:' has no directive to continue`) | A line starts with `from:`, `to:` or `then:` but there is no directive before it | Put the parameter after a directive. If the name in the message looks like a normal directive, such as `'#print:'`, see "Windows: a correct file fails on line 1" below |
| `#print needs some text` (and similar `needs` messages) | The directive was given no arguments | Add the missing argument |
| `#convert supports only 'to: cslash'` | `to:` is missing or names another format | Use `to: cslash` |
| `error: blocked by sandbox: <path>` | The path is outside the allowed folders | Add the folder to `pr.cache`, or use a path inside `dir/default`. See [Sandbox](sandbox.md) |
| `error: unknown directory: dir/NAME` | The name is not defined | Add `dir/NAME = path` to `pr.cache`, or use `dir/default` |
| `error: convert: file not found: <path>` | The file to convert does not exist | Check the path |
| `error: convert: already a .cslash file: <path>` | The source is already C/ | Use a file with another extension |
| `error: run: not a .cslash file: <path>` | `#run` was given a file with another extension | Only `.cslash` files can be run |
| `error: run: file is missing or invalid: <path>` | The file does not exist, or is not valid C/ | Run `cslash check <file>` to test it |
| `error: nesting too deep (possible #run/#include loop)` | A file runs itself, or the chain is more than 32 levels deep | Break the loop |
| `error: import not found: <name>` | `--strict` is on and the imported file is missing | Create the file, or remove `--strict` |
| `error: pr.cache not found: <name>` | `--strict` is on and the cache was not found | Put the cache next to the program or in the folder you run from |
| `error: cannot read pr.cache: <path>` | The cache file exists but cannot be read | Check its permissions |
| `pr.cache: expected 'name = path'` | A cache line has no `=` | Write each entry as `dir/NAME = path` |
| `pr.cache: key must look like dir/NAME` | An entry name does not start with `dir/` | Rename it, for example `dir/default` |
| `pr.cache: empty path` | A cache entry has nothing after `=` | Give it a path |

## Warnings

Warnings do not stop the program.

| Message | Meaning |
|---------|---------|
| `warning: import not found, skipped: <name>` | A file named in `#import` does not exist, so it was skipped |
| `warning: cache not found, using defaults: <name>` | The cache named by `from:` was not found next to the program or in the current folder, so `dir/default` is still the Downloads folder |

## Platform problems

**Windows: "Unknown publisher" or SmartScreen warning.** The program is unsigned. Choose **More info**, then **Run anyway**.

**Windows: a correct file fails on line 1.** The error looks like `syntax error (line 1): hello.cslash: '#print:' has no directive to continue`, even though the first line is right. The file starts with an invisible byte order mark (BOM), which some editors and PowerShell commands add when they save "UTF-8". The SDK does not accept a BOM. Save the file as "UTF-8" without BOM (in Notepad, choose **UTF-8** and not "UTF-8 with BOM"), or create it with `Set-Content` rather than `>` or `Out-File`.

**Android (Termux): cannot write to Downloads.** Run `termux-setup-storage` once and allow access. If it still fails, point `dir/default` at a folder inside Termux (for example `dir/default = work`) in a `pr.cache`.

**Files appear in the wrong place.** Without a `pr.cache`, files go to your Downloads folder. Use a `pr.cache` to choose another folder, and look at the `[cslash] created:` lines to see the exact paths.

**`cslash: command not found`.** The program is not on your `PATH`. Run it by its full path (for example `./build/cslash`) or see [Installation](installation.md#running-the-program-from-anywhere-optional).
