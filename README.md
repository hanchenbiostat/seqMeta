<!-- README.md is generated from README.Rmd. Please edit that file -->
seqMeta
=======

[![Build Status](https://travis-ci.org/DavisBrian/seqMeta.svg?branch=master)](https://travis-ci.org/DavisBrian)[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/DavisBrian/seqMeta?branch=master)](https://ci.appveyor.com/project/DavisBrian/seqMeta)

Meta-Analysis of Region-Based Tests of Rare DNA Variants

Computes necessary information to meta analyze region-based tests for rare genetic variants (e.g. SKAT, T1) in individual studies, and performs meta analysis.

You can install:
----------------

-   the latest released version from CRAN with

    ``` r
    install.packages("dplyr")
    ```

-   the latest development version for testing by downloading seqMeta\_1.5.0.9XXX.tar.gz and running

    ``` r
    install.packages("/path/to/file/seqMeta_1.5.0.9XXX.tar.gz", type = "source")
    ```

-   the latest development version from github with

    ``` r
    if (packageVersion("devtools") < 1.6) {
      install.packages("devtools")
    }
    devtools::install_github("DavisBrian/seqMeta")
    ```

If you encounter a clear bug, please file a minimal reproducible example on [github](https://github.com/DavisBrian/seqMeta/issues).

seqMeta 1.5.0.9011
------------------

-   Migrated to git / github
-   Minimum R version moved to 3.1.0
-   Fixed issue \#1 - Duplicated SNP in snpinfo gene pulls from the genotype matrix twice.
-   Fixed issue \#2 - Monomorphic snps with caf != 0 handled incorrectly.
-   Fixed issue \#4 - Replaced `any(is.na(Z))` with `anyNA(Z)`
-   Fixed issue \#5 - Range test now checks that genotypes are [0, 2].
-   Added new function prepScores2

### prepScores2

prepScores2 is a drop in replacement for prepScores and prepScoresX. The only difference is the family argument should be text. `gaussian()` becomes `"gaussian"`. Although the former will **temporarily** still work. Eventually I'd like to merge prepCox into this function as 90+% of the code is the same.

-   enforces assumption that when a gene is being anlyzed the same snp can only be in that gene once (same snp can still be in multiple genes or in the same gene in SNPInfo)
-   Reorganized code
-   Moved data checks before imputation of genotypes
-   Moved other checks closer to where the data is calculated / used
-   Improved speed and reduced memory usage by
    -   Replacing genotype imputation `apply` code with a faster vectorized version.
    -   Replacing `any(is.na(Z))` with `anyNA(Z)`
    -   Replacing `range` with `min` and `max`
    -   Replacing matrix operations of the form `t(x)%*%y` with `crossprod(x, y)`
    -   Removing duplicate calculations
    -   Removing orphaned code
    -   Removing second imputation of genotypes in the covariance calculation
    -   Converting genotype matrix of type data.frame to a matrix.
-   Added Error Messages for
    -   Missing weight for snp-gene pair
    -   Duplicate weights for snp-gene pair
    -   Non-numeric genotypes
    -   family != gaussian or binomial
-   Added Warning Messages for:
    -   Removing missing snps and genes from SNPInfo
    -   Removing duplicated snp-gene pairs

### Todo

-   change genotype documentation to "matrix or an object which can be coerced to one with as.matrix"
-   add back verbose option to `prepScores2`
-   prepScores2 support for Survival
-   check kins and Z are in the same order?
-   need to verify `impute_to_mean` code does NOT make a copy of the genotype matrix if it is a matrix
-   test real `binomial` cases
-   test real kinship cases
