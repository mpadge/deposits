# deposits

<!-- badges: start -->

[![R build
status](https://github.com/ropenscilabs/deposits/workflows/R-CMD-check/badge.svg)](https://github.com/ropenscilabs/deposits/actions?query=workflow%3AR-CMD-check)
[![codecov](https://codecov.io/gh/ropenscilabs/deposits/branch/main/graph/badge.svg)](https://codecov.io/gh/ropenscilabs/deposits)
[![Project Status:
Concept](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
<!-- badges: end -->

The `deposits` R package is a universal client for depositing and
accessing research data anywhere. Currently supported services are
[zenodo](https://zenodo.org) and [figshare](https://figshare.com). This
README primarily contains information on setting up the `deposits`
package. Use of the package is described in [the introductory
vignette](https://docs.ropensci.org/deposits/articles/deposits.html).

## Installation

The package can be installed by enabling the [“ropensci”
r-universe](https://ropensci.r-universe.dev), and using
`install.packages()`:

``` r
options (repos = c (
    ropensci = "https://ropensci.r-universe.dev",
    CRAN = "https://cloud.r-project.org"
))
install.packages ("deposits")
```

Alternatively, the package can be installed directly from GitHub with
the following command:

``` r
remotes::install_github ("ropenscilabs/deposits")
```

The package can then be loaded for use with:

``` r
library (deposits)
```

## Data Repositories

The list of data repositories currently supported is accessible by the
`deposits_services()` function:

``` r
deposits_services ()
```

    ##             name                           docs                    api_base_url
    ## 1         zenodo https://developers.zenodo.org/         https://zenodo.org/api/
    ## 2 zenodo-sandbox https://developers.zenodo.org/ https://sandbox.zenodo.org/api/
    ## 3       figshare     https://docs.figshare.com/    https://api.figshare.com/v2/

[`zenodo`](https://zenodo.org) offers a “sandbox” environment, which
offers an ideal environment for testing the functionality of this
package.

## API tokens

All services require users to create an account and then to generate API
(“Application Programming Interface”) tokens. Click on the following
links to generate tokens, also listed as sequences of menu items used to
reach token settings:

-   [zenodo/account/settings/applications/tokens/new](https://zenodo.org/account/settings/applications/tokens/new/)
-   [zenodo-sandbox/account/settings/applications/tokens/new](https://sandbox.zenodo.org/account/settings/applications/tokens/new/),
-   [figshare/account/applications](https://figshare.com/account/applications).

It is not necessary to create or register applications on any of these
services; this package uses personal tokens only. The tokens need to be
stored as local environment variables the names of which must include
the names of the respective services, as defined by the “name” column
returned from `deposits_service()`, as shown above. This can be done as
in the following example:

``` r
Sys.setenv ("ZENODO_SANDBOX_TOKEN" = "<my-token")
```

Alternatively, these tokens can be stored in a `~/.Renviron` file, where
they will be automatically loaded into every R session.

## Functionality

The `deposits` package extends from [an `R6`
client](https://github.com/r-lib/R6) offering a variety of methods for
performing actions on deposits services. Details of the methods can be
seen in the [help file for the
`depositsClient`](https://docs.ropensci.org/deposits/reference/depositsClient.html).
All `deposits` operations start with a client constructed with the
`new()` function, with a first parameter specifying the desired deposits
service:

``` r
cli <- depositsClient$new (service = "zenodo", sandbox = TRUE)
cli
```

    ## <deposits client>
    ##  deposits service : zenodo
    ##            sandbox: TRUE
    ##          url_base : https://sandbox.zenodo.org/api/
    ##  Current deposits : <none>
    ## 
    ##    hostdata : <none>
    ##    metadata : <none>

The command, `ls(cli)`, will list all elements of the R6 object, with
the functions prefixed with `deposit_` or `deposits_`. These functions
are:

1.  `deposit_delete(deposit_id)` to delete a nominated deposit;
2.  `deposit_download_file(deposit_id, filename)` to download nominated
    `filename` from specified deposit.
3.  `deposit_fill_metadata(meta)` to fill a `deposits` client with
    metadata (see introductory vignette for details);
4.  `deposit_new()` to create a new deposit on a deposits service using
    the metadata inserted with `fill_metadata()`;
5.  `deposit_retrieve(deposit_id)` to retrieve information on a
    specified deposit;
6.  `deposit_update(deposit_id)` to update deposit on server with local
    metadata;
7.  `deposit_upload_file(deposit_id, path)` to upload a local file to a
    specified deposit; and
8.  `deposits_list()` to update lists of current deposits within client.

[An introductory
vignette](https://docs.ropensci.org/deposits/articles/deposits.html)
demonstrates all of these functions, while the “Metadata” vignette
describes processes to specify deposit metadata with a `deposits`
client.

## Code of Conduct

Please note that this package is released with a [Contributor Code of
Conduct](https://ropensci.org/code-of-conduct/). By contributing to this
project, you agree to abide by its terms.
