# Directives

Every directive in SDK v1.0.0.

| Directive | What it does |
|-----------|--------------|
| [`#import`](#import) | Load a file once |
| [`#include`](#include) | Run a file's statements every time |
| [`#create`](#create) | Create empty files |
| [`#convert`](#convert) | Make a C/ wrapper around another file |
| [`#if` and `then:`](#if-and-then) | Run an action only if a check is true |
| [`#run`](#run) | Run a valid `.cslash` file |
| [`#byte_generate`](#byte_generate) | Write a binary file such as a PNG |
| [`#print`](#print) | Print text |

Messages that start with `[cslash]` are progress notes. Warnings and errors are explained in [Troubleshooting](troubleshooting.md).

Paths in directives follow the rules in [Runtimes and pr.cache](pr-cache.md#how-paths-are-resolved): `dir/NAME/file` points inside a configured folder, and a plain name is looked up next to the script, then in `dir/default`.

## #import

```text
#import: "file" [, "file2" ...];
```

Loads each file one time per run. Importing the same file again does nothing.

- A `.cslash` file runs at that point.
- Any other file (for example `pr.txt`) is recorded, and the SDK prints `[cslash] imported: <path>`.
- A file that cannot be found prints a warning and is skipped. With `--strict` it is an error.

Example:

```text
#import: "library.cslash";
#import: "library.cslash";
#print: "main";
```

Output:

```text
library loaded
main
```

(if `library.cslash` prints `library loaded`.)

## #include

```text
#include: "file" [, "file2" ...];
```

Runs the statements of each file at that point, every time. The file must contain C/. Unlike `#import`, a repeated `#include` runs the file again.

```text
#include: "library.cslash";
#include: "library.cslash";
```

Output:

```text
library loaded
library loaded
```

A missing file is always an error.

## #create

```text
#create: "name" [, "name2" ...] [, to: folder];
```

Creates empty files. `to:` names the folder, for example `to: dir/default`. Without `to:`, the files are created in `dir/default`. Missing folders are created as needed, and a name can include subfolders such as `"logs/today.txt"`.

An existing file is left untouched:

```text
[cslash] created: /home/you/tutorial/work/notes.txt
[cslash] exists: /home/you/tutorial/work/notes.txt
```

The destination must be inside a configured folder. See [the sandbox](sandbox.md).

## #convert

```text
#convert: path, to: cslash;
```

Writes a C/ wrapper file next to the source file, with the same name and the extension `.cslash`. Only `to: cslash` is supported.

```text
#convert: dir/default/app.js, to: cslash;
```

creates `app.cslash` containing:

```text
* // converted by cslash from app.js (JavaScript)
* // language: JavaScript
#import: "dir/default/app.js";
```

The wrapper records the language and imports the original file. It does not translate the original code into C/ (see [Limitations](limitations.md)).

The language is recognized from the file extension:

| Extensions | Language |
|------------|----------|
| `.js` | JavaScript |
| `.ts` | TypeScript |
| `.java` | Java |
| `.class` | Java bytecode |
| `.jar` | Java archive |
| `.c` | C |
| `.cpp`, `.hpp` | C++ |
| `.cs`, `.csx`, `.csproj`, `.sln`, `.slnx` | C# |
| anything else | data |

Rules:

- The source file must exist and must not already be a `.cslash` file.
- The source must be inside a configured folder such as `dir/default`, because the wrapper is written next to it.
- An existing wrapper with the same name is overwritten.

## #if and then:

```text
#if: left = right, then: action;
#if: left = right, #directive ...;
```

Runs the action only when the check is true. If the check is false, the action is skipped and the program continues with the next statement.

There are two kinds of check.

**File exists in a folder.** `left` is a file name and `right` is a folder:

```text
#if: "notes.txt" = dir/default, then: print "found";
```

This is true when `notes.txt` is in `dir/default`. If `right` is a full file path instead of a folder, the check is true when that file exists and its name matches `left`:

```text
#if: "notes.txt" = dir/default/notes.txt, then: print "found";
```

Only the file name part of `left` is compared. `"sub/notes.txt"` is treated like `"notes.txt"`.

**File is valid C/.** When `right` is the word `valid`, `left` is a path to a `.cslash` file:

```text
#if dir/default/app.cslash = valid,
    then: print "wrapper is valid C/";
```

This is true when the file exists and parses as correct C/. The file is checked, not run.

Both kinds work with or without a `pr.cache`. See [Language reference](language-reference.md#actions-then-and-comma-chaining) for the ways to write the action.

## #run

```text
#run: "file.cslash" [, then: action];
```

Runs a C/ file. The file must end in `.cslash` and must parse as valid C/, or the SDK stops with an error. It prints `[cslash] running: <path>` and runs the file's statements, then continues with the action if there is one.

```text
#run: "dir/default/app.cslash";
```

## #byte_generate

```text
#byte_generate: "name" [, "name2" ...] [, to: folder];
```

Writes a binary file. A name ending in `.png` becomes a real 32 by 32 PNG image filled with dark green. Any other name becomes an empty file. Folders are handled as in `#create`, but existing files are overwritten.

```text
#byte_generate: "badge.png", to: dir/default;
```

## #print

```text
#print: "text" [, "more text" ...];
```

Prints its arguments on one line, separated by single spaces. `#print` needs at least one argument. Use single quotes around text that contains double quotes:

```text
#print: "Hello from C/";
#print: 'hello world!, my name is "Guy"';
```

Output:

```text
Hello from C/
hello world!, my name is "Guy"
```

## Parameters used by all directives

`from: "file"` on any directive loads a `pr.cache` before the directive runs. See [Runtimes and pr.cache](pr-cache.md#loading-a-cache).
