# Find/predict the soma side or position of male cns neurons.

`mcns_somapos` returns the XYZ location (in nm, microns or raw voxel
space) of the soma position for neurons. When no valid soma position is
available, then a `NA` value is returned.

## Usage

``` r
mcns_soma_side(ids, method = c("auto", "position", "instance", "manual"))

mcns_somapos(
  ids,
  units = c("nm", "microns", "um", "raw"),
  as_character = FALSE
)
```

## Arguments

- ids:

  A set of bodyids or a dataframe containing the `name` and
  `somaLocation` fields

- method:

  Whether to use the side recorded in the instance field, the soma
  position or each of those in turn to predict. The method manual
  returns manually curated soma sides recorded via Clio (see details).

- units:

  For `mcns_somapos` the units of returned 3D positions. Defaults to
  *nm*.

- as_character:

  Whether to return the positions as Nx3 matrix (the default) or as
  comma separated strings.

## Value

For `mcns_soma_side` a vector of sides (L, R, M, U or NA). Midline or
unpaired neurons should be indicated with an M although I have seen U in
the past.

For `mcns_somapos` a matrix or a character vector depending on the value
of `as_character`

## Details

the recorded `somaSide` column in neuPrint / `soma_side` in clio should
be preferred but is not always available. `method='auto'` will prefer
those columns but then cascade through instance and somaLocation to
define the rest.

## See also

Other annotations:
[`mcns_body_annotations()`](https://natverse.org/malecns/reference/mcns_body_annotations.md),
[`mcns_dvid_annotations()`](https://natverse.org/malecns/reference/mcns_dvid_annotations.md),
[`mcns_neuprint_meta()`](https://natverse.org/malecns/reference/mcns_neuprint_meta.md)

## Examples

``` r
# \donttest{
mcns_soma_side('/LAL04.*')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# }
if (FALSE) { # \dontrun{
# All neurons with a type
table(mcns_soma_side('/.*'), useNA='if')

# compare manual with predictions
mcnswsoma=mcns_body_annotations(query=list(soma_side="exists/1"))
mcnswsoma$pside=mcns_soma_side(mcnswsoma$bodyid)
with(mcnswsoma, table(soma_side, pside))

# compare manual with instance
mcnswsoma=mcns_body_annotations(query=list(soma_side="exists/1"))
mcnswsoma$iside=mcns_soma_side(mcnswsoma$bodyid, method='instance')
with(mcnswsoma, table(soma_side, iside, useNA = 'i'))

# converse: compare instance with manual
mcns_instance=mcns_neuprint_meta('/name:.+')
if(!"somaSide" %in% colnames(mcns_instance))
  mcns_instance$somaSide=mcns_soma_side(mcns_instance, method='manual')
mcns_instance$iside=mcns_soma_side(mcns_instance, method='instance')
# many mismatches e.g. due to neurons without a soma including truncated
# sensory neurons, ascending neurons etc
with(mcns_instance, table(somaSide, iside, useNA = 'i'))

# we can parse that a bit by doing
mcns_instance %>% filter(soma) %>% with(table(somaSide, iside, useNA = 'i'))
} # }
sp=mcns_somapos('/LAL04.*', units='um')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
plot(sp[,1:2])
#> Error: object 'sp' not found
mcns_somapos('LAL042', units='um', as_character=TRUE)
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
```
