# Predict the cell type of male cns neurons from type and name/instance fields

Predict the cell type of male cns neurons from type and name/instance
fields

## Usage

``` r
mcns_predict_type(
  ids,
  method = c("auto", "instance", "type", "foreign_type", "all"),
  prefer.foreign = FALSE
)
```

## Arguments

- ids:

  Body ids in any form understandable by
  [`mcns_ids`](https://natverse.org/malecns/reference/mcns_ids.md)

- method:

  The prediction method to use (type, instance or auto, which uses type
  when available, instance otherwise). The special value of all adds 3
  columns to the metadata data.frame `type_t, type_i, type_a` containing
  the results of each of the other methods.

- prefer.foreign:

  Whether to prefer a foreign type (flywire, manc, hemibrain) when
  available. This may be useful for integration across datasets (see
  details).

## Value

A data.frame when `type='all'`, a character vector otherwise.

## Details

For the time being when the instance is of the form 12345_L then 12345
will be returned as the type. This obviously isn't very useful for most
purposes (use
[`mcns_predict_group`](https://natverse.org/malecns/reference/mcns_predict_group.md)
if you want the group).

Note that when `prefer.foreign=TRUE` or `method='foreign_type'`, if both
flywire and manc types are available, the flywire type will be preferred
*except* for (sensory) ascending neurons. The thinking behind this
exception is that the types given in flywire for the truncated axons of
the ascending neurons are unlikely to be canonical.

## See also

[`mcns_predict_group`](https://natverse.org/malecns/reference/mcns_predict_group.md)

## Examples

``` r
# \donttest{
library(dplyr)
mnm.ti <- mcns_neuprint_meta('where:exists(n.type) OR exists(n.instance)')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# Descending neurons with the wrong superclass
mnm.ti %>%
  mutate(ptype=mcns_predict_type(.)) %>%
  filter(grepl("DN[abdgpx]", ptype)) %>%
  filter(superclass!='Descending' | is.na(superclass))
#> Error: object 'mnm.ti' not found

# report all the different types available for these two VPNs
mcns_predict_type("LoVP106", method = 'all') %>%
  select(matches('.ype'))
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# report just one type, preferring malecns type (the default)
mcns_predict_type("LoVP106")
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# ... or preferring the foreign type (flywire in this case)
mcns_predict_type("LoVP106", prefer.foreign=TRUE)
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
# }
```
