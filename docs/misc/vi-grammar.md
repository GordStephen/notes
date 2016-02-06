# Composable Vim Elements

_Or, how to [grok vi](http://stackoverflow.com/a/1220118/2624652)._

The depth and breadth of functionality in Vim (or evil-mode in Emacs) can be daunting at first glance, but breaking normal mode edit commands down into orthogonal, composable elements (movements, operators, text objects, and counts) reveals a simple and intuitive syntax structure. `ex` functionality in command-line mode (i.e. `:` commands) represents a whole other world that isn't covered here.

## Orthogonal elements
These lists are by no means exhaustive, but when taken together they can provide very efficient (if not the most efficient) means of achieving frequently-required functionality. 

### Movements

#### Move to:

- Beginning of document - `gg`
- End of document - `G`

- __T__op of screen - `T`
- __M__iddle of screen - `M`
- Bottom (__L__ow) of screen - `L`

 - The mark named 'a' - ```a``
 - The beginning of the line containing the mark named 'a' - `'a`

#### Relative to the cursor, move to:

 - First character on the line - `0`
 - Previous character on the line - `h`
 - Next character on the line - `l`
 - Last character on the line - `$`

 - Previous line - `k`
 - Next line - `j`

 - Next beginning of word - `w`

|                           | ...word | ...sentence | ...paragraph  |
|---------------------------|---------|-------------|---------------|
| Previous beginning of...  | `b`     | `(`         | `{`           |
| Next end of...            | `e`     | `)`         | `}`           |

 - Previous in-line search hit for character 's' - Fs
 - Next in-line search hit for character 's' - Fs

 - Character after previous in-line search hit for character 's' - Ts
 - Character before next in-line search hit for character 's' - Ts

 - Previous in-document search hit for "searchterm" - `?searchterm`
 - Next in-document search hit for "searchterm" - `\searchterm`

### Operators

Most operators need something to operate on: either the space between the cursor and the destination of a movement (see above), or a text object (see below).

| Action       	    	      	 	  | Operator symbol |
|-----------------------------------------|-----|
| Change (delete + insert something new)  | `c` |
| Delete (cut to register)                | `d` |
| Yank (copy to register)                 | `y` |
| Put (paste from register)               | `p` |
| Indent                                  | `>` |
| Reverse Indent                          | `<` |

### Text objects

A text object represents the innermost meaningful grouping of document text that contains the cursor, pre-qualified with either `a` ("__a__...", "__all__ the...") or `i` ("__i__nside"):

| | select __a__... | select __i__nside a... |
|-|-----------------|------------------------|
| __w__ord | `aw` | `iw` |
| __s__entence | `as` | `is` |
| __p__aragraph | `ap` | `ip` |
| HTML/XML __t__ag | `at` | `it` |
| __(__...__)__ | `a(`, `a)` | `i(`, `i)` |
| __[__...__]__ | `a[`, `a]` | `i[`, `i]` |
| __{__...__}__ | `a{`, `a}` | `i{`, `i}` |
| __<__...__>__ | `a<`, `a>` | `i<`, `i>` |
| __"__...__"__ | `a"` | `i"` |
| __'__...__'__ | `a'` | `i'` |
| __\`__...__\`__ | ``a` `` | ``i` `` |

### Counts
Operators, text objects, and many movements can be prefixed by integers to indicate that the operation/movement should occur a certain number of times, or the n-th smallest surrounding text object should be selected.

## Composing orthogonal elements 

### Operator + movement
Apply an operator between the cursor and a movement destination: `{operator}{movement}`

For example:

 - Delete this line and the previous: `dk`
 - Change text from the cursor to the end of the word: `ce` (`cw` does this as a special case too)
 - Yank from here to the next letter 'q': `yfq`
 - Delete the next 3 characters: `d3l`

### Operator + text object
Apply an operator to a text object: `{operator}{textobject}`

For example:

 - Delete the current word: `daw`
 - Yank the current sentence: `yas`
 - Indent the current paragraph: `>ap`
 - Yank the contents of the third-innermost set of brackets surrounding the cursor: `y3i(`

