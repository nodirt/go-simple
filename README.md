Gosimple is a linter for Go source code that specialises on
simplifying code.

## Installation

Gosimple requires Go 1.6 or later.

    go get honnef.co/go/simple/cmd/gosimple

## Usage

Invoke `gosimple` with one or more filenames, a directory, or a package named
by its import path. Gosimple uses the same
[import path syntax](https://golang.org/cmd/go/#hdr-Import_path_syntax) as
the `go` command and therefore
also supports relative import paths like `./...`. Additionally the `...`
wildcard can be used as suffix on relative and absolute file paths to recurse
into them.

The output of this tool is a list of suggestions in Vim quickfix format,
which is accepted by lots of different editors.

## Purpose

Gosimple differs from golint in that gosimple focusses on simplifying
code, while golint flags common style issues. Furthermore, gosimple
always targets the latest Go version. If a new Go release adds a
simpler way of doing something, gosimple will suggest that way.

Gosimple will never contain rules that are also present in golint,
even if they would fit into gosimple. If golint should merge one of
gosimple's rules, it will be removed from gosimple shortly after, to
avoid duplicate results. It is strongly suggested that you use golint
and gosimple together and consider gosimple an addon to golint.

## Checks

Gosimple checks for the following unsimple constructs:

- Uses of `select{}` with just one case, that could be a simple
  send/receive operation instead
- Uses of `for { select {} }` with just one case and no additional
  code in the loop body, that could be a simple `range` over a channel
  instead
- Comparisons of boolean values against the `true` and `false` constants
- Uses of the strings.Index* and bytes.Index family, when
  strings.Contains* and bytes.Contains suffice
- for loops that copy slices, when copy() would be simpler
- Using `bytes.Compare() == 0` instead of `bytes.Equal` – not only is
  the latter simpler, it's also faster.
