# optparse: Command line optional argument parser

[![CRAN Status Badge](https://www.r-pkg.org/badges/version/optparse)](https://cran.r-project.org/package=optparse) [![R-CMD-check](https://github.com/trevorld/r-optparse/actions/workflows/R-CMD-check.yaml/badge.svg?branch=master)](https://github.com/trevorld/r-optparse/actions) [![Coverage Status](https://codecov.io/github/trevorld/r-optparse/branch/master/graph/badge.svg)](https://app.codecov.io/github/trevorld/r-optparse?branch=master) [![RStudio CRAN mirror downloads](https://cranlogs.r-pkg.org/badges/optparse)](https://cran.r-project.org/package=optparse) [![Dependencies](https://tinyverse.netlify.app/badge/optparse)](https://tinyverse.netlify.app/)

<img src="man/figures/logo.png" align="right" width="200px" alt="optparse hex sticker">

A pure R language command line parser inspired by Python's
[optparse](https://docs.python.org/3/library/optparse.html) library to
be used with `Rscript` to write `#!` shebang scripts that accept short
and long flag/options.

To install the last version released on CRAN use the following command:

    install.packages("optparse")

To install the development version use the following command:

    install.packages("remotes")
    remotes::install_github("trevorld/r-optparse")

## examples

A simple example:

    library("optparse")
    parser <- OptionParser() |>
        add_option(c("-v", "--verbose"), action = "store_true",
                   default = TRUE, help = "Print extra output [default]") |>
        add_option(c("-q", "--quietly"), action = "store_false",
                   dest = "verbose", help = "Print little output") |>
        add_option(c("-c", "--count"), type = "integer", default = 5,
                   help = "Number of random normals to generate [default %default]",
                   metavar = "number")
    parse_args(parser, args = c("--quietly", "--count=15"))

    ## $help
    ## [1] FALSE
    ## 
    ## $verbose
    ## [1] FALSE
    ## 
    ## $count
    ## [1] 15

Note that the `args` argument of `parse_args()` default is
`commandArgs(trailing=TRUE)` so it typically doesn't need to be
explicitly set if writing an Rscript.

[optparse](https://github.com/trevorld/r-optparse) automatically creates
a help option:

    parse_args(parser, args = c("--help"))

    Usage: %prog [options]


    Options:
        -h, --help
            Show this help message and exit

        -v, --verbose
            Print extra output [default]

        -q, --quietly
            Print little output

        -c NUMBER, --count=NUMBER
            Number of random normals to generate [default 5]


    Error in parse_args(parser, args = c("--help")) : help requested

Note by default when `optparse::parse_args()` sees a `--help` flag it
will first print out a usage message and then either throw an error in
interactive use or call `quit()` in non-interactive use (i.e. when used
within an `Rscript` called by a shell). To disable the error/quit set
`print_help_and_exit = FALSE` in `parse_args()` and to simply print out
the usage string one can also use the function `print_usage()`.

[optparse](https://github.com/trevorld/r-optparse) has limited
positional argument support, other command-line parsers for R such as
[argparse](https://github.com/trevorld/r-argparse) have richer
positional argument support:

    parse_args(parser, args = c("-vc", "25", "75", "22"), positional_arguments = TRUE)

    ## $options
    ## $options$help
    ## [1] FALSE
    ## 
    ## $options$verbose
    ## [1] TRUE
    ## 
    ## $options$count
    ## [1] 25
    ## 
    ## 
    ## $args
    ## [1] "75" "22"

The function `parse_args2` wraps `parse_args` while setting
`positional_arguments=TRUE` and `convert_hyphens_to_underscores=TRUE`:

    parse_args2(parser, args = c("-vc", "25", "75", "22"))

    ## $options
    ## $options$help
    ## [1] FALSE
    ## 
    ## $options$verbose
    ## [1] TRUE
    ## 
    ## $options$count
    ## [1] 25
    ## 
    ## 
    ## $args
    ## [1] "75" "22"

## other R packages

-   When [optparse](https://github.com/trevorld/r-optparse) was
    originally written in 2009 the only option parsing package on CRAN
    was the comparatively low-level
    [getopt](https://github.com/trevorld/r-getopt) package but as of
    2026 there are *at least* 13 R [R Argument/Option Parser
    Packages](https://github.com/trevorld/r-cli-pkgs).
-   Personally I use [optparse](https://github.com/trevorld/r-optparse)
    for most scripts I write:
    -   It supports the most important features in an argument/option
        parsing packages with a reasonably high-level interface.
    -   It is well-tested with [high code
        coverage](https://app.codecov.io/github/trevorld/r-optparse?branch=master)
        and lots of users with millions of cumulative downloads over the
        past 15+ years.
    -   It has no dependencies.
    -   It features a stable API with a semantic version number greater
        than 1.0.
-   Occasionally I'll also use the
    [argparse](https://github.com/trevorld/r-argparse) package when I
    need advanced features unsupported by
    [optparse](https://github.com/trevorld/r-optparse) like sub parsers
    and named positional arguments:
    -   In general [argparse](https://github.com/trevorld/r-argparse)
        supports more advanced features than
        [optparse](https://github.com/trevorld/r-optparse) but depends
        on Python and three R packages whereas
        [optparse](https://github.com/trevorld/r-optparse) is a pure R
        package with no dependencies.
    -   However, unlike
        [argparse](https://github.com/trevorld/r-argparse),
        [optparse](https://github.com/trevorld/r-optparse) does support
        a callback action and also allows writing custom help usage
        formatters.
