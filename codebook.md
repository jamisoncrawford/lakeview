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

All [reformatted text files](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20CSVs%20-%20Redacted) and [reformatted workbooks](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20Workbooks%20-%20Redacted) contain the same variables regardless of content.

* `ending`: The ending date, in `YYYY-MM-DD` format, for the work period of a given record.
  - Some entries contain `ending` values within data despite containing no worker or payment data
* `zip`: The 5-digit ZIP code of a worker disclosed in her or his payment record.
  - Some `zip` values were extracted from `address` strings before the latter were redacted
  - Therefore, reformatted datasets may contain `zip` values that are not present in preprocessed workbooks
* `ssn`: The last 4 digits disclosed in a given record for its corresponding worker.
* `class`: Classification of the worker, typically vocational, for a given record.
* `hours`: The total hours disclosed in a record for the given period, `ending`.
* `rate`: The hourly wage of the worker described in the payment record.
  - `ot` indicates if `rate` describes the hourly wage for overtime
* `gross`: The total pay for `period` absent deductions, e.g. taxes and union dues.
  - In instances where, arithmetically, `gross` is nonsensical based on `hours` and `rate`, the value is defined as `NA` (misssing)
  - This was a summary decision in preprocessing, though values may exist in raw data
* `net`: Total earnings for `period` equalling `gross` less deductions.
* `sex`: Nonordinal categorical defining worker gender, viz. `male` or `female`
  - In instances where `sex` is disclosed for `female` workers, only, it is assumed that missing values are `male`
* `race`: Nonordinal categorical defining worker race for a given record.
  - In instances where `race` is disclosed only for minorities, it is assumed that missing values are `White`
* `dup`: Binary indicating whether a worker has 2 payment records in a given `period` (`1`) or not (`0`)
  - This appears to be the case for only one contractor, viz. `tblr_am_electric.*`
* `ot`: Binary indicating whether the payment is for overtime only (`1`) or not (`0`)
* `add`: Binary indicating whether a personal address was redacted (`1`) or not (`0`)
