- [Methodology](methodology.md)
- [Advice for others](howto.md)
- [Suggested reading](reading.md)

# Data and documentation

The Star Tribune has compiled zoning and other related data related to 102 Twin Cities-area municipalities into simplified files for analysis.

### Copyright and license
The Star Tribune's zoning spreadsheets posted here are open data, licensed under the Open Data Commons Open Database License (ODbL) by the Star Tribune.

You are free to copy, distribute, transmit and adapt the spreadsheets, so long as you:

Credit the Star Tribune as specified below.
Inform the Star Tribune that you are using the spreadsheets in your work by emailing Michael Corey at michael.corey@startribune.com.
If you alter or build upon our data, you may distribute the result only under the same license. The full legal code explains your rights and responsibilities.

How to credit the Star Tribune
We require that you use the credit “Star Tribune, city and county governments and the U.S. Census Bureau." The credit must link to https://github.com/striblab/2021-zoning-project, unless the credit is appearing in printed media.

You must also make it clear to anyone who requests access to the data that it is available under the Open Database License. If you are distributing the spreadsheets in a data form, you can name and link directly to the license.

### Standardized zoning types
The Star Tribune categorized each city's zoning codes into several standardized categories to make analysis at a metro level possible.

|Zoning type|Description|
|---|---|
|only detached|Zoning districts that only allow single-family detached homes as a permitted use. Categorized as residential.|
|rural residential|Zoning districts that only allow single-family detached homes but have minimum lot size requirements exceeding 5 acres. Many of these are labeled in zoning codes as "intended for future development" and require lots to be divided for sale at no less than 10, 20 or even 40 acres. Not included in our residential calculations.|
|two family|Zoning districts that allow "middle" housing as the most dense form of housing. This includes townhomes, duplexes, triplexes, fourplexes and mobile homes. Often these areas also allow single-family detached homes, as well. Categorized as residential.|
|MF residential|Multi-family residential zoning districts that allow apartment complexes of four units or more as the most dense form of housing. Categorized as residential.|
|mixed|Areas allowing both residential and commercial uses. Typically the residential uses were solely multi-family housing. Categorized as residential.|
|non-residential|Any zoning districts that are for commercial, industrial or other non-residential purposes.|
|agriculture|Areas defined in zoning codes as being primarily used for agriculture, although these areas allow single-family homes. Not included in our residential calculations.|
|PUD-MIXED|Areas under Planned Unit Development Agreement(s) that intended to result in a mixed use development. Categorized as residential.|
|PUD-NONRES|Areas under Planned Unit Development Agreement(s) that intended to result in non-residential development.|
|PUD-R|Areas under Planned Unit Development Agreement(s) that intended to result in residential development. Categorized as residential. It wasn't feasible to identify the specific type of housing allowed in each area. That would have required reading each PUD agreement.|
|PUD-unknown|Areas under Planned Unit Development Agreement(s) where the Star Tribune couldn't identify whether the intended development would be residential, non-residential or mixed use. Not included in our residential calculations.|

### Fields in the data
#### city_zoning_sizes.csv
City-level zoning statistics, derived from city parcel data. Note that each standardized zoning type may appear more than once in each city, if more than one local code has been determined to match the standardized zoning type. Data collected in 2019.

|Column name|Format|Description|
|---|---|---|
|city|string|City name|
|zoning_code_orig|string|A city's original zoning code. Varies widely by city.|
|zoning_cat_general_2|string|Star Tribune standardized zoning type. (see above)|
|min_lot_size_per_unit|integer|The zone's minimum lot size per residential unit, in square feet|
|pct_total_land_area|float|Percentage of city's total land area that has this zoning type.|
|pct_res_land_area|float|Percentage of city's total residential land area that has this zoning type. (Calculated for non-residential zoning types too, but probably not useful for those.)|
|zone_area_acres|float|Area the city with this zoning code, in acres.|


#### county_parcel_zones_dissolved.csv and county_parcel_zones_dissolved.shp

A parcel-level dataset including a standardized zoning type, block group identifiers, year built and property values, generated from county parcel data maintained by [MetroGIS](https://www.metrogis.org/how-do-i-get/parcel-data.aspx) and U.S. Census TIGER files. Some counties fill out some fields more completely than other counties do. Shapefile field names are the same as CSV field names, but clipped to 10 characters. We have filtered out stacked parcels (often in areas with mobile homes, sometimes with condos) to avoid double-counting areas. Some parcel IDs appear more than once in the data if they cross city boundaries. Parcel data retrieved November 2019. Note: County-provided and city-provided parcel data will not always match depending on when the data was acquired and differences in city and county data maintenance practices.

|Column name|Format|Description|
|---|---|---|
|county|string|County name|
|cty_fips|string|Census 3-digit county FIPS code. Example: 053|
|tract|string|Census tract ID. Example: 026605|
|blk_grp|integer|Census block group ID. Example: 1|
|geoid|string|Full Census GEOID of block group, including state code. Example: 270530266051|
|city_name|string|City name|
|zoning_type|string|Star Tribune standardized zoning type. (see above)|
|area_sq_feet|float|Area of the parcel, in square feet|
|est_bldg_value|integer|Estimated building value|
|est_land_value|integer|Estimated land value|
|est_total_value|integer|Estimated combined value for land and buildings|
|fin_sq_ft|integer|Finished square feet on this parcel|
|year_built|integer|TK TK |
|zone_area_acres|float|Area of the parcel, in acres. Derived from geometry.|
