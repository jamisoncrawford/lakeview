# Lakeview Amphitheater: Variable Codebook

The following provides definitions across datasets for each variable, units of measurement, and variable transformations, if applicable, for all *formatted and redacted* datasets.

Moreover, a brief note on raw data preprocessing by Legal Services of Central New York (LSCNY) as well as a note on data redactions is provided.

## On Raw Data & Redactions

A number of employee records provided by Onondaga County to LSCNY contain the personal addresses of construction workers. Therefore, all personal addresses disclosed in employment records have been redacted in the [Preprocessed Workbooks](https://github.com/jamisoncrawford/lakeview/tree/master/Preprocessed%20Workbooks%20-%20Redacted) provided by LSCNY, [Reformatted Workbooks](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20Workbooks%20-%20Redacted), and [Reformatted Text Files](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20CSVs%20-%20Redacted).

Redactions are indicated with `r_` in file name prefixes, including:

* `prer*` for preprocessed .xlsx workbooks 
* `tblr*` for reformatted .xlsx workbooks and .csv text files

Each *formatted* file contains binary variable `add` indicating whether an address was redacted (`1`) or missing (`0`).

## On Preprocessing

LSCNY manually scraped 22 "smaller" datasets provided by Onondaga County, which are preserved in the [Preprocessed Workbooks Folder](https://github.com/jamisoncrawford/lakeview/tree/master/Preprocessed%20Workbooks%20-%20Redacted). These .xlsx files are unaltered, barring redacted personal addresses, and contain color-coding and commentary reflecting summary decisions in preprocessing.

In order to prevent information loss, the following variables were added to *formatted* .xlsx and .csv files in lieu of comments and other features which cannot translate to text:

* `dup`: Binary variable indicating whether a worker has two records in the same work period (`ending`)
  - `1` indicates a second entry within period `ending`
* `ot`: Binary variable indicating whether a record in work period `ending` is entirely comprised of overtime.

## Preprocessed & Reformatted Variable Definitions

