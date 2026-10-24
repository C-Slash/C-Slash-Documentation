# Runtimes and pr.cache

## The two runtimes

C/ programs run on a **Runtime**. There are two ways to run one:

- **The normal Runtime** has `dir/default` already configured. It is set to your Downloads folder.
- **A Private Runtime (PR)** is a self-hosted version of the Runtime. It does not guess where to work: you give it a `pr.cache` file that defines its folders.

The full Runtime is not public. The `cslash` command in this SDK supports both modes. It is in PR mode as soon as a `pr.cache` is loaded.

File checks (`#if`) look at local files in both modes.

## Default folder

Without a `pr.cache`, `dir/default` is the Downloads folder of the current user:

| System | `dir/default` |
|--------|----------------|
| Windows | `C:\Users\<you>\Downloads` |
| Android | `/storage/emulated/0/Download` |
| Linux and macOS | `~/Downloads` |

## The pr.cache file

`pr.cache` maps folder names to real paths on your machine. Linux example:

```text
{
dir/default = /home/C-Slash/Downloads
}
```

Android example:

```text
{
dir/default = storage/emulated/0/Download
}
```

Windows example:

```text
{
dir/default = C:/Users/you/Downloads
}
```

Rules:

- The whole file is wrapped in braces `{` and `}`.
- Each entry is `dir/NAME = path`. The name must start with `dir/`.
- The path may be wrapped in single or double quotes. This is optional.
- Comment lines start with `* //`. Blank lines are ignored.
- A relative path is measured from the folder that contains the `pr.cache` file. `dir/default = work` means a folder called `work` next to the cache.
- On Android, a path that starts with `storage/` is treated as starting at the root (`/storage/...`).
- You can define as many folders as you like: `dir/projects = /home/you/projects`. Use them as `dir/projects/file.txt`.
- A cache entry replaces the default for the same name, so `dir/default` in a cache replaces the Downloads folder.

A problem in the file is an error, such as `pr.cache: expected 'name = path'`.

## Loading a cache

There are two ways:

**In the program.** Add `from: "pr.cache"` to any directive. It is loaded just before that directive runs:

```text
#create: "notes.txt", to: dir/default;
from: "pr.cache";
```

The SDK looks for the file next to the program first, then in the folder you ran the command from. A cache is loaded only once, however many directives name it. If the file cannot be found, you get a warning and the defaults are used. With `--strict` it is an error.

**On the command line.** Pass the file with `--cache`:

```text
cslash run program.cslash --cache pr.cache
```

The cache is then loaded before the program starts.

## How paths are resolved

A path written in a C/ program is turned into a real path like this:

| You write | Meaning |
|-----------|---------|
| `dir/NAME` or `dir/NAME/rest` | The folder `NAME` from the cache, plus `rest` |
| A full path such as `/home/you/a.txt` | That exact path |
| A plain name such as `lib.cslash` when reading | Next to the program, if it exists there; otherwise in `dir/default` |
| A plain name such as `notes.txt` when writing | In `dir/default` |

After a path is resolved, the sandbox checks it. See [Sandbox](sandbox.md). Using a name with `dir/` that is not defined gives `unknown directory: dir/NAME`.
