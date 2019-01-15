optparse: Command line optional argument parser
===============================================

[![CRAN Status Badge](https://www.r-pkg.org/badges/version/optparse)](https://cran.r-project.org/package=optparse)

[![Travis-CI Build Status](https://travis-ci.org/trevorld/r-optparse.svg?branch=master)](https://travis-ci.org/trevorld/r-optparse)

[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/trevorld/r-optparse?branch=master&svg=true)](https://ci.appveyor.com/project/trevorld/r-optparse)

[![Coverage Status](https://img.shields.io/codecov/c/github/trevorld/r-optparse/master.svg)](https://codecov.io/github/trevorld/r-optparse?branch=master)

[![RStudio CRAN mirror downloads](https://cranlogs.r-pkg.org/badges/optparse)](https://cran.r-project.org/package=optparse)

A pure R language command line parser inspired by Python\'s \'optparse\'
library to be used with Rscript to write \"\#!\" shebang scripts that
accept short and long flag/options.

To install the last version released on CRAN use the following command:

    > install.packages("optparse")

To install the development version use the following command:

    > remotes::install_github("trevorld/r-optparse")

dependencies
------------

This package depends on the R package `getopt`.

To run the unit tests you will need the suggested R package `testthat`
and in order to build the vignette you will need the suggested R package
`knitr` which in turn probably requires the system tool `pandoc`:

    $ sudo apt-get install pandoc

examples
--------

A simple example:

    > library("optparse")
    > parser <- OptionParser()
    > parser <- add_option(parser, c("-v", "--verbose"), action="store_true", 
    >                 default=TRUE, help="Print extra output [default]")
    > parser <- add_option(parser, c("-q", "--quietly"), action="store_false", 
    >                     dest="verbose", help="Print little output")
    > parser <- add_option(parser, c("-c", "--count"), type="integer", default=5, 
    >                 help="Number of random normals to generate [default %default]",
    >                 metavar="number")
    > parse_args(parser, args = c("--quietly", "--count=15"))
    $help
    [1] FALSE

    $verbose
    [1] FALSE

    $count
    [1] 15

Note that the `args` argument of `parse_args` default is
`commandArgs(trailing=TRUE)` so it typically doesn\'t need to be
explicitly set if writing an Rscript.

One can also equivalently make options in a list:

    > library("optparse")
    > option_list <- list( 
    >     make_option(c("-v", "--verbose"), action="store_true", default=TRUE,
    >         help="Print extra output [default]"),
    >     make_option(c("-q", "--quietly"), action="store_false", 
    >         dest="verbose", help="Print little output"),
    >     make_option(c("-c", "--count"), type="integer", default=5, 
    >         help="Number of random normals to generate [default %default]",
    >         metavar="number")
    >     )
    >                                     
    > parse_args(OptionParser(option_list=option_list), args = c("--verbose", "--count=11"))
    $verbose
    [1] TRUE

    $count
    [1] 11

    $help
    [1] FALSE

`optparse` automatically creates a help option:

    > parse_args(parser, args = c("--help"))

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

Note by default when `optparse::parse_args` sees a `--help` flag it will
first print out a usage message and then either throw an error in
interactive use or call `quit` in non-interactive use (i.e. when used
within an Rscript called by a shell). To disable the error/quit set the
argument `print_help_and_exit` to `FALSE` in `parse_args` and to simply
print out the usage string one can also use the function `print_usage`.

`optparse` has limited positional argument support, other command-line
parsers for R such as `argparse` have richer positional argument
support:

    > parse_args(parser, args = c("-v", "-c25", "75", "22"), positional_arguments = TRUE)

    $options
    $options$help
    [1] FALSE

    $options$verbose
    [1] TRUE

    $options$count
    [1] 5


    $args
    [1] "75"   "22"  

The function `parse_args2` wraps `parse_args` while setting
`positional_arguments=TRUE` and `convert_hyphens_to_underscores=TRUE`:

    > parse_args2(parser, args = c("-v", "-c25", "75", "22"))

    $options
    $options$help
    [1] FALSE

    $options$verbose
    [1] TRUE

    $options$count
    [1] 5


    $args
    [1] "75"   "22"  
