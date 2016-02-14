% Useful Unix Utility / Shell Scripting Syntax

## Pipes and redirection

`<` directs the contents of a file into a program as `stdin`.

`|` pipes data from one program to another, forming a pipeline.

`>` (or `1>`) will redirect a pipeline's `stdout` to overwrite a given file location.

`>>` will redirect `stdout` to append to a given file location.

`2>` will redirect a pipleline's `stderr`.

`&>` will redirect both `stdout` *and* `stderr` to the same file location.

## `grep`

Searches for a given pattern line-by-line in an input file or `stdin` stream and prints matching lines to `stdout`. Useful as a filter operation in a pipeline.

## `sed`

Performs a line-by-line regex-based find/replace on an input file or `stdin` stream and prints output to `stdout`. Useful as a mapping operation in a pipeline. `-i` enables in-place find/replace on a file.

## `awk`

Automatically parses line-by-line file or `stdin` stream input with delimiters (eg a CSV file) into multiple input variables, which can be manipulated with and output to `stdout`. Useful in pipelines as a mapping operation over tabular/record-based data.

## Perl

With the help of specific command line flags, Perl one-liners can reproduce and extend the functionality of `grep`, `sed`, or `awk`.

Perl "micro-scripts" can be particularily useful for easily combining find/replace operations using regular expressions with math operations. For example, to convert 30 fps video timestamps from a frame-based format to one based on milliseconds:

    perl -pe 's/(\d\d);(\d\d);(\d\d);(\d\d)/"$1:$2:$3,".(sprintf "%03d", $4 * 1000 \/ 30)/ge'

`00;00;03;25` is converted into `00:00:03,833`. `-p` runs the operation repeatedly on each line piped to `stdin` and sends the result to `stdout` (like `sed` or `awk` would).

## `read`

Reads a line from `stdin`. Mainly useful in shell scripts for accepting interactive input or piping stream output into a while-loop for more sophisticated processing. 

## `sort`

Concatenates and sorts the input stream (or files) and returns the results to `stdout`. Can take a delimeter and sort on specified columns of tabular data.

## `uniq`

Eliminates *adjacent* duplicate lines from input stream (or file) and returns results to output stream (or file). Can also prepend the count of adjacent lines (useful for "histogramming" categorical data in combination with `sort`).

## `cut`

Removes subsets of lines passed in through a file or stream. Useful for filtering out unnessecary columns from tabular datasets.
