# Predict the group of neurons using instance or type information

Predict the group of neurons using instance or type information

## Usage

``` r
mcns_predict_group(
  ids,
  method = c("auto", "fullauto", "group", "manc", "instance", "type", "pmanc", "all"),
  badtypes = c(NA, "", "Lamina_R1-R6", "Descending", "KC", "ER", "LC", "PB",
    "Ascending Interneuron", "Delta", "P1_L candidate", "LT", "MeMe", "PFGs", "Mi", "VT",
    "ML", "EL", "FB", "Dm", "DNp", "FC", "OL", "T", "Y")
)
```

## Arguments

- ids:

  Body ids in any form understood by
  [`mcns_ids`](https://natverse.org/malecns/reference/mcns_ids.md). If
  you have a metadata dataframe as returned by
  [`mcns_neuprint_meta`](https://natverse.org/malecns/reference/mcns_neuprint_meta.md)
  then this is ideal as that function is called under the hood.

- method:

  A string specifying which of 5 methods to use to identify the group.
  `"all"` means to return all 5, while `"fullauto"` means to look at
  each method in turn successively filling in missing group values.
  Method `"auto"` (the default) excludes predicted manc matches (see
  details).

- badtypes:

  Values of the type column which should be ignored for the purposes of
  defining cell type groups. This will be because they contain bad
  values or because the types are too broad to be very useful.

## Value

For `method="all"` a dataframe as returned by
[`mcns_neuprint_meta`](https://natverse.org/malecns/reference/mcns_neuprint_meta.md)
with additional columns `instance_group` and `type_group`. Otherwise a
numeric vector.

## Details

Grouping information for neurons in the male cns is presently scattered
in several locations. These include the numeric group field, the type
field or the instance field. If the type field has the same value, the
neurons should form a group. However there are some values that are
known to be bad and these are excluded.

An additional source of group information comes from matches of VNC
neurons to the MANC dataset. These either come as curated matches (where
the `manc_group` column has been entered in Clio, `method="manc"`) or as
predicted matches (based on the `manc_bodyid` column, `method="pmanc"`).

`method="pmanc"` should be used with caution since a significant
percentage of these matches are wrong. However, since the majority
should be correct, they may still be a useful source of group
information e.g. for connectivity clustering which is typically not that
sensitive to errors.

Given this situation `method='auto'` (the default) only uses curated
matches (`method="manc"`). Select `method='fullauto'` to use the
predicted MANC matches as a fall-back.

## See also

[`mcns_predict_type`](https://natverse.org/malecns/reference/mcns_predict_type.md)

## Examples

``` r
# \donttest{
library(dplyr)
# return all body ids with a group type or instance
tig_ids=mcns_ids('where:exists(n.group) OR exists(n.type) OR exists (n.instance)')
#> Error in (function (path, body = NULL, server = NULL, conf = NULL, parse.json = TRUE,     include_headers = TRUE, simplifyVector = FALSE, app = NULL,     ...) {    if (is.null(app))         app = paste0("neuprintr/", utils::packageVersion("neuprintr"))    req <- if (is.null(body)) {        httr::GET(url = file.path(server, path, fsep = "/"),             config = conf, httr::user_agent(app), ...)    }    else {        httr::POST(url = file.path(server, path, fsep = "/"),             body = body, config = conf, httr::user_agent(app),             ...)    }    neuprint_error_check(req)    if (parse.json) {        parsed = neuprint_parse_json(req, simplifyVector = simplifyVector)        if (length(parsed) == 2 && isTRUE(names(parsed)[2] ==             "error")) {            stop("neuPrint error: ", parsed$error)        }        if (include_headers) {            fields_to_include = c("url", "headers")            attributes(parsed) = c(attributes(parsed), req[fields_to_include])        }        parsed    }    else req})(path = path, body = body, server = server, conf = conf, parse.json = parse.json,     include_headers = include_headers, simplifyVector = simplifyVector,     app = app): Unauthorized (HTTP 401). Failed to process url: https://neuprint.janelia.org/api/dbmeta/datasets with neuPrint error: invalid or expired token — neuPrint has moved to a new authorization system; log in at https://neuprint.janelia.org/account to obtain a new token.
allg=mcns_predict_group(tig_ids, method = 'all')
#> Error: object 'tig_ids' not found
# neurons where the recorded group and instance group disagree
allg %>% filter(!is.na(group) & !is.na(instance_group) & group!=instance_group)
#> Error: object 'allg' not found
# }
if (FALSE) { # \dontrun{
# neurons where the recorded group and type group disagree
type_group_mismatch <- allg %>% filter(!is.na(group) & !is.na(type_group) & group!=type_group)
allg %>%
  filter(group %in% type_group_mismatch$group | type_group %in% type_group_mismatch$type_group) %>%
 select(bodyid, type, name, group, type_group, instance_group) %>%
 arrange(type, group) %>% View
} # }
```
