# Language reference

This page describes the syntax of C/ as implemented by SDK v1.0.0. For what each directive does, see [Directives](directives.md).

## Files

C/ source files are plain text and end in `.cslash`. Save them as UTF-8 without a byte order mark (BOM). Programs run from top to bottom.

## Statements

A statement is a directive followed by its arguments. It starts with `#` and a name, and ends with a semicolon:

```text
#print: "hello";
```

Write the semicolon on every statement. The SDK is lenient and also ends a statement at the next `#directive` or at the end of the file, but the language specification requires the semicolon.

Directive names are lowercase and case-sensitive. The colon after the name (`#print:`) is part of the normal style.

## Arguments

After the directive name come its arguments, separated by commas.

```text
#create: "a.txt", "b.txt";
```

An argument is either a quoted string or a bare word. A bare word is any run of characters that does not contain a space, a comma, a semicolon, an equals sign or a quote. Paths such as `dir/default/app.js` are bare words.

## Parameters

A `key: value` pair sets a named parameter:

```text
#create: "notes.txt", to: dir/default;
```

Common parameters are `to:` (a target folder or format) and `from:` (a cache file to load). See each directive for the parameters it understands.

## Continuation lines

A line that starts with `key:` instead of `#directive` continues the directive before it. This lets you put long parameters on their own line:

```text
#create: "notes.txt", to: dir/default;
from: "pr.cache";
```

Here `from:` belongs to `#create`. A continuation line always belongs to the most recent directive at the top level of the program. A line starting with `key:` with no directive before it is an error.

## Strings

Strings are wrapped in single or double quotes. The two kinds are interchangeable, so you can put one kind inside the other without escape characters:

```text
#print: 'hello world!, my name is "Guy"';
#print: "it's fine";
```

- There are no escape sequences. A backslash is just a backslash.
- A string cannot contain both kinds of quote.
- A string can span several lines.
- A string with no closing quote is an error.

## Comments

A comment starts with `*` followed by `//` and runs to the end of the line:

```text
* // This is a comment.
#print: "visible";
```

A `*` that is not followed by `//` is an error.

## Conditions

`#if` uses `left = right` to build a check:

```text
#if: "notes.txt" = dir/default, then: print "found";
```

The only operator is `=`. See [`#if` and `then:`](directives.md#if-and-then) for the forms it accepts.

## Actions: then: and comma chaining

An action is a directive that runs after another directive, usually after an `#if`. There are two ways to write one.

With `then:`:

```text
#if: "notes.txt" = dir/default,
    then: print "found";
```

The action after `then:` can be written with or without the `#` and colon: `then: print "found"` and `then: #print: "found"` mean the same thing.

By chaining a directive after a comma:

```text
#if: "notes.txt" = dir/default,
#print: "found";
```

Actions can be chained further:

```text
#run: "build.cslash", then: print "build finished";
```

A line that begins with `then:` continues the previous directive, in the same way as `from:`.

An action after `#if` runs only when the check is true. After any other directive, the action runs once that directive has finished.

## Order and errors

Statements run from top to bottom. A directive that fails stops the program, and the SDK exits with status 1. Warnings (such as a missing `#import` file) do not stop the program unless you run with `--strict`.

`#run`, `#include` and `#import` can nest, up to 32 levels deep. A file that includes itself stops with a "nesting too deep" error instead of running forever.

## Summary of the grammar

```text
program     = { statement }
statement   = directive | continuation
directive   = "#" name ":" [ items ] ";"
continuation = key ":" value { "," item } ";"
items       = item { "," item }
item        = value | key ":" value | value "=" value | "then:" action
action      = [ "#" ] name [ ":" ] [ items ]
value       = string | bare-word
string      = "'" text "'" | '"' text '"'
comment     = "*" "//" text end-of-line
```

This grammar is a summary, not a formal definition.
