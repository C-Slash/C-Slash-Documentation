# Getting started

This tutorial builds up from one line to a small multi-file program. Every step uses a plain-text file ending in `.cslash`.

Make a folder called `tutorial`, and work inside it. Create each file with any plain-text editor and save it as plain text (UTF-8 without a BOM). Then run it with:

```text
cslash run <file>
```

If you built from source and did not add `cslash` to your `PATH`, give the full path instead, for example `../C-Slash/build/cslash run hello.cslash`.

## 1. Hello

`hello.cslash`

```text
#print: "Hello from C/";
```

Run it:

```text
cslash run hello.cslash
```

Output:

```text
Hello from C/
```

A C/ program is a list of directives. Each starts with `#` and ends with `;`.

## 2. Quotes, words and comments

`words.cslash`

```text
* // This line is a comment and is ignored.
#print: "double quotes";
#print: 'single quotes, so I can say "hi" inside';
#print: "one", "line", "of", "words";
```

Output:

```text
double quotes
single quotes, so I can say "hi" inside
one line of words
```

- Single and double quotes are interchangeable. Use whichever one is not inside your text.
- Comments start with `* //` and run to the end of the line.
- Several arguments separated by commas are printed on one line, separated by spaces.

## 3. Choose a working folder

Programs that create files need a folder to work in. You choose it in a file called `pr.cache`.

`pr.cache`

```text
{
dir/default = work
}
```

This says that `dir/default` is a folder called `work`, next to `pr.cache`. You do not need to create it.

`files.cslash`

```text
#create: "notes.txt", to: dir/default;
from: "pr.cache";
#print: "created a file";
```

Output (your paths will differ):

```text
[cslash] loaded cache: /home/you/tutorial/pr.cache
[cslash] created: /home/you/tutorial/work/notes.txt
created a file
```

`from: "pr.cache"` on the second line belongs to `#create`. It tells the SDK which cache to load. Run the program again and `created` becomes `exists`, because `#create` never overwrites a file.

## 4. Decisions with #if and then:

`check.cslash`

```text
#if: "notes.txt" = dir/default,
from: "pr.cache";
    then: print "notes.txt exists";
#if: "missing.txt" = dir/default,
    then: print "this will not print";
#print: "done";
```

Output:

```text
[cslash] loaded cache: /home/you/tutorial/pr.cache
notes.txt exists
done
```

`#if: "notes.txt" = dir/default` is true when a file called `notes.txt` is in that folder. The `then:` action only runs when the check is true.

## 5. Chaining with a comma

Instead of `then:`, you can end the `#if` line with a comma and put another directive straight after it.

`chain.cslash`

```text
#if: "notes.txt" = dir/default,
#print: "the chained directive runs only when the check is true";
from: "pr.cache";
```

Output:

```text
[cslash] loaded cache: /home/you/tutorial/pr.cache
the chained directive runs only when the check is true
```

## 6. Split a program into files

`library.cslash`

```text
#print: "library loaded";
```

`main.cslash`

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

`#import` loads a file only once, even though the line appears twice. If you want the file to run every time, use `#include` instead.

## 7. Convert a file, then run it

`convert.cslash`

```text
#create: "app.js", to: dir/default;
from: "pr.cache";
#convert: dir/default/app.js, to: cslash;
#run: "dir/default/app.cslash";
```

Output:

```text
[cslash] loaded cache: /home/you/tutorial/pr.cache
[cslash] created: /home/you/tutorial/work/app.js
[cslash] converted: /home/you/tutorial/work/app.js -> /home/you/tutorial/work/app.cslash
[cslash] running: /home/you/tutorial/work/app.cslash
[cslash] imported: /home/you/tutorial/work/app.js
```

`#convert` wrote `work/app.cslash`, a short C/ file that records the source language and imports the original. `#run` checked that it was valid C/ and ran it. Open `work/app.cslash` to see it:

```text
* // converted by cslash from app.js (JavaScript)
* // language: JavaScript
#import: "dir/default/app.js";
```

## 8. Generate a file

`image.cslash`

```text
#byte_generate: "badge.png", to: dir/default;
from: "pr.cache";
```

Output:

```text
[cslash] loaded cache: /home/you/tutorial/pr.cache
[cslash] generated: /home/you/tutorial/work/badge.png
```

This writes a real 32 by 32 PNG image into `work`.

## Next steps

- [Language reference](language-reference.md) for the exact syntax rules
- [Directives](directives.md) for every directive in detail
- [Runtimes and pr.cache](pr-cache.md) for more about folders
- [Troubleshooting](troubleshooting.md) if something goes wrong
