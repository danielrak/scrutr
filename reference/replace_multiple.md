# Replace character vector values using a correspondence approach

Names of `replacements` are matched literally (not as regular
expressions). Elements of `input_vector` that match none of the patterns
are returned unchanged.

## Usage

``` r
replace_multiple(input_vector, replacements, replace_all = FALSE)
```

## Arguments

- input_vector:

  Character. Character vector on which replacements take place.

- replacements:

  Character. Named character vector defining replacement correspondences
  (names = patterns, values = replacements). Names are treated as
  literal strings (regex metacharacters such as `.`, `(`, `+` are not
  interpreted).

- replace_all:

  Logical. If `TRUE`,
  [`stringr::str_replace_all()`](https://stringr.tidyverse.org/reference/str_replace.html)
  is used instead of
  [`stringr::str_replace()`](https://stringr.tidyverse.org/reference/str_replace.html).

## Value

Character vector with replacements applied.

## Overlapping patterns

When several patterns can match the same element, the first match found
in `input_vector` (scanned left to right) is used. When patterns start
at the same position, the one listed first in `replacements` wins, not
the longest. Order `replacements` accordingly if some patterns are
prefixes of others.

## Examples

``` r
input <- c("one-one", "two-two-one", "three-three-two")

replace_multiple(input,
                 replacements =
                   c("one" = "1", "two" = "2",
                     "three" = "3"))
#> [1] "1-one"       "2-two-one"   "3-three-two"

replace_multiple(input,
                 replacements =
                   c("one" = "1", "two" = "2",
                     "three" = "3"),
                 replace_all = TRUE)
#> [1] "1-1"     "2-2-one" "3-3-two"

# Unmatched elements are returned as-is:
replace_multiple(c("one", "unmatched"), c("one" = "1"))
#> [1] "1"         "unmatched"

# Regex metacharacters are matched literally:
replace_multiple(c("a.b", "aXb"), c("." = "DOT"))
#> [1] "aDOTb" "aXb"  
```
