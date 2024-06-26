## NAME
       bat - a cat(1) clone with syntax highlighting and Git integration.

## USAGE
       `bat [OPTIONS] [FILE]...`

       bat cache [CACHE-OPTIONS] [--build|--clear]

## DESCRIPTION
       bat  prints  the syntax-highlighted content of a collection of FILEs to the terminal. If no FILE is specified, or when FILE is
       '-', it reads from standard input.

       bat supports a large number of programming and markup languages.  It also communicates with git(1) to show modifications  with
       respect to the git index.  bat automatically pipes its output through a pager (by default: less).

       Whenever  the  output  of bat goes to a non-interactive terminal, i.e. when the output is piped into another process or into a
       file, bat will act as a drop-in replacement for cat(1) and fall back to printing the plain file contents.

