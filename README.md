[![Test Badge](https://github.com/entangled/filters/workflows/Tests/badge.svg)](https://github.com/entangled/filters/actions?query=workflow%3ATests)
[![codecov](https://codecov.io/gh/entangled/filters/branch/master/graph/badge.svg)](https://codecov.io/gh/entangled/filters)

# Entangled - Pandoc filters

This contains several Pandoc filters and scripts for literate programming in Markdown. These filters are enough to get you going with literate programming using Pandoc.

| Filter                     | Function                                         |
|----------------------------|--------------------------------------------------|
|`pandoc-annotate-codeblocks`| Adds annotation to code blocks in the woven output. |
|`pandoc-doctest`            | Runs doc-tests by passing the content of code blocks through Jupyter. |
|`pandoc-tangle`             | Generate source files from the content of code blocks. |
|`pandoc-bootstrap`          | Expand some elements specifically targeting a Bootstrap page. |

## Install

### Prerequisites

- **Python >=3.7**: All of these filters are written in Python. This is mainly to encourage as many users (**I mean YOU**) to start developing Pandoc filters.
- **Dhall**: the `pandoc-bootstrap` filter requires `dhall-to-json` to be installed: see [Dhall language](https://dhall-lang.org/).
  TLDR: download `dhall-json-*-[windows|macos|linux].[zip|tar.bz2]` from the [Dhall release page](https://github.com/dhall-lang/dhall-haskell/releases), and extract it to a location in your `$PATH`. Dhall is awesome, it *will* make your life better.

```shell
pip install entangled-filters
```

### For development

To run tests, after doing a normal install (to get the executables installed), run

```shell
pip install --upgrade -e .[test]
pytest
```

The executables are auto-generated by the `setup.py` script and call some `python -m` command.

## Supported syntax

See the [project homepage](https://entangled.github.io) for more info.

### Named code blocks

~~~markdown
``` {.python #hello}
print("Hello, World!")
```
~~~

### Reference code blocks

~~~markdown
``` {.python #main}
def main():
    <<hello>>
```
~~~

### Define files

~~~markdown
``` {.python file=hello.py}
<<main>>

if __name__ == "__main__":
    main()
```
~~~

### Documentation tests

~~~markdown
``` {.python .doctest #the-question}
6*7
---
42
```
~~~

## `pandoc-tangle`

Extracts code blocks and writes them to files.

```shell
pandoc -t plain --filter pandoc-tangle hello.md
```

## `pandoc-doctest`

Runs doctests, and include results into output.

```shell
pandoc -t html5 -s --filter pandoc-doctest hello.md
```

## Docker

The Entangled pandoc filters is available as a [Docker image](https://hub.docker.com/repository/docker/nlesc/pandoc-tangle).

### Run

In your current working directory with a README.md file run 

```
docker run --rm -ti --user $UID -v $PWD:/data nlesc/pandoc-tangle README.md
```

This will extracts code blocks and writes them to files.

### Build

```shell
docker build -t nlesc/pandoc-tangle .
```
