# Limitations

What SDK v1.0.0 does not do yet. These are known gaps, not bugs.

## Foreign code is recorded, not run

C/ is designed to bring code from JavaScript, TypeScript, Java, C, C++ and C# into one program. In v1.0.0 the SDK recognizes these files by their extension, but it does not run or translate them:

- `#import` of a non-C/ file only records it and prints `[cslash] imported: <path>`.
- `#convert` writes a wrapper that imports the original file. It does not turn the code into C/.

## No variables, functions or loops

C/ v1.0.0 has directives only. There are no variables, functions, loops or arithmetic, so the only decision-making is `#if` with `then:`.

## #if is limited

- The only operator is `=`.
- There is no `else`. To handle the false case, write a second `#if` that checks for the opposite.
- The two checks are "file exists in a folder" and "file is valid C/".

## Grammar extensions are not applied

The language design allows a file such as `pr.txt` to change the lexer's rules (for example to teach it a new kind of string). In v1.0.0:

- `#import: "pr.txt";` is accepted and recorded, but its content is not applied.
- Both single and double quotes are always accepted.

## #include runs statements

`#include` runs the included file's statements at that point. It does not paste raw text into the file before it is read.

## Small details

- `#byte_generate` can only make a PNG (a fixed 32 by 32 dark-green image) or an empty file.
- A string cannot contain both kinds of quote, because there are no escape sequences.
- `#convert` needs its source file to be inside a configured folder.
