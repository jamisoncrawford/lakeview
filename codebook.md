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
  - Two or more records during the same `ending` period for the same worker are considered distinct payments
* `ot`: Binary variable indicating whether a record in work period `ending` is entirely comprised of overtime.

## Preprocessed & Reformatted Variable Definitions

All [reformatted text files](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20CSVs%20-%20Redacted) and [reformatted workbooks](https://github.com/jamisoncrawford/lakeview/tree/master/Reformatted%20Workbooks%20-%20Redacted) contain the same variables regardless of content.

* `ending`: The ending date, in `YYYY-MM-DD` format, for the work period of a given record.
  - Some entries contain `ending` values within data despite containing no worker or payment data
* `zip`: The 5-digit ZIP code of a worker disclosed in her or his payment record.
  - Some `zip` values were extracted from `address` strings before the latter were redacted
  - Therefore, reformatted datasets may contain `zip` values that are not present in preprocessed workbooks
  - In instances where `zip` is unavailable, but `address` is, `zip` is extrapolated
* `ssn`: The last 4 digits disclosed in a given record for its corresponding worker.
* `class`: Classification of the worker, typically vocational, for a given record.
   - "Apprentice" or "Journeyman" status provided, if disclosed, including year, if applicable
   - *John C. Lowery, Inc.* records containing "App." or "app." are assumed to be an "Apprentice" `class`
   - Unless disclosed elsehow, `class` values are assumed to be "Journeyman" status
   - `class` also includes "Foreman", "Subforeman", "General Foreman", and variations
     - During scraping, values are recorded as-is, but will be homogenized once merged
     - In instances where `class` changes over time, only the first value is recorded
* `hours`: The total hours disclosed in a record for the given period, `ending`.
   - Calculated by the sum of regular and overtime pay, if provided
   - In instances of obvious bookkeeping errors, `hours` and related variables are corrected
   - In instances where `hours` are comprised of negative values, they are replaced with "0"
* `rate`: The hourly wage of the worker described in the payment record.
  - `ot` indicates if `rate` describes the hourly wage for overtime
  - In instances where only overtime is recorded, `rate` is calculated by dividing the given value by 1.5
  - In instances where more than one `rate` value exists for a unique worker, only the first may be recorded
* `gross`: The total pay for `period` absent deductions, e.g. taxes and union dues.
  - In instances where, arithmetically, `gross` is nonsensical based on `hours` and `rate`, value defined as misssing
    - This was a summary decision in preprocessing, though values may exist in raw data
  - In instances where `gross` is missing, albeit calculable given `rate` and `hours`, it is calculated
  - In instances where `gross` is comprised of negative values, they are replaced with "0"
* `net`: Total earnings for `period` equalling `gross` less deductions.
  - In some instances, e.g. some records in *Northeast Construction, Inc.*, `net` includes earnings from other jobs
  - In effect, it is recommended that `gross` be used as the principal means of measuring earned income
* `sex`: Nonordinal categorical defining worker gender, viz. `male` or `female`
  - In instances where `sex` is disclosed for `female` workers, only, it is assumed that missing values are `male`
  - In instances where `sex` is disclosed and contains at least one `female`, missing values are assumed `male`
* `race`: Nonordinal categorical defining worker race for a given record.
  - In instances where `race` is disclosed only for minorities, it is assumed that missing values are `White`
  - In instances where `race` is disclosed and contains at least one minority, missing values are assumed `White`
  - Categories of `race` have been abbreviated to include:
    - "White"
    - "Black"
    - "Native"
    - "Hispanic"
    - "Asian"
* `dup`: Binary indicating whether a worker has 2 payment records in a given `period` (`1`) or not (`0`)
  - Initial scrapes may indicate `1`, but collapsed overtime rows in later processing changes to `0`
  - Deprecated in master merge table (Hancock & Lakeview)
* `ot`: Binary indicating whether the payment is for overtime only (`1`) or not (`0`)
  - Two instances in *Quality Structures, Inc.* indicate vacation pay only, rather than overtime
* `add`: Binary indicating whether a personal address was redacted (`1`) or not (`0`)
  - Deprecated in master merge table (Hancock & Lakeview)

## Final Tables Variables

All tables located in the ["Final Tables"](https://github.com/jamisoncrawford/lakeview/tree/master/Final%20Tables) folder of the [Lakeview Repository](https://github.com/jamisoncrawford/lakeview) are prepared for merging into a master table, including Hancock Airport Renovations and Lakeview Ampitheater Construction. In addition to the above variables, these are largely metadata variables for each payment record.

* `project`: Non-ordinal categorical variable for title of construction project, e.g. "Lakeview" or "Hancock"
* `name`: Non-ordinal categorical variable for title of contractor or company, e.g. "Burn Bros" or "Ajay Glass"
* `pdf_no`: Integer indicating which PDF document contains particular record in raw data; typically "1"
* `pdf_pg`: Integer indicating which page number of PDF identified in `pdf_no` for partcular record
* `pg_ob`: Integer indicating which observation on `pdf_pg` the record represents
  - In instances where `pg_ob` appear missing, multiple rows for a single observation are collapsed
 
## Common Structure: Hancock & Lakeview

The 2018 Hancock Airport Renovation datasets have been reformatted using the above variables and definitions and are available in the [Final Tables folder](https://github.com/jamisoncrawford/hancock/tree/master/Final%20Tables) of the [Hancock Renovations Repository](https://github.com/jamisoncrawford/hancock). 
 
## Processing Transformations

All tables located in the ["Final Tables"](https://github.com/jamisoncrawford/lakeview/tree/master/Final%20Tables) folder are merged into a master table after which the data are homogenized with the following transformations.

* `ending` dates have been made uniform by rounding all `ending` values to the nearest Sunday
    - Companies like *M&S Fire Protection* tend to have non-calendar `ending` periods
    - Other companies, e.g. *Ajay Glass* tend to report `ending` periods mid-week, albeit consistently
* `zip` and `ssn` are left-side padded with "0" to remedy autoformatting and trimming leading zeroes
* `class` has been relabeled to be both homogenized and comparable between contractors:
    - `Journeyman` is comprised of the following values:
        - "Iron Workers Rochester",   
        - "Syracuse Iron Workers",       
        - "Syracuse Glazers"
        - "LMJ1"
        - "Journeyman"             
        - "Journey Wireman"
        - "Ironworker"
        - "Cement Mason"
        - "Piping Journeyman"
        - "PLBR/FTR JOUR SY"
        - "Sprinkler Fitter Journeyman"
        - "Tile Finisher Journeyman"
        - "Tile Mechanic Journeyman"
        - "Piledriver Journeyman"
        - "Piledriver Welder"
        - "Carpenter"
        - "Mason"
        - "Lab Haz 1"
        - "Journeyman Techn"
        - "Journeyman 2nd S"
        - "Plumber Journeyman"
        - "Service Tradesman"
        - "Iron Worker"
        - "IWJ"
        - "Oiler"                
        - "Journeyman Iron Worker"
        - "IW Journeyman"
        - "Painter"           
        - "Taper"
    - `Foreman` is comprised of the following values:
        - "Sub-Foreman"     
        - "Foreman Laborer"
        - "PL/FTR FORE SYRA"
        - "Foreman 2"
        - "Foreman"
        - "Piledriver Foreman"
        - "Piledriver Foreman"
        - "Foreman A"
        - "IWF"
        - "Subforeman"       
        - "Carpenter Foreman"
        - "Plumber Foreman"
        - "Lab. Form. Base"
    - `General Foreman` is comprised of the following values:
        - "PL/FTR GFORE SYR"
        - "General Foreman"
        - "Supervisor"
        - "Superintendent GF"
        - "Superintendent"
    - `Apprentice` is comprised of the following values:
        - "LAP3", "Apprentice"
        - "2nd Period Apprentice"
        - "PL/FTR 4THYR SYR"
        - "PL/FTR 1ST YR"
        - "PLB/FTR 2NDYR SY"
        - "PL/FTR 5THYR SYR"
        - "Sheet Metal Apprentice"    
        - "Sprinkler Fitter Apprentice"
        - "Tile Mechanic Appr Year 4 1/2"
        - "Tile Finisher Appt Year 1"
        - "4th Yr Appr Piledriver"
        - "Apprentice Carpenter"
        - "Apprentice Taper"
        - "Apprentice 4th Per"            
        - "Apprentice 5th Per"          
        - "Apprentice 2nd Per"
        - "Apprentice 6th Per"           
        - "Apprentice 3rd Per"          
        - "Apprentice 1st Per"
        - "Plumber Apprentice"           
        - "Apprentice 1st Year"         
        - "Apprentice 3rd Year"
        - "Apprentice 2nd Year"
        - "IW Apprentice"        
        - "Ironworker Apprentice" 
        - "2nd Year Apprentice"          
        - "3rd Period Apprentice" 
        - "4th Period Apprentice"
    - `Office` is comprised of the following values:
        - "Admin"
        - "Field Office Staff"
        - "Office Staff"
        - "Office"
        - "Clerical"
    - `Laborer` is comprised of the following values:
        - "Laborer"
        - "Labor"
        - "Laborer Base"
        - "Building Laborer"
    - `Other` is comprised of the following values:
        - "Owner"
        - "Teamster Truck Driver"
