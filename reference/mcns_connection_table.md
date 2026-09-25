# Connectivity query for CNS neurons

Connectivity query for CNS neurons

## Usage

``` r
mcns_connection_table(
  ids,
  partners = c("inputs", "outputs"),
  moredetails = c("group", "superclass", "somaSide"),
  summary = FALSE,
  threshold = 1L,
  roi = NULL,
  by.roi = FALSE,
  conn = mcns_neuprint(),
  ...
)
```

## Arguments

- ids:

  A set of body ids (see
  [`manc_ids`](https://natverse.org/malevnc/reference/manc_ids.html) for
  a range of ways to specify these).

- partners:

  Either inputs or outputs. Redundant with `prepost`, but probably
  clearer.

- moredetails:

  Either a logical (to add all fields when `TRUE`) or a character vector
  naming additional fields returned by
  [`mcns_neuprint_meta`](https://natverse.org/malecns/reference/mcns_neuprint_meta.md)
  that will be added to the results data.frame.

- summary:

  Whether to summarise results per partner when giving multiple query
  neurons

- threshold:

  Only return partners \>= to an integer value. Default of 1 returns all
  partners. This threshold will be applied to the ROI weight when the
  `roi` argument is specified, otherwise to the whole neuron.

- roi:

  a single ROI. Use `neuprint_ROIs` to see what is available.

- by.roi:

  logical, whether or not to break neurons' connectivity down by region
  of interest (ROI)

- conn:

  Optional, a
  [`neuprint_connection`](https://natverse.org/neuprintr/reference/neuprint_login.html)
  object, which also specifies the neuPrint server. Defaults to
  [`manc_neuprint()`](https://natverse.org/malevnc/reference/manc_neuprint.html)
  to ensure that query is against the VNC dataset.

- ...:

  additional arguments passed to
  [`neuprint_connection_table`](https://natverse.org/neuprintr/reference/neuprint_connection_table.html)

## Value

A data.frame

## Examples

``` r
library(dplyr)
#> 
#> Attaching package: ‘dplyr’
#> The following objects are masked from ‘package:nat’:
#> 
#>     intersect, setdiff, union
#> The following objects are masked from ‘package:stats’:
#> 
#>     filter, lag
#> The following objects are masked from ‘package:base’:
#> 
#>     intersect, setdiff, setequal, union
mcns_connection_table('DNa02', partners = 'out') %>% head()
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
mcns_connection_table('DNa02', partners = 'out', summary = TRUE) %>% head()
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# return weight of outputs in the brain specifically (see ROIweight column)
mcns_connection_table('DNa02', partners = 'out', roi='CentralBrain') %>% head()
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.

# \donttest{
joffrey.id=mcns_xyz2bodyid(cbind(24590, 13816, 26102)+4096, node = 'neuprint')
joffrey.us=mcns_connection_table(joffrey.id, partners = 'in')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
joffrey.uss=mcns_connection_table(joffrey.id, partners = 'in', summary=TRUE)
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# }
if (FALSE) { # \dontrun{
# open top 10 partners in neuroglancer,
# NB segmentation / meshes to match neuprint
mcns_scene(joffrey.uss$partner[1:10], open = TRUE, node='neuprint')
} # }
```
