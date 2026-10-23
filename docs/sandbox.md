# Sandbox

C/ programs run inside a sandbox. The SDK checks every path a directive touches, and refuses anything outside the folders the program was given.

## What is allowed

| Action | Allowed in |
|--------|------------|
| Reading files | The folders in `pr.cache` (and `dir/default`), and the folder the program itself is in |
| Creating or changing files | The folders in `pr.cache` (and `dir/default`) only |
| Network access | Never. C/ has no network directives |

The folder that contains the program is read-only. A program cannot write next to itself unless that folder is also listed in `pr.cache`.

## What happens when a path is refused

The program stops with an error:

```text
error: blocked by sandbox: /tmp/x.txt
```

The message shows the real path that was refused.

To allow the path, add the folder to your `pr.cache` (see [Runtimes and pr.cache](pr-cache.md)), or change the program so it stays inside the folders it has.

## How the check works

1. The path from your program is turned into a full path.
2. The full path is cleaned up: `.` and `..` are resolved, and symbolic links are followed.
3. The cleaned path must be inside one of the allowed folders.

Because the check uses the cleaned path, tricks such as `dir/default/../../escape.txt` do not work. They point outside the folder, so they are refused:

```text
#create: "x.txt", to: dir/default/../..;
```

```text
error: blocked by sandbox: /tmp/x.txt
```

## Other limits

- `#run`, `#include` and `#import` can nest at most 32 levels. A file that includes itself stops with an error.
- `#import` and `#convert` do not execute foreign code. A `.js`, `.java` or `.cs` file is recorded, not run (see [Limitations](limitations.md)).
- Every directive only works with files. The language has no way to run other programs.
