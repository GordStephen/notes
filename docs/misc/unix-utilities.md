# Useful Unix Utility / Shell Scripting Syntax

## Pipes and redirection

`<` directs the contents of a file into a program as `stdin`.

`|` pipes data from one program to another, forming a pipeline.

`>` (or `1>`) will redirect a pipeline's `stdout` to overwrite a given file location.

`>>` will redirect `stdout` to append to a given file location.

`2>` will redirect a pipleline's `stderr`.

`&>` will redirect both `stdout` *and* `stderr` to the same file location.

## `read`

## `grep`

## `sed`

## `awk`

## Perl

With the help of specific command line flags, perl one-liners can reproduce and extend the functionality of `sed` or `awk`.

Perl "micro-scripts" can be particularily useful for easily combining find/replace operations using regular expressions with math operations. For example, to convert 30 fps video timestamps from a frame-based format to one based on milliseconds:

    perl -pe 's/(\d\d);(\d\d);(\d\d);(\d\d)/"$1:$2:$3,".(sprintf "%03d", $4 * 1000 \/ 30)/ge'

`00;00;03;25` is converted into `00:00:03,833`. `-p` runs the operation repeatedly on each line piped to `stdin` and sends the result to `stdout` (like `sed` or `awk` would).

