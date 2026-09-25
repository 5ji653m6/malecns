# Map XYZ locations to bodyids for the male cns dataset

Map XYZ locations to bodyids for the male cns dataset

## Usage

``` r
mcns_xyz2bodyid(
  xyz,
  units = c("raw", "nm", "microns", "um"),
  node = "neutu",
  cache = FALSE
)
```

## Arguments

- xyz:

  xyz location (by default in raw malecns pixels)

- units:

  The Optional units of the incoming 3D positions. Defaults to *raw*.

- node:

  A DVID node as returned by
  [`manc_dvid_node`](https://natverse.org/malevnc/reference/manc_dvid_node.html).
  The default is to return the current active (unlocked) node being used
  through neutu.

- cache:

  Whether to cache the result of this call for 5 minutes.

## Value

A character vector of body ids (0 is missing somas / missing locations)

## Examples

``` r
# \donttest{
# find the bodyids corresponding to set of soma positions
mcns_xyz2bodyid(mcns_somapos("/LAL04[12]", units='raw'), units='raw')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# }
# the APL
if (FALSE) { # \dontrun{
mcns_xyz2bodyid(cbind(24508, 15674, 26116)+4096)
} # }
```
